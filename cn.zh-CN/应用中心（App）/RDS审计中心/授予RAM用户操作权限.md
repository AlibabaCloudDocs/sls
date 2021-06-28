# 授予RAM用户操作权限

本文介绍如何授予阿里云RAM用户操作RDS审计中心的权限。

已创建RAM用户。具体操作，请参见[创建RAM用户](/cn.zh-CN/开发指南/访问控制RAM/创建RAM用户及授权.md)。

您可以通过如下两种方式给RAM用户授予RDS审计中心的操作权限。

-   极简授权：权限较大，操作简单。
-   自定义权限策略：权限精细，配置复杂。

## 极简授权

使用阿里云账号登录[RAM控制台](https://ram.console.aliyun.com/)，为RAM用户授予全部管理权限（AliyunLogFullAccess、AliyunRAMFullAccess）。具体操作，请参见[为RAM用户授权](/cn.zh-CN/用户管理/授权管理/为RAM用户授权.md)。

## 自定义权限策略

1.  使用阿里云账号登录[RAM控制台](https://ram.console.aliyun.com/)。

2.  创建权限策略。

    1.  在左侧导航栏中，选择**权限管理** \> **权限策略管理**。

    2.  单击**创建权限策略**。

    3.  在新建自定义权限策略页面中，配置如下参数，并单击**确定**。

        |参数|说明|
        |--|--|
        |**策略名称**|配置策略名称。|
        |**配置模式**|选择**脚本配置**。|
        |**策略内容**|将配置框中的原有脚本替换为如下内容。 您可以授予RAM用户使用RDS审计中心的只读权限或读写权限，具体权限策略说明如下：

        -   只读权限（只允许查看RDS审计中心中的各个页面。）

            ```
{
    "Version": "1",
    "Statement": [
        {
            "Action": [
                "rds:DescribeSqlLogInstances",
                "rds:DisableSqlLogDistribution"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Effect": "Allow",
            "Action": [
                "log:CreateLogStore",
                "log:CreateIndex",
                "log:UpdateIndex",
                "log:ListLogStores",
                "log:GetLogStore",
                "log:GetLogStoreLogs",
                "log:CreateDashboard",
                "log:CreateChart",
                "log:UpdateDashboard"
            ],
            "Resource": [
                "acs:log:*:*:project/sls-alert-*/logstore/*",
                "acs:log:*:*:project/sls-alert-*/dashboard/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "log:CreateProject"
            ],
            "Resource": [
                "acs:log:*:*:project/sls-alert-*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "log:GetLogStore",
                "log:ListLogStores",
                "log:GetIndex",
                "log:GetLogStoreHistogram",
                "log:GetLogStoreLogs",
                "log:GetDashboard",
                "log:ListDashboard",
                "log:ListSavedSearch"
            ],
            "Resource": [
                "acs:log:*:*:project/*/logstore/*",
                "acs:log:*:*:project/*/dashboard/*",
                "acs:log:*:*:project/*/savedsearch/*"
            ]
        },
        {
            "Action": [
                "ram:GetRole"
            ],
            "Resource": "acs:ram:*:*:role/aliyunlogarchiverole",
            "Effect": "Allow"
        }
    ]
}
            ```

        -   读写权限（允许操作RDS审计中心中的各个功能。）

            ```
{
    "Version": "1",
    "Statement": [
        {
            "Action": [
                "rds:DescribeSqlLogInstances",
                "rds:DisableSqlLogDistribution",
                "rds:DisableSqlLogDistribution",
                "rds:EnableSqlLogDistribution",
                "rds:ModifySQLCollectorPolicy"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Effect": "Allow",
            "Action": [
                "log:CreateLogStore",
                "log:CreateIndex",
                "log:UpdateIndex",
                "log:ListLogStores",
                "log:GetLogStore",
                "log:GetLogStoreLogs",
                "log:CreateDashboard",
                "log:CreateChart",
                "log:UpdateDashboard"
            ],
            "Resource": [
                "acs:log:*:*:project/sls-alert-*/logstore/*",
                "acs:log:*:*:project/sls-alert-*/dashboard/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "log:CreateProject"
            ],
            "Resource": [
                "acs:log:*:*:project/sls-alert-*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "log:GetLogStore",
                "log:ListLogStores",
                "log:GetIndex",
                "log:GetLogStoreHistogram",
                "log:GetLogStoreLogs",
                "log:GetDashboard",
                "log:ListDashboard",
                "log:ListSavedSearch",
                "log:CreateLogStore",
                "log:CreateIndex",
                "log:UpdateIndex",
                "log:ListLogStores",
                "log:GetLogStore",
                "log:GetLogStoreLogs",
                "log:CreateDashboard",
                "log:CreateChart",
                "log:UpdateDashboard",
                "log:UpdateLogStore"
            ],
            "Resource": [
                "acs:log:*:*:project/*/logstore/*",
                "acs:log:*:*:project/*/dashboard/*",
                "acs:log:*:*:project/*/savedsearch/*"
            ]
        },
        {
            "Action": [
                "log:SetGeneralDataAccessConfig"
            ],
            "Resource": [
                "acs:log:*:*:resource/sls.general_data_access.rds.global_conf.single_account_channel/record"
            ],
            "Effect": "Allow"
        },
        {
            "Action": "ram:CreateServiceLinkedRole",
            "Resource": "*",
            "Effect": "Allow",
            "Condition": {
                "StringEquals": {
                    "ram:ServiceName": "audit.log.aliyuncs.com"
                }
            }
        },
        {
            "Action": [
                "ram:*"
            ],
            "Resource": [
                "acs:ram:*:*:role/aliyunlogarchiverole",
                "acs:ram:*:*:policy/AliyunLogArchiveRolePolicy"
            ],
            "Effect": "Allow"
        }
    ]
}
            ``` |

3.  为RAM用户授权。

    1.  在左侧导航栏中，选择**人员管理** \> **用户**。

    2.  找到目标RAM用户，单击**添加权限**。

    3.  在添加权限面板的选择权限区域，单击**自定义策略**，选中步骤[2](#step_4td_146_5cn)中创建的策略。

    4.  单击**确定**。


