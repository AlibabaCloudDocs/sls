# Query audit logs

You can query audit logs to view requests within a specific period of time, and use filter conditions to filter the requests.

[Enable the log audit feature](/intl.en-US/Data Collection/Cloud product collection/ApsaraDB for Redis logs/Enable the log audit feature.md)

## Background

Audit logs provide a detailed insight into the status of your ApsaraDB for Redis instance. You can use audit logs to view request records. This allows you to check records of modify and delete operations and find the cause of sudden increases in database resource consumption.

## View audit logs

1.  Log on to the [ApsaraDB for Redis console](https://kvstore.console.aliyun.com/).

2.  On the top of the page, select the region where the instance is deployed.

3.  On the **Instances** page, click the Instance ID of the instance.

4.  The Instance Information page appears. In the left-side navigation pane, choose **Logs** \> **Audit Log**.

5.  On the **Audit Log** page, you can check the audit log details for the instance.


## Filter audit logs

You can define different conditions to filter audit logs.

1.  Log on to the [ApsaraDB for Redis console](https://kvstore.console.aliyun.com/).

2.  On the top of the page, select the region where the instance is deployed.

3.  On the **Instances** page, click the Instance ID of the instance.

4.  The Instance Information page appears. In the left-side navigation pane, choose **Logs** \> **Audit Log**.

5.  On the **Audit Log** page, you can define conditions to filter audit logs.

    ![Set filter conditions](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7307549951/p77324.png)

    |Filter condition|Description|
    |----------------|-----------|
    |Keyword|Filters audit logs by keyword, such as the client IP address, executed commands, accounts, and extended information. **Note:**

    -   The Keyword filed supports exact match. You must enter complete information in this field.
        -   For example, you must enter a complete IP address, such as 192.168.1.1, instead of 192.168 or 1.1.
        -   You must enter a complete command, such as AUTH or auth, instead of au.
    -   You must enclose keywords that contain colons within double quotation marks \(""\), for example, "userId:1". |
    |Type|The type of audit logs. Valid values:     -   redis\_audit\_log: the audit log of a data shard.
    -   redis\_proxy\_audit\_log: the audit log of a proxy server. |
    |Account|The [account](/intl.en-US/User Guide/Security management/Manage database accounts.md) used to connect to the ApsaraDB for Redis instance. Default value: null.|
    |Client ip|The client IP address used to connect to the ApsaraDB for Redis instance.|
    |DB|The database of which you want to query the audit logs.|


## View audit logs within a specified time range

You can view audit logs within a specified time range by using the time picker.

1.  Log on to the [ApsaraDB for Redis console](https://kvstore.console.aliyun.com/).

2.  On the top of the page, select the region where the instance is deployed.

3.  On the **Instances** page, click the Instance ID of the instance.

4.  The Instance Information page appears. In the left-side navigation pane, choose **Logs** \> **Audit Log**.

5.  On the **Audit Log** page, click **Please Select**.

    ![Click the time picker](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7307549951/p77365.png)

6.  In the Time pane, specify the time range to query audit logs.

    ![Time picker](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7307549951/p77366.png)

    |No.|Section name|Description|
    |---|------------|-----------|
    |1|Time Range|Information about a specified time range appears in this section when you place the pointer over a relative time range or a time frame.|
    |2|Relative|A time range relative to the current point in time. Information about the specified time range appears in the Time Range section when you place the pointer over any element in this section.|
    |2|Time Frame|A time range that is more than one minute. Information about the specified time range appears in the Time Range section when you place the pointer over any element in this section.|
    |4|Custom|A custom time range. Specify a time range and click **OK** to confirm the time range.|


