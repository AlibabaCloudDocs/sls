# Manage a Logstore

This topic describes how to create, modify, and delete a Logstore in the Log Service console.

## Create a Logstore

**Note:** You can create a maximum of 200 Logstores for a project.

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the **Projects** section, click the target project.

3.  Choose **Log Management** \> **Logstores**. Click **+**.

4.  In the Create Logstore dialog box, set the required parameters. The following table describes the parameters.

    |Parameter|Description|
    |---------|-----------|
    |Logstore Name|The name of the Logstore. The name must be unique in the project to which the Logstore belongs. After the Logstore is created, the name cannot be modified.|
    |WebTracking|Specifies whether to enable web tracking. If you enable this feature, Log Service collects logs from web browsers and mobile apps that run on iOS or Android. The collected logs are copied to Log Service. By default, this feature is disabled.|
    |Permanent Storage|Specifies whether to enable permanent storage. If you enable this feature, Log Service permanently stores the collected logs. By default, this feature is disabled.|
    |Data Retention|The retention period of the collected logs. If you disable the **Permanent Storage** feature, you must specify the **Data Retention** parameter. Valid values: 1 to 3000. Unit: days. Logs that have been retained for more than the specified days are automatically deleted. |
    |Shards|Select the number of shards in the Logstore. You can create up to 10 shards for each Logstore. You can create up to 200 shards for each project.|
    |Automatic Sharding|Specifies whether to enable automatic sharding. If you enable this feature, Log Service increases the number of shards when read and write requests exceed the capacity of the existing shards. By default, this feature is enabled. For more information, see [Manage shards](/intl.en-US/Data Collection/Preparation/Manage shards.md).|
    |Maximum Shards|The maximum number of shards after automatic sharding. This parameter is required if you enable the **Automatic Sharding** feature. Maximum value: 64.|
    |Log Public IP|Specifies whether to enable the log public IP feature. If you enable this feature, Log Service adds the following information to the tag field of the collected logs:     -   \_\_client\_ip\_\_: the public IP address of the log source.
    -   \_\_receive\_time\_\_: the time when Log Service receives the log. The time is a Unix timestamp. |

5.  Click **OK**.


## Modify a Logstore

1.  Choose **Log Management** \> **Logstores**. Click the management icon of the target Logstore, and then select **![Modify a Logstore](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5023359951/p52318.png)** \> **Modify**.

2.  On the **Logstore Attributes** page, click **Modify** in the upper-right corner.

    For more information about the parameters, see [Create a Logstore](#section_v52_2jx_ndb).

3.  Click **Save**.


## Delete a Logstore

**Note:**

-   Before you can delete a Logstore, you must delete all relevant Logtail configurations.
-   If LogShipper is enabled for the Logstore, we recommend that you stop writing data to the Logstore before you delete the Logstore. In addition, make sure that all data in the Logstore is shipped.
-   If you cannot delete a Logstore by using your Alibaba Cloud account due to insufficent permissions, submit a [ticket](https://workorder-intl.console.aliyun.com/console.htm).

1.  Choose **Log Management** \> **Logstores**. Click the management icon of the target Logstore, and then select **![Delete a Logstore](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5023359951/p52318.png)** \> **Delete**.

    **Warning:** If a Logstore is deleted, the log data in the Logstore is permanently deleted. Proceed with caution.

2.  In the dialog box that appears, click **OK**.


