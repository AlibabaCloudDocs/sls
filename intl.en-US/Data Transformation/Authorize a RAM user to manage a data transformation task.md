# Authorize a RAM user to manage a data transformation task

This topic describes how to create a RAM user and authorize the RAM user to manage a data transformation task.

You can use your Alibaba Cloud account to authorize a RAM user to manage a data transformation task.

-   Create, delete, and modify a data transformation task.
-   Read data from a source Logstore to preview a data transformation task.

**Note:** An authorized RAM user can manage a data transformation task in the Log Service console. The authorization method is different from the authorization method that you use to create a data transformation task. The latter method uses AccessKey pairs of a RAM user to manage a data transformation task. For more information, see [Configure an AccessKey pair for a RAM user to access a source Logstore and a destination Logstore](/intl.en-US/Data Transformation/Authorize Log Service/Configure an AccessKey pair for a RAM user to access a source Logstore and a destination
         Logstore.md).

You can authorize a RAM user to transform data in Log Service in one of the following modes:

-   Simple mode: In this mode, you can grant full access permissions on Log Service to the RAM user. You do not need to modify relevant parameters.
-   Custom mode: You can customize the permission policy to grant only required permissions to the RAM user. This mode requires complex configurations and provides finer-grained access control.

## Simple mode

Use your Alibaba Cloud account to log on to the [RAM console](https://ram.console.aliyun.com/)[RAM console](https://partners-intl.console.aliyun.com/#/ram). Attach the AliyunRAMReadOnlyAccess policy and AliyunLogFullAccess policy to the RAM user. For more information, see [Create a RAM user and authorize the RAM user to access Log Service](/intl.en-US/Developer Guide/Access control RAM/Create a RAM user and authorize the RAM user to access Log Service.md).

## Custom mode

1.  Log on to the [RAM console](https://ram.console.aliyun.com/)[RAM console](https://partners-intl.console.aliyun.com/#/ram).

2.  Create a policy.

    1.  In the left-side navigation pane, choose **Permissions** \> **Policies**.

    2.  On the Policies page, click **Create Policy**.

    3.  On the Create Custom Policy page, set the parameters, and then click **OK**. The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |**Policy Name**|The name of the policy.|
        |**Configuration Mode**|Select **Script**.|
        |**Policy Document**|Replace the content in the editor with the following script: Replace the project name in <project name\> with the name of the Log Service project. Replace the Logstore name in <Logstore name\> with the name of the source Logstore.

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
            "Resource":"acs:log:*:*:project/<project name>/logstore/internal-etl-log"
        },
        {
            "Action":[
                "log:List*"
            ],
            "Resource":"acs:log:*:*:project/<project name>/logstore/*",
            "Effect":"Allow"
        },
        {
            "Action":[
                "log:Get*",
                "log:List*"
            ],
            "Resource":[
                "acs:log:*:*:project/<project name>/logstore/<Logstore name>"
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
            "Resource":"acs:log:*:*:project/<project name>/dashboard/internal-etl-insight"
        },
        {
            "Effect":"Allow",
            "Action":"log:CreateDashboard",
            "Resource":"acs:log:*:*:project/<project name>/dashboard/*"
        },
        {
            "Effect":"Allow",
            "Action":[
                "log:*"
            ],
            "Resource":"acs:log:*:*:project/<project name>/job/*"
        },
        {
            "Effect":"Allow",
            "Action":[
                "log:*"
            ],
            "Resource":"acs:log:*:*:project/<project name>/jobschedule/*"
        }
    ]
}
        ``` |

3.  Create a RAM user. For more information, see [Create a RAM user](/intl.en-US/Developer Guide/Access control RAM/Create a RAM user and authorize the RAM user to access Log Service.mdsection_wz1_e6j_bdy).

4.  Authorize the RAM user.

    1.  In the left-side navigation pane, choose **Identities** \> **Users**.

    2.  On the Users page, find the RAM user, and click **Add Permissions** in the Actions column.

    3.  Click the **System Policy** tab under the Select Policy field and select **AliyunRAMReadOnlyAccess**.

    4.  Click the **Custom Policy** tab under the Select Policy field, and then select the policy that you created in [Step 2](#step_ztk_st8_z2e).

    5.  Click **OK**.


