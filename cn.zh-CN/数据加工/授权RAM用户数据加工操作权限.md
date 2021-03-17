# 授权RAM用户数据加工操作权限

本文档介绍如何创建阿里云RAM用户并授予其数据加工操作权限。

使用阿里云主账号给RAM用户授权，可以实现通过RAM用户进行数据加工操作。

-   创建、删除、修改数据加工任务。
-   读取源Logstore数据进行加工任务预览。

**说明：** 本文对RAM用户授权仅用于实现通过RAM用户登录日志服务控制台进行数据加工操作。该授权与创建加工任务时所需的RAM用户访问密钥（AccessKey）不同，获取数据加工任务的访问密钥请参见[配置访问密钥](/cn.zh-CN/数据加工/配置访问授权/配置访问密钥.md)。

您可以通过如下两种方式给RAM用户授予日志服务数据加工功能权限。

-   极简授权：权限较大，操作简单。
-   自定义权限策略：权限精细，配置复杂。

## 极简授权

使用阿里云主账号登录[RAM控制台](https://ram.console.aliyun.com/)[RAM控制台](https://partners-intl.console.aliyun.com/#/ram)，为RAM用户授予RAM读权限（AliyunRAMReadOnlyAccess）和全部管理权限（AliyunLogFullAccess）。更多信息，请参见[创建RAM用户及授权](/cn.zh-CN/开发指南/访问控制RAM/创建RAM用户及授权.md)。

## 自定义权限策略

1.  登录[RAM 控制台](https://ram.console.aliyun.com/)[RAM控制台](https://partners-intl.console.aliyun.com/#/ram)。

2.  创建权限策略。

    1.  在左侧导航栏中，单击**权限管理** \> **权限策略管理**。

    2.  单击**新建权限策略**。

    3.  在新建自定义权限策略页面中，配置如下参数，并单击**确定**。

        |参数|说明|
        |--|--|
        |**策略名称**|配置策略名称。|
        |**配置模式**|选择**脚本配置**。|
        |**策略内容**|将配置框中的原有脚本替换为如下内容。 请将<Project名称\>和<Logstore名称\>替换为进行数据加工的日志服务Project名称和源Logstore名称。

        ```
{
    "Version":"1",
    "Statement":[
        {
            "Effect":"Allow",
            "Action":[
                "log:CreateLogStore",
                "log:CreateIndex",
                "log:UpdateIndex",
                "log:Get*"
            ],
            "Resource":"acs:log:*:*:project/<Project名称>/logstore/internal-etl-log"
        },
        {
            "Action":[
                "log:List*"
            ],
            "Resource":"acs:log:*:*:project/<Project名称>/logstore/*",
            "Effect":"Allow"
        },
        {
            "Action":[
                "log:Get*",
                "log:List*"
            ],
            "Resource":[
                "acs:log:*:*:project/<Project名称>/logstore/<Logstore名称>"
            ],
            "Effect":"Allow"
        },
        {
            "Effect":"Allow",
            "Action":[
                "log:GetDashboard",
                "log:CreateDashboard",
                "log:UpdateDashboard"
            ],
            "Resource":"acs:log:*:*:project/<Project名称>/dashboard/internal-etl-insight"
        },
        {
            "Effect":"Allow",
            "Action":"log:CreateDashboard",
            "Resource":"acs:log:*:*:project/<Project名称>/dashboard/*"
        },
        {
            "Effect":"Allow",
            "Action":[
                "log:*"
            ],
            "Resource":"acs:log:*:*:project/<Project名称>/job/*"
        },
        {
            "Effect":"Allow",
            "Action":[
                "log:*"
            ],
            "Resource":"acs:log:*:*:project/<Project名称>/jobschedule/*"
        }
    ]
}
        ``` |

3.  创建RAM用户。更多信息，请参见[创建RAM用户](/cn.zh-CN/开发指南/访问控制RAM/创建RAM用户及授权.mdsection_wz1_e6j_bdy)。

4.  为RAM用户授权。

    1.  在左侧导航栏中，单击**人员管理** \> **用户**。

    2.  找到目标RAM用户，单击**添加权限**。

    3.  单击**系统策略**，选中**AliyunRAMReadOnlyAccess**。

    4.  单击**自定义策略**，选中[步骤2](#step_ztk_st8_z2e)中创建的策略。

    5.  单击**确定**。


