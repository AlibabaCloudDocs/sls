# Query slow query logs

In the ApsaraDB for Redis console, you can view slow query logs and use them to analyze how to improve performance and optimize requests.

[Enable the log audit feature](/intl.en-US/Data Collection/Cloud product collection/ApsaraDB for Redis logs/Enable the log audit feature.md)

## View slow logs on the console

Limits

-   The engine version of the ApsaraDB for Redis instance is Redis 4.0 or later.
-   The minor version of the ApsaraDB for Redis instance is the latest.
-   **Note:** If your instance version does not meet the conditions for querying slow logs and you need to upgrade the version to use the slow log feature, upgrade the major or minor version as needed. For more information, see [Upgrade the minor version](/intl.en-US/User Guide/Manage instances/Lifecycle management/Upgrade the minor version.md) and [Upgrade the major version](/intl.en-US/User Guide/Manage instances/Lifecycle management/Upgrade the major version.md).


Procedure

1.  Log on to the [ApsaraDB for Redis console](https://kvstore.console.aliyun.com/).
2.  In the upper-left corner of the top navigation bar, select the region where the target instance is located.
3.  On the Instance List page, click the target instance ID or **Manage** in the **Action** column for the target instance.
4.  On the Instance Information page, choose **Logs** \> **Slow Logs** from the left-side navigation pane.
5.  On the Slow Logs page, click the calendar icon next to **Query Time**, select a time option or set **Start Time** and **End Time**, and then click **OK**.

    ![View slow logs in Logs of the console](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8307549951/p36254.png)

    **Note:** If you use an ApsaraDB for Redis instance of the cluster edition, you can click **Data nodes** next to **Query Time** and select the target node.


## View slow logs in DMS

Procedure

1.  [Use DMS to log on to the instance](/intl.en-US/Quick Start/Step 3: Connect to the instance/Use DMS.md).
2.  Choose Performance \> **Slow Logs** from the top navigation bar.
3.  On the Current Logs tab, view slow logs in the list.

