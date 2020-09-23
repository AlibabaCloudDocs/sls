# Authorize Log Service to assume a custom role

You can authorize Log Service to assume a custom role to read data from a Logstore and write transformed data to one or more Logstores. This topic describes how to authorize Log Service to assume a custom role.

## Authorize Log Service to read data from a source Logstore

1.  Log on to the [RAM console](https://ram.console.aliyun.com/) by using an Alibaba Cloud account.

2.  Create a policy that grants the permission to read data from a source Logstore.

    1.  In the left-side navigation pane, choose **Permissions** \> **Policies**.

    2.  On the Policies page, click **Create Policy**.

    3.  On the Create Custom Policy page, set the parameters, and then click **OK**. The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |**Policy Name**|The name of the policy. In this example, set the value to log-etl-source-reader-1-policy.|
        |**Configuration Mode**|Select **Script**.|
        |**Policy Document**|The content of the policy. Replace the content in the editor with one of the following scripts based on your business requirements.         -   Permission policy that uses exact match

The source project name is log-project-prod. The source Logstore name is access\_log. Replace the regular expressions for exact match based on your business scenarios.

            ```
{
  "Version": "1",
  "Statement": [
    {
      "Action": [
        "log:ListShards",
        "log:GetCursorOrData",
        "log:GetConsumerGroupCheckPoint",
        "log:UpdateConsumerGroup",
        "log:ConsumerGroupHeartBeat",
        "log:ConsumerGroupUpdateCheckPoint",
        "log:ListConsumerGroup",
        "log:CreateConsumerGroup"
      ],
      "Resource": [
        "acs:log:*:*:project/log-project-prod/Logstore/access_log",
        "acs:log:*:*:project/log-project-prod/Logstore/access_log/*"
      ],
      "Effect": "Allow"
    }
  ]
}
            ```

        -   Permission policy that uses fuzzy match

The source project names are log-project-dev-a, log-project-dev-b, and log-project-dev-c. The source Logstore names are app\_a\_log, app\_b\_log, and app\_c\_log. Replace the regular expressions for fuzzy match based on your business scenarios.

            ```
{
  "Version": "1",
  "Statement": [
    {
      "Action": [
        "log:ListShards",
        "log:GetCursorOrData",
        "log:GetConsumerGroupCheckPoint",
        "log:UpdateConsumerGroup",
        "log:ConsumerGroupHeartBeat",
        "log:ConsumerGroupUpdateCheckPoint",
        "log:ListConsumerGroup",
        "log:CreateConsumerGroup"
      ],
      "Resource": [
        "acs:log:*:*:project/log-project-dev-*/Logstore/app_*_log",
    "acs:log:*:*:project/log-project-dev-*/Logstore/app_*_log/*"
      ],
      "Effect": "Allow"
    }
  ]
}
            ```

For more information, see [Use custom policies to grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md). |

3.  For more information, see [Create a RAM role](/intl.en-US/Developer Guide/Access control RAM/Assign a RAM role to an Alibaba Cloud service.md).

4.  Attach the policy to the RAM role.

    1.  On the RAM Roles page, find the RAM role, and click **Add Permissions** in the Actions column.

    2.  In the Select Policy section, select **Custom Policy**. In the Authorization Policy Name list, select the policy that you created in [Step 2](#step_r0m_vqb_wev), and then click OK. In this example, the policy is **log-etl-source-reader-1-policy**.

    3.  Click **Complete**.

5.  Obtain the ARN of the RAM role.

    On the RAM Roles page, click the RAM role. In the Basic Information section of the page that appears, obtain the ARN of the RAM role, for example, acs:ram::13234:role/logrole.


## Authorize Log Service to write data to a destination Logstore

1.  Log on to the [RAM console](https://ram.console.aliyun.com/) by using an Alibaba Cloud account.

2.  Create a policy that grants read/write permissions on a destination Logstore.

    1.  In the left-side navigation pane, choose **Permissions** \> **Policies**.

    2.  On the Policies page, click **Create Policy**.

    3.  On the Create Custom Policy page, set the parameters, and then click **OK**. The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |**Policy Name**|The name of the policy. In this example, set the value to log-etl-target-writer-1-policy.|
        |**Configuration Mode**|Select **Script**.|
        |**Policy Document**|The content of the policy. Replace the content in the editor with one of the following scripts based on your business requirements.         -   Permission policy that uses exact match

The source project name is log-project-prod. The source Logstore name is access\_log\_output. Replace the regular expressions for exact match based on your business scenarios.

            ```
{
  "Version": "1",
  "Statement": [
    {
      "Action": [
        "log:Post*"
      ],
       "Resource": "acs:log:*:*:project/log-project-prod/logstore/access_log_output",
      "Effect": "Allow"
    }
  ]
}
            ```

        -   Permission policy that uses fuzzy match

The source project names are log-project-dev-a, log-project-dev-b, and log-project-dev-c. The source Logstore names are app\_a\_log\_output, app\_b\_log\_output, and app\_c\_log\_output. Replace the regular expressions for fuzzy match based on your business scenarios.

            ```
{
  "Version": "1",
  "Statement": [
    {
      "Action": [
        "log:Post*"
      ],
       "Resource": "acs:log:*:*:project/log-project-dev-*/logstore/app_*_log_output",
      "Effect": "Allow"
    }
  ]
}
            ```

For more information, see [Use custom policies to grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md). |

3.  Create a RAM role. For more information, see [Create a RAM role](/intl.en-US/Developer Guide/Access control RAM/Assign a RAM role to an Alibaba Cloud service.md).

4.  Attach the policy to the RAM role.

    1.  On the RAM Roles page, find the RAM role, and click **Add Permissions** in the Actions column.

    2.  In the Select Policy section, select **Custom Policy**. In the Authorization Policy Name list, select the policy that you created in [Step 2](#step_r0m_vqb_wev), and then click OK. In this example, the policy is **log-etl-source-reader-1-policy**.

    3.  Click **Complete**.

5.  Obtain the ARN of the RAM role.

    On the RAM Roles page, click the RAM role. In the Basic Information section of the page that appears, obtain the ARN of the RAM role, for example, acs:ram::13234:role/logrole.


## Authorize Log Service to write data to a destination Logstore across Alibaba Cloud accounts

If the source and destination Logstores belong to different Alibaba Cloud accounts \(Account A and Account B\), you must authorize Log Service of Account A to access the destination Logstore of Account B by using RAM. This way, you can write the transformed data of Account A to the destination Logstore of Account B.

1.  Use Account B to complete the operations that are described in Section 2. For more information, see [Authorize Log Service to write data to a destination Logstore](#section_v6z_5m4_cyt).

2.  Log on to the [RAM console](https://ram.console.aliyun.com/) by using Alibaba Cloud Account B.

3.  In the left-side navigation pane, click RAM Roles.

4.  In the RAM Role Name list, click the RAM role that is authorized to write data to the destination Logstore.

5.  On the page that appears, click the Trust Policy Management tab. On this tab, click **Edit Trust Policy**.

6.  Modify the policy.

    In the Edit Trust Policy dialog box, add a data entry in the format of Alibaba Cloud account ID@log.aliyuncs.com in the Service field. The Alibaba Cloud account ID is the ID of Alibaba Cloud Account A. To view the ID of Alibaba Cloud Account A, log on to the RAM console by using Alibaba Cloud Account A. On the page that appears, click the profile picture in the upper-right corner. On the page that appears, choose **Account Management** \> **Security Settings**. The ID of the Alibaba Cloud Account A is displayed on the Security Settings page. This policy indicates that Log Service of Alibaba Cloud Account A is authorized to manage the cloud resources of Alibaba Cloud Account B by using a temporary STS token.

    ```
    {
        "Statement": [
            {
                "Action": "sts:AssumeRole",
                "Effect": "Allow",
                "Principal": {
                    "Service": [
                        "<The ID of Alibaba Cloud Account A to which the source Logstore belongs>@log.aliyuncs.com"
                    ]
                }
            }
        ],
        "Version": "1"
    }
    ```

7.  Obtain the ARN of the RAM role.

    On the RAM Roles page, click the RAM role. In the Basic Information section of the page that appears, view the ARN of the role, for example, acs:ram::13234:role/logrole.


