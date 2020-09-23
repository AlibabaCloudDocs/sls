# Configure an AccessKey pair for a RAM user to access a source Logstore and a destination Logstore

When you create a transformation rule, you can configure an AccessKey pair for an Alibaba Cloud account or RAM user. Then you can use the Alibaba Cloud account or RAM user to read data from a source Logstore and write the transformed data to one or more destination Logstores. This topic describes how to configure AccessKey pairs for RAM users to perform read and write operations on Logstores.

A data transformation task is performed based on the following steps. In Step 1 and Step 3, permissions are required to access Logstores. If you use an Alibaba Cloud account to access the source and destination Logstores, you do not need to separately configure read and write permissions for the account. For the concerns of account security and fine-grained access control, we recommend that you use your Alibaba Cloud account to grant permissions to RAM users and then use the RAM users to access Logstores.

1.  Read data from a source Logstore.
2.  Transform the source data.
3.  Write transformed data to a destination Logstore.

## Create an AccessKey pair for a RAM user to access a source Logstore

1.  Log on to the [RAM console](https://ram.console.aliyun.com/) by using an Alibaba Cloud account.

2.  Create a RAM user. For more information, see [Create a RAM user](/intl.en-US/Developer Guide/Access control RAM/Create a RAM user and authorize the RAM user to access Log Service.md).

    When you create a RAM user, select **Programmatic Access** for **Access Mode**, and save the AccessKey pair of the RAM user.

    **Note:**

    -   You can create a maximum of two AccessKey pairs for a RAM user.
    -   After you create an AccessKey pair, you cannot view the AccessKey secret of the AccessKey pair by using the console. We recommend that you copy the AccessKey secret when you create an AccessKey pair and keep the AccessKey secret confidential.
3.  Create a permission policy for the RAM user to read data from a source Logstore.

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

4.  Attach the policy to the RAM user.

    1.  In the left-side navigation pane, choose **Identities** \> **Users**.

    2.  On the Users page, find the RAM user, and click **Add Permissions** in the Actions column.

    3.  In the Select Policy section, select **Custom Policy**. In the Authorization Policy Name list, select the policy that you created in [Step 3](#step_6nl_j25_k2h), and then click OK. In this example, the policy is **log-etl-source-reader-1-policy**.

        ![Add permissions](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9943749951/p58754.png)


## Create an AccessKey pair for a RAM user to access a destination Logstore

1.  Log on to the [RAM console](https://ram.console.aliyun.com/) by using an Alibaba Cloud account.

2.  Create a RAM user. For more information, see [Create a RAM user](/intl.en-US/Developer Guide/Access control RAM/Create a RAM user and authorize the RAM user to access Log Service.md).

    When you create a RAM user, select **Programmatic Access** for **Access Mode**, and save the AccessKey pair of the RAM user.

    **Note:**

    -   You can create a maximum of two AccessKey pairs for a RAM user.
    -   After you create an AccessKey pair, you cannot view the AccessKey secret of the AccessKey pair by using the console. We recommend that you copy the AccessKey secret when you create an AccessKey pair and keep the AccessKey secret confidential.
3.  Create a permission policy for the RAM user to write data to a destination Logstore.

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

4.  Attach the policy to the RAM user.

    1.  In the left-side navigation pane, choose **Identities** \> **Users**.

    2.  On the Users page, find the RAM user, and click **Add Permissions** in the Actions column.

    3.  In the Select Policy section, select **Custom Policy**. In the Authorization Policy Name list, select the policy that you created in [Step 3](#step_bqu_zxr_ag0), and then click OK. In this example, the policy is **log-etl-source-reader-1-policy**.

        ![Write permissions](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9943749951/p107689.png)


You must configure an AccessKey pair for a RAM user when you create a data transformation rule. For more information, see [Create a data transformation task](/intl.en-US/Data Transformation/Create a data transformation task.md). Enter the AccessKey pair that you create for the RAM user to access the source Logstore in the first red box. For more information, see [Create an AccessKey pair for a RAM user to access a source Logstore](#section_bp1_fos_oep). Enter the AccessKey pair that you create for the RAM user to access the destination Logstore in the second red box. For more information, see [Create an AccessKey pair for a RAM user to access a destination Logstore](#section_6rs_1c4_9h8).

![Modify the transformation rule](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2708280061/p58759.png)

