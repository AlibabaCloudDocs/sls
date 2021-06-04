# Configure access control policies

To monitor data from different projects and regions that belongs to multiple Alibaba Cloud accounts, you must be granted the required permissions. For example, you must be granted the read permissions from the related Logstores and Metricstores. This topic describes how to grant related access control permissions.

## Authorization method

Log Service provides default roles, built-in roles, and custom roles to obtain access permissions for Logstores and Metricstores.

|Authorization method|Scenario|
|--------------------|--------|
|**Default**|Monitor data from all Logstores and Metricstores of the same project in which the monitoring rule resides.|
|**Built-in Role**|Monitor data from Logstores and Metricstores of multiple projects that belongs to an Alibaba Cloud account. The data is stored in different projects and regions.|
|**Custom Role**|Monitor data from multiple Alibaba Cloud Accounts or from all projects that reside in different region of an Alibaba Cloud account. Custom role requires complex configurations and provides fine-grained access control.|

## Default authorization method

Monitor data from all Logstores and Metricstores of a project. When you [Create an alert monitoring rule](/intl.en-US/Alerting/Alerting (New)/Alert monitoring/Create an alert monitoring rule for logs.md), select **Default** from the **Authorization** drop-down list in the **Advanced Settings** of **Query Statistics**.

## Built-in role authorization method

To monitor data from Logstores and Metricstores of multiple projects that belong to an Alibaba Cloud account, the account must assume the AliyunSLSAlertMonitorRole built-in role. This way, you can read data from source Logstores.

The following steps describe how to grant the AliyunSLSAlertMonitorRole built-in role when you [Create an alert monitoring rule](/intl.en-US/Alerting/Alerting (New)/Alert monitoring/Create an alert monitoring rule for logs.md).

1.  When you create an alert monitoring rule, click the search bar of the **Query Statistics** field.

2.  In the Query Statistics dialog box, click **Advanced Settings**.

3.  Select **Built-in Role** from the **Authorization** drop-down list.

4.  Click **Authorize** when you configure the built-in role for the first time.

    **Note:** RAM users must be granted the required permissions by Alibaba Cloud accounts.

    ![neizih](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2909872261/p248932.png)

5.  On the **Cloud Resource Access Authorization** page, click **Confirm Authorization Policy**.


## Authorize a custom role to monitor data of an Alibaba Cloud account

You can create a custom role to monitor data from all Logstores and Metricstores of multiple projects that belong to an Alibaba Cloud account.

1.  Log on to the [RAM console](https://ram.console.aliyun.com/).

2.  Create a policy to monitor alert data.

    1.  In the left-side navigation pane, choose **Permissions** \> **Policies**.

    2.  On the Policies page, click **Create Policy**. The Create Custom Policy page appears.

    3.  On the Create Custom Policy page, set the parameters, and then click **OK**. The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |**Policy Name**|The name of the policy. For example, sls-alert-monitor-1-policy.|
        |**Configuration Mode**|Select **Script**.|
        |**Policy Document**|The content of the policy. Replace the content in the editor with the following script.         ```
{   "Version": "1",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "log:ListProject",
                "log:GetLogStore",
                "log:ListLogStores",
                "log:GetLogStoreLogs",
                "log:GetResourceRecord",
                "log:ListResourceRecords",
                "log:ListResources",
                "log:GetResource"
            ],
            "Resource": [
                "acs:log:*:*:*" 
           ]
        }
    ]
}
        ```

If you want to create a role that can create only alert monitoring rules in a specific project, you can replace the value of the Resource field with the actual project name. The value must be in the `acs:log:*:*:project/project name` format. |

    4.  Click **OK**.

3.  If no RAM role is available, create a RAM role to manage alerts.

    For more information, see [Create a RAM role](/intl.en-US/Developer Guide/Access control RAM/Assign a RAM role to an Alibaba Cloud account.md).

4.  Attach alert-related policies to the RAM role.

    1.  In the left-side navigation pane, click **RAM Roles**.

    2.  On the RAM Roles page, find the RAM role, and click **Add Permissions** in the Actions column.

    3.  On the **Custom Policy** tab, select the policy that you created in [Step 2](#step_syv_9qy_jyl), such as sls-alert-monitor-1-policy, and click **OK**.

    4.  Confirm the authorization result, and then click **Complete**.

    5.  On the Trust Policy Management tab, click **Edit Trust Policy**.

        Add log.aliyuncs.com to the Service field. This policy indicates that a temporary authorization token is obtained to manage Log Service resources that belong to a Alibaba Cloud account.

        ```
        {
            "Statement": [
                {
                    "Action": "sts:AssumeRole",
                    "Effect": "Allow",
                    "Principal": {
                        "Service": [
                            "log.aliyuncs.com"
                        ]
                    }
                }
            ],
            "Version": "1"
        }
        ```

5.  Obtain the ARN of the RAM role.

    1.  Find the destination RAM role and click the RAM role name.

    2.  Obtain the ARN in the basic information section of the role. For example, `acs:ram::13234:role/logrole`.

6.  Enter the ARN on the Advanced Settings tab of the Query Statistics dialog box.

    ![Authorize a custom role to monitor data of an Alibaba Cloud account](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2909872261/p254393.png)


## Authorize a custom role to monitor data from multiple Alibaba Cloud accounts

You can create a custom role to monitor data from Logstores and Metricstores of multiple Alibaba Cloud accounts. For example, an alert is created by using Alibaba Cloud Account A and you want to monitor data from Logstores and Metricstores of Alibaba Cloud Account B. The following steps describe how to grant the permissions.

1.  Log on to the [RAM console](https://ram.console.aliyun.com/) by using Alibaba Cloud Account B.

2.  Create a policy for the custom role.

    For more information, see Step 2 in [Authorize a custom role to monitor data of an Alibaba Cloud account](#section_9zg_8ex_3cj). For example, the name of the policy that you create is sls-alert-monitor-2-policy.

3.  If no RAM role is available, create a RAM role to manage alerts.

    For more information, see [Create a RAM role](/intl.en-US/Developer Guide/Access control RAM/Assign a RAM role to an Alibaba Cloud account.md).

4.  Grant alert-related policies to the RAM role.

    1.  In the left-side navigation pane, click **RAM Roles**.

    2.  On the RAM Roles page, find the RAM role, and click **Add Permissions** in the Actions column.

    3.  On the **Custom Policy** tab, select the policy that you created in [Step 2](#step_8ln_m0o_e8e), such as sls-alert-monitor-2-policy, and click **OK**.

    4.  Confirm the authorization result, and then click **Complete**.

    5.  On the Trust Policy Management tab, click **Edit Trust Policy**.

        Add Alibaba Cloud Account A to the Service field in the format of Alibaba Cloud account ID@log.aliyuncs.com. Replace the Alibaba Cloud account ID with the actual ID number. You can check the Alibaba Cloud ID in **Account Management** \> **Security Settings**. This policy indicates that Alibaba Cloud Account A has the permissions to monitor data from Logstores and Metricstores of Alibaba Cloud Account B by using a temporary authorization token.

        ```
        {
            "Statement": [
                {
                    "Action": "sts:AssumeRole",
                    "Effect": "Allow",
                    "Principal": {
                        "Service": [
                            "ID of Alibaba Cloud account A@log.aliyuncs.com",
                            "log.aliyuncs.com"
                        ]
                    }
                }
            ],
            "Version": "1"
        }
        ```

5.  Obtain the ARN of the RAM role.

    Obtain the ARN in the Basic Information section of the role, for example, acs:ram::13234:role/logrole.

6.  Enter the ARN on the Advanced Settings tab of the Query Statistics dialog box.

    ![Authorize a custom role to monitor data from multiple Alibaba Cloud accounts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2909872261/p254393.png)


