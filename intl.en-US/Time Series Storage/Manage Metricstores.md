# Manage Metricstores

This topic describes how to create, modify, and delete a Metricstore in the Log Service console.

A project is created. For more information, see [Create a project](/intl.en-US/Data Collection/Preparation/Manage a project.md).

## Create a Metricstore

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the project where you want to create a Metricstore.

3.  On the **Metricstore Storage** \> **Metricstore** tab, click the plus sign \(**+**\) next to the search box.

4.  In the Create Metricstore pane, set the following parameters.

    |Parameter|Description|
    |---------|-----------|
    |Metricstore Name|The name of the Metricstore. The name must be unique in the project to which the Metricstore belongs. After the Metricstore is created, the name cannot be modified.|
    |Permanent Storage|Specifies whether to enable permanent storage. If you enable this feature, Log Service permanently stores the collected time series data. By default, this feature is disabled.|
    |Data Retention Period|The retention period of the time series data. If you turn off the **Permanent Storage** switch, you must set the **Data Retention Period** parameter. Valid values: 1 to 3000. Unit: days. When the retention period expires, the time series data is deleted. |

5.  Click **OK**.


## Modify the configurations of a Metricstore

1.  On the **Metricstore Storage** \> **Metricstore** tab, choose **![More](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5023359951/p52318.png)** \> **Modify** next to the Metricstore.

2.  In the upper-right corner of the **Metricstore Attribute** page, click **Modify**.

    -   Modify the data retention period. For more information, see [Create a Metricstore](#section_8ju_apk_egg).
    -   Manage shards.

        After a Metricstore is created, two shards are automatically created in the Metricstore. You can split, merge, and delete shards as needed. The procedures to manage shards in a Metricstore are the same as the procedures to manage shards in a Logstore. For more information, see [Manage shards](/intl.en-US/Data Collection/Preparation/Manage shards.md).

3.  Click **Save**.


## Delete a Metricstore

**Note:**

-   Before you can delete a Metricstore, you must delete all Logtail configuration files that are associated with the Metricstore.
-   If data shipping is enabled for the Metricstore, we recommend that you stop writing data to the Metricstore and ensure that all data has been shipped to the Metricstore before deletion.
-   If you use the Alibaba Cloud account to delete a Metricstore but the system prompts that the account does not have the required permissions, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm?spm=a2796.7919406.0.dcontactus3.676a2d23RjosdV#/ticket/add/?productId=1210).

1.  On the **Metricstore Storage** \> **Metricstore** tab, choose **![More](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5023359951/p52318.png)** \> **Delete** next to the Metricstore.

    **Warning:** After you delete a Metricstore, the time series data in the Metricstore is deleted and cannot be restored.

2.  In the message that appears, click **OK**.


