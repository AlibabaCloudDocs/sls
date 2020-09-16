# Enable the log management feature

This topic describes how to enable the log management feature in the Message Service \(MNS\) console. After you enable the feature, you can ship MNS operations logs to Log Service.

-   A project and a Logstore are created. For more information, see [Step 1: Create a project and a Logstore](/intl.en-US/Quick Start/Quick start.md).

    You can ship operations logs of queue messages and topic messages to a project that resides in the same region as the queues and topics.

-   MNS is authorized to assume the AliyunMNSLoggingRole role to ship logs to Log Service.

    You can go to the [Cloud Resource Access Authorization](https://ram.console.aliyun.com/?spm=5176.mns.0.0.3f9c637462ljRq#/role/authorize?request=%7B%22Requests%22:%20%7B%22request1%22:%20%7B%22RoleName%22:%20%22AliyunMNSLoggingRole%22,%20%22TemplateId%22:%20%22Logging%22%7D%7D,%20%22ReturnUrl%22:%20%22https:%2F%2Fmns.console.aliyun.com%2F%22,%20%22Service%22:%20%22MNS%22%7D) page to authorize Log Service to access OSS.

    **Note:**

    -   You must not delete the RAM role or revoke the permissions from the RAM role. Otherwise, logs cannot be shipped to Log Service.
    -   If you use a RAM user to log on to MNS, you must authorize the RAM user by using an Alibaba Cloud account.

## Step 1: Configure log shipping

1.  Log on to the [MNS console](https://mns.console.aliyun.com).

2.  In the upper-left corner of the page, select a region.

3.  In the left-side navigation pane, click **Logging Management**.

4.  Click **Configurations** in the Actions column of the selected region.

5.  In the Configurations dialog box, click the **Push Logs to LogService** tab. On this tab, select a project and a Logstore, and then click **OK**.

    **Note:** MNS operations logs are shipped to the specified Logstore about 3 minutes after they are generated.


## Step 2: Enable the log management feature

To enable the log management feature for operations logs of queue messages, perform the following steps:

1.  Log on to the [MNS console](https://mns.console.aliyun.com).

2.  In the left-side navigation pane, click **Queues**.

3.  In the upper-right corner of the **Queues** page, click **Create Queue**.

4.  In the Create Queue dialog box, turn on the **Enable Logging** switch. For more information about other parameters, see [Create a queue]().

5.  Click **OK**.


To enable the log management feature for operations logs of topic messages, perform the following steps:

1.  Log on to the [MNS console](https://mns.console.aliyun.com).

2.  In the left-side navigation pane, click **Topics**.

3.  In the upper-right corner of the **Topics** page, click **Create Topic**.

4.  In the Create Topic dialog box, turn on the **Enable Logging** switch. For more information about other parameters, see [Create a topic]().

5.  Click **OK**.


After MNS operations logs are collected by Log Service, you can query, download, ship, and transform the logs. You can also create alerts for the logs. For more information, see [Common operations on logs of Alibaba Cloud services](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).

