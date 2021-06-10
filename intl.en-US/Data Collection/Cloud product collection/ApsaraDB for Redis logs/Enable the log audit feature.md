# Enable the log audit feature

This topic describes how to enable the log audit feature in the ApsaraDB for Redis console. This topic also describes how to use the feature to push audit logs, slow query logs, and operational logs to Log Service.

-   An ApsaraDB for Redis instance is created. The version of the instance is Redis 4.0 or later and the minor version of the instance is the latest. The editions of ApsaraDB for Redis include Community Edition and Enhanced Edition. For more information, see [Step 1: Create an ApsaraDB for Redis instance](/intl.en-US/Quick Start/Step 1: Create an ApsaraDB for Redis instance.md).

    For information about how to upgrade the minor and major versions, see [Upgrade the major version](/intl.en-US/User Guide/Manage instances/Lifecycle management/Upgrade the major version.md) and [Upgrade the minor version](/intl.en-US/User Guide/Manage instances/Lifecycle management/Upgrade the minor version.md).

-   Log Service is activated.

## Procedure

1.  Log on to the [ApsaraDB for Redis console](https://kvstore.console.aliyun.com/).

2.  In the upper-left corner of the page, select the region where the ApsaraDB for Redis instance resides.

3.  On the Instances page, click the name of the required instance.

4.  In the left-side navigation pane, choose **Logs** \> **Audit Log**.

5.  Select **Official Edition** and click **Enable Audit Logs**.

6.  In the **Enable Audit Logs** dialog box, click **OK**.


After ApsaraDB for Redis logs are collected by Log Service, you can query, analyze, download, ship, and transform the collected logs. You can also configure alerts for these logs. For more information, see [Common operations on logs of Alibaba Cloud services](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).

