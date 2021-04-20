# Authorize a RAM user to manage a data transformation task

This topic describes how to create and authorize a Resource Access Management \(RAM\) user to manage a data transformation task.

You can use your Alibaba Cloud account to authorize a RAM user to manage a data transformation task.

-   Create, delete, or modify a data transformation task.
-   Read data from a source Logstore to preview a data transformation task.

**Note:** An authorized RAM user can manage a data transformation task in the Log Service console. This authorization of a RAM user is different from the authorization to access the data in a Logstore during a data transformation task. The AccessKey pair of the current RAM user may be used to manage a data transformation task and access the data in a Logstore during a data transformation task. In this case, you must combine the content of the permission policy in this topic with the content of the permission policy in [Configure an AccessKey pair for a RAM user to access a source Logstore and a destination Logstore](/intl.en-US/Data Transformation/Authorize Log Service/Configure an AccessKey pair for a RAM user to access a source Logstore and a destination
         Logstore.md).

You can authorize a RAM user to transform data in Log Service by using one of the following modes:

-   Simple mode: You can grant full access permissions on Log Service to the RAM user. You do not need to modify the related parameters.
-   Custom mode: You can customize a permission policy to grant only required permissions to the RAM user. This mode requires complex configurations and provides fine-grained access control.

## Simple mode

Use your Alibaba Cloud account to log on to the [RAM console](https://ram.console.aliyun.com/). Attach the AliyunRAMReadOnlyAccess policy and AliyunLogFullAccess policy to the RAM user. For more information, see [Create a RAM user and authorize the RAM user to access Log Service](/intl.en-US/Developer Guide/Access control RAM/Create a RAM user and authorize the RAM user to access Log Service.md).

## Custom mode

1.  Log on to the [RAM console](https://ram.console.aliyun.com/).

2.  Create a policy.

    1.  In the left-side navigation pane, choose **Permissions** \> **Policies**.

    2.  On the Policies page, click **Create Policy**.

    3.  On the Create Custom Policy page, set the required parameters, and then click **OK**. The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |**Policy Name**|The name of the policy.|
        |**Configuration Mode**|Select **Script**.|
        |**Policy Document**|Replace the content in the editor with the following script. Replace <project name\> with the name of the project in which a data transformation rule is created. Replace <Logstore name\> with the name of the related Logstore.

**Note:** If you want to use the AccessKey pair of the current RAM user to read and write Logstore data, you must add the related policy to the following sample script. For more information, see [Configure an AccessKey pair for a RAM user to access a source Logstore and a destination Logstore](/intl.en-US/Data Transformation/Authorize Log Service/Configure an AccessKey pair for a RAM user to access a source Logstore and a destination
         Logstore.md).

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

4.  Grant the required permissions to the RAM user.

    1.  In the left-side navigation pane, choose **Identities** \> **Users**.

    2.  On the Users page, find the RAM user, and click **Add Permissions** in the Actions column.

    3.  In the Add Permissions panel, click **System Policy** in the Select Policy section, and then select the **AliyunRAMReadOnlyAccess** policy.

    4.  Click **Custom Policy** in the Select Policy section, and then select the policy that you created in [Step 2](#step_ztk_st8_z2e).

    5.  Click **OK**.


