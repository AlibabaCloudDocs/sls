# Authorize a RAM user to manage alerts

This topic describes how to authorize a Resource Access Management \(RAM\) user to manage alerts.

You can authorize a RAM user by attaching one of the following permission policies to the RAM user:

-   Simple mode: You can grant full access permissions on your data in Log Service to the RAM user. You do not need to modify the related parameters.
    -   Use your Alibaba Cloud account to log on to the [RAM console](https://ram.console.aliyun.com/) and attach the AliyunLogFullAccess policy to a RAM user. Then, the RAM user has full access permissions on your data in Log Service. For more information, see [Create a RAM user and authorize the RAM user to access Log Service](/intl.en-US/Developer Guide/Access control RAM/Create a RAM user and authorize the RAM user to access Log Service.md).
    -   You can authorize a RAM user to query data in Metricstores or Logstores. This requires that the RAM user assumes a built-in role or custom role. In these cases, you must grant the RAM user the `ram:PassRole` permission of the role. The following script shows the permission policy:

        ```
        {
             "Action": "ram:PassRole",
             "Effect": "Allow",
             "Resource": "acs:ram::Alibaba Cloud account: role /Role ARN"
         }
        ```

        For more information about how to create a built-in role or authorize a custom role, see [Configure access control policies]().

-   Custom mode: You can customize a permission policy. This can be used to grant only required permissions to the RAM user. This mode requires complex configurations and provides fine-grained access control.

    This topic describes how to authorize a RAM user by using a custom policy.


1.  Log on to the [RAM console](https://ram.console.aliyun.com/).

2.  Create a policy.

    1.  In the left-side navigation pane, choose **Permissions** \> **Policies**.

    2.  On the Policies page, click **Create Policy**.

    3.  On the Create Custom Policy page, set the required parameters, and then click **OK**. The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |**Policy Name**|The name of the policy.|
        |**Configuration Mode**|Select **Script**.|
        |**Policy Document**|The content of the policy. Replace the content in the editor with the following script.         -   Replace Project name with the actual project name.
        -   `sls-alert-*` is the project of the global alert center within the current Alibaba Cloud account. Format: `sls-alert-${uid}-${region}`. The project contains all alert handling reports.
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
     "Resource": [
       "acs:log:*:*:project/Project name/logstore/internal-alert-history",
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
     "Resource": "acs:log:*:*:project/Project name/dashboard/*"
   },
   {
     "Effect": "Allow",
     "Action": [
       "log:*"
     ],
     "Resource": "acs:log:*:*:project/Project name/job/*"   
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
     "Action": "ram:PassRole",
     "Effect": "Allow",
     "Resource": "acs:ram::Alibaba Cloud account: role /Role ARN"
   }
 ]
}
        ``` |

3.  Create a RAM user.

    For more information about how to create a RAM user, see [Create a RAM user](/intl.en-US/Developer Guide/Access control RAM/Create a RAM user and authorize the RAM user to access Log Service.mdsection_wz1_e6j_bdy).

4.  Grant the required permissions to the RAM user.

    1.  In the left-side navigation pane, choose **Identities** \> **Users**.

    2.  On the Users page, find the RAM user, and click **Add Permissions** in the Actions column.

    3.  In the Add Permissions panel, click **System Policy** in the Select Policy section, and then select the **AliyunRAMReadOnlyAccess** policy.

    4.  In the Select Policy section, click **Custom Policy**, select the policy that you created in [Step 2](#task_2045213/step_0ac_f3o_w3i), and then click **OK**.

    5.  Click **Complete**.


