# 授予RAM用户Scheduled SQL操作权限

当您使用RAM用户操作Scheduled SQL时，需参见本文中的步骤完成授权。

已创建RAM用户。具体操作，请参见[创建RAM用户](/cn.zh-CN/开发指南/访问控制RAM/创建RAM用户及授权.md)。

## 操作步骤

1.  登录[RAM控制台](https://ram.console.aliyun.com/)。

2.  创建权限策略。

    1.  在左侧导航栏中，单击**权限管理** \> **权限策略管理**。

    2.  单击**创建权限策略**。

    3.  在新建自定义权限策略页面中，配置如下参数，并单击**确定**。

        |参数|说明|
        |--|--|
        |**策略名称**|配置策略名称。|
        |**配置模式**|选择**脚本配置**。|
        |**策略内容**|将配置框中的原有脚本替换为如下内容。 请根据实际情况替换脚本中的Project名称和SQL作业名称。

**说明：** 如果您要使用RAM用户配置Scheduled SQL作业告警，还需授予RAM用户告警操作权限。更多信息，请参见[授予RAM用户告警操作权限](/cn.zh-CN/告警/告警（新版）/授予RAM用户告警操作权限.md)。

        ```
{
    "Version": "1",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "log:GetJobInstance",
                "log:ModifyJobInstance",
                "log:ListJobInstance"
            ],
            "Resource": "acs:log:*:*:project/Project名称/job/SQL作业名称/jobinstance/*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "log:*"
            ],
            "Resource": "acs:log:*:*:project/Project名称/job/*"
        },
        {
            "Effect": "Allow",
            "Action": "log:*",
            "Resource": "acs:log:*:*:project/Project名称/jobs"
        },
        {
            "Effect": "Allow",
            "Action": ["ram:PassRole","ram:GetRole","ram:ListRoles"],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "log:CreateLogStore",
                "log:CreateIndex",

"log:UpdateIndex"
            ],
            "Resource": [
                "acs:log:*:*:project/sls-alert-*/logstore/internal-alert-center-log"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "log:CreateDashboard",
                "log:CreateChart",
                "log:UpdateDashboard"
            ],
            "Resource": [
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
        }
    ]
}
        ``` |

3.  为RAM用户授权。

    1.  在左侧导航栏中，单击**人员管理** \> **用户**。

    2.  找到目标RAM用户，单击**添加权限**。

    3.  单击**自定义策略**，选中[步骤2](#step_l4m_48a_akf)中创建的策略。

    4.  单击**确定**。


