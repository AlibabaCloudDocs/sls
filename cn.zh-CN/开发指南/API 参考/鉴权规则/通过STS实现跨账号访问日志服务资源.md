# 通过STS实现跨账号访问日志服务资源

阿里云账号可以通过创建并授权用户角色的方式赋予其他云账号一定的资源权限，其他云账号扮演该角色，并为其名下的RAM用户授予AssumeRole权限之后，其他云账号或其子账号可以通过访问STS接口获取临时AK和Token函数，调用日志服务API接口。

出于业务隔离或项目外包等需求，云账号A希望将部分日志服务业务授权给云账号B，由云账号B操作维护这部分业务。基本需求如下：

-   云账号B拥有向企业A的日志服务中写入数据和使用消费组的权限。
-   云账号B的指定RAM用户也拥有日志服务的写入和消费组权限。
-   云账号B可获取STS临时凭证，访问日志服务API接口，详情请参见[STS](/cn.zh-CN/API参考/API 参考（STS）/什么是STS.md)。

## 赋权过程概述

1.  云账号A创建RAM角色，并指定云账号B扮演该角色并赋予日志服务的指定权限。
2.  云账号B创建RAM用户B1，并为其赋予`AliyunSTSAssumeRoleAccess`（调用STS AssumeRole接口）的系统策略。
3.  RAM账号B1调用STS AssumeRole接口访问日志服务API接口，操作日志服务资源。

## 步骤1：云账号A为云账号B创建RAM角色并授权

云账号A创建RAM角色，并指定云账号B扮演该角色并为角色赋予日志服务的指定权限。

可以通过RAM控制台创建RAM角色，请参见[创建RAM用户及授权](/cn.zh-CN/开发指南/访问控制RAM/创建RAM用户及授权.md)或通过RAM的API CreateRole创建RAM角色，请参见[CreateRole](https://api.aliyun.com/?product=Ram&api=CreateRole#/?product=Ram&api=CreateRole)，以下以控制台创建为例进行详细步骤说明。

1.  云账号A登录[RAM控制台](https://ram.console.aliyun.com/)。

2.  云账号A创建RAM角色，并指定云账号B扮演该角色。

    1.  在左侧导航栏，单击**RAM角色管理**。

    2.  在RAM角色管理页面，单击**创建RAM角色**。

    3.  选中可信实体类型为**阿里云账号**，单击**下一步**。

    4.  输入**角色名称**和**备注**，**选择云账号**为**其他云账号**，填写云账号B的账号ID，单击**完成**。

        **说明：** 将鼠标悬停在控制台右上角头像的位置，单击**安全信息管理**，即可查询主账号ID。

        以上步骤中创建的RAM角色详情如下。

        ```
        {
          "Statement": [
            {
              "Action": "sts:AssumeRole",
              "Effect": "Allow",
              "Principal": {
                "RAM": [
                  "acs:ram::<云账号B的账号ID>:root"
                ]
              }
            }
          ],
          "Version": "1"
        }
        ```

3.  创建权限策略。

    1.  在左侧导航栏的**权限管理**菜单下，单击**权限策略管理**。

    2.  在权限策略管理页面，单击**创建权限策略**。

    3.  在新建自定义权限策略页面，配置如下参数：

        |参数|说明|
        |--|--|
        |策略名称|输入策略名称。|
        |备注|输入您需要备注的内容。|
        |配置模式|选中**脚本配置**。|
        |策略内容|此处配置的权限策略为云账号A授予云账号B的权限内容。 如果仅需要写数据，具体权限如下：

        ```
{
  "Version": "1",
  "Statement": [
    {
      "Action": "log:PostLogStoreLogs",
      "Resource": "*",
      "Effect": "Allow"
    }
  ]
}
        ```

如果需要通过协同消费库获取数据，具体权限如下：

        ```
{
  "Version": "1",
  "Statement": [
    {
      "Action": [
         "log:GetCursorOrData",
         "log:CreateConsumerGroup",
         "log:ListConsumerGroup",
         "log:ConsumerGroupUpdateCheckPoint",
         "log:ConsumerGroupHeartBeat",
         "log:GetConsumerGroupCheckPoint",
         "log:UpdateConsumerGroup"
      ],
      "Resource": "*",
      "Effect": "Allow"
    }
  ]
}
        ```

以上两类资源都是授权指定用户的所有Project和Logstore，如果您需要授权指定Project和Logstore，请参考如下内容：

        -   授权指定Project：`acs:log::\{projectOwnerAliUid\}:project/`。
        -   授权指定Logstore：`acs:log::\{projectOwnerAliUid\}:project/\{projectName\}/logstore/\{logstoreName\}/`。
完整的资源说明请参见[日志服务RAM资源](/cn.zh-CN/开发指南/API 参考/鉴权规则/资源列表.md)。 |

    4.  单击**确定**。

4.  云账号A为RAM角色授权。

    1.  在左侧导航栏，单击**RAM角色管理**。

    2.  在**RAM角色名称**列表下，找到目标RAM角色，单击**添加权限**。

    3.  **权限类型**选择**自定义策略**并在列表中选中[步骤3](#step_skd_d9f_wb3)创建的权限策略，单击**确定**。

    4.  单击**完成**，即完成授权。


## 步骤2：云账号B创建RAM用户B1并授权

云账号B创建RAM用户B1，并为其授予`AliyunSTSAssumeRoleAccess`（调用STS AssumeRole接口）的系统策略。

1.  云账号B登录[RAM控制台](https://ram.console.aliyun.com/)。

2.  在左侧导航栏中，选择**人员管理** \> **用户**。

3.  在用户页面，单击**创建用户**。

4.  在创建用户页面，参考如下说明进行参数配置并单击**确定**。

    |参数|说明|
    |--|--|
    |**用户账号信息**|请输入**登录名称**及**显示名称**。|
    |**访问方式**|选中**控制台密码登录**和**编程访问**。|

5.  返回用户页面，在**用户登录名称/显示名称**列表中，找到目标RAM用户，单击**添加权限**。

6.  在添加权限页面，**被授权主体**会自动填入。选中**系统策略**，在**权限策略名称**列表下，单击需要授予RAM角色的权限策略**AliyunSTSAssumeRoleAccess**，单击**确定**。

7.  单击**完成**。


## 步骤3：RAM账号B1获取STS临时凭证访问日志服务

1.  调用STS AssumeRole接口获取临时AK和Token。更多信息，请参见[AssumeRole](/cn.zh-CN/API参考/API 参考（STS）/操作接口/AssumeRole.md)。

    您可以选择以下方式调用该接口：

    通过STS SDK调用。更多信息，请参见[STS SDK概览](/cn.zh-CN/SDK参考/SDK参考（STS）/STS SDK概览.md)。

2.  调用日志服务接口，关于日志服务SDK请参见[SDK参考](/cn.zh-CN/开发指南/SDK 参考/概述.md)。


## 示例代码

示例代码基于Java SDK，以云账号B通过STS向用户A的Project写入数据为例，详情请下载并参见[示例代码](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/47277/cn_zh/1479281238498/StsSample.java)。

