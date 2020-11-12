# 授权RAM用户告警操作权限

使用阿里云主账号给RAM用户授权，可实现通过RAM用户进行告警操作。本文档介绍如何创建阿里云RAM用户并授予其告警操作权限。

您可以通过如下两种方式给RAM用户授予日志服务告警操作权限。

-   极简授权：授予RAM用户日志服务的全部操作权限（AliyunLogFullAccess）。更多信息，请参见[创建RAM用户及授权](/cn.zh-CN/开发指南/访问控制RAM/创建RAM用户及授权.md)。
-   自定义权限策略：仅授予RAM用户创建、修改告警的权限。本文以此方式为例进行授权操作。

1.  登录[RAM 控制台](https://ram.console.aliyun.com/)。

2.  创建权限策略。

    1.  在左侧导航栏中，选择**权限管理** \> **权限策略管理**。

    2.  单击**创建权限策略**。

    3.  在新建自定义权限策略页面中，配置如下参数，并单击**确定**。

        |参数|说明|
        |--|--|
        |**策略名称**|配置策略名称。|
        |**配置模式**|选择**脚本配置**。|
        |**策略内容**|将配置框中的原有脚本替换为如下内容。 其中，请根据实际情况替换Project名称。

        ```
{
  "Version": "1",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "log:CreateLogStore",
        "log:CreateIndex",
        "log:UpdateIndex"
      ],
      "Resource": "acs:log:*:*:project/Project名称/logstore/internal-alert-history"
    },
    {
      "Effect": "Allow",
      "Action": [
        "log:CreateDashboard",
        "log:CreateChart",
        "log:UpdateDashboard"
      ],
      "Resource": "acs:log:*:*:project/Project名称/dashboard/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "log:*"
      ],
      "Resource": "acs:log:*:*:project/Project名称/job/*"
    }
  ]
}
        ``` |

3.  创建RAM用户。

    如何创建RAM用户，请参见[创建RAM用户](/cn.zh-CN/开发指南/访问控制RAM/创建RAM用户及授权.mdsection_wz1_e6j_bdy)。如果已有可用的RAM用户，请跳过此步骤。

4.  为RAM用户授权。

    1.  在左侧导航栏中，选择**人员管理** \> **用户**。

    2.  找到目标RAM用户，单击**添加权限**。

    3.  单击**自定义策略**，选中[步骤2](#step_pzo_yvb_cmr)中创建的策略，单击**确定**。

    4.  单击**完成**。


