# Manage a Logstore

This topic describes how to create, modify, and delete a Logstore in the Log Service console.

## Create a Logstore

**Note:** You can create a maximum of 200 Logstores for a project.

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the destination project.

3.  Choose **Log Storage** \> **Logstores**, and then click the **+** icon.

4.  In the Create Logstore dialog box, set the required parameters. The following table describes the parameters.

    |Parameter|Description|
    |---------|-----------|
    |Logstore Name|The name of the Logstore. The name must be unique in the project to which the Logstore belongs. After the Logstore is created, you cannot modify the name.|
    |WebTracking|After you turn on the **WebTracking** switch, you can collect data from HTML, HTML5, iOS, or Android platforms and send the data to Log Service by using web tracking.|
    |Permanent Storage|After you turn on the **Permanent Storage** switch, Log Service permanently stores the collected logs.**Note:** If you set the data retention period to 3650 by calling the related API operation, the data is permanently stored. |
    |Data Retention Period|The retention period of the logs in the Logstore. Valid values: 1 to 3000. Unit: days. Logs are automatically deleted after the specified data retention period expires.If you do not turn on the **Permanent Storage** switch, you must set the **Data Retention Period** parameter. |
    |Shards|Log Service uses shards to read and write data. You can create up to 10 shards for each Logstore. You can create up to 200 shards for each project. For more information, see [Shards](/intl.en-US/Product Introduction/Basic concepts/Shards.md).|
    |Automatic Sharding|After you turn on the **Automatic Sharding** switch, if the read or write speed does not meet your requirements, Log Service increases the number of shards. For more information, see [Manage shards](/intl.en-US/Data Collection/Preparation/Manage shards.md).|
    |Maximum Shards|After you turn on the **Automatic Sharding** switch, you must set the Maximum Shards parameter. The maximum number of shards is 64.|
    |Log Public IP|After you turn on the **Log Public IP** switch, Log Service adds the following information to the Tag field of the collected logs.    -   \_\_client\_ip\_\_: the public IP address of the log source.
    -   \_\_receive\_time\_\_: the time when Log Service receives the log. The timestamp follows the UNIX timestamp format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970. |

5.  Click **OK**.


## Modify the configurations of a Logstore

1.  Choose **Log Storage** \> **Logstores**, find the Logstore that you want to modify, and then choose **![Modify a Logstore](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5023359951/p52318.png)** \> **Modify**.

2.  On the **Logstore Attributes** page, click **Modify**.

    For more information about the related parameters, see [Create a Logstore](#section_v52_2jx_ndb).

3.  Click **Save**.


## Delete a Logstore

**Note:**

-   Before you delete a Logstore, you must delete all related Logtail configuration files.
-   If the log shipping feature is enabled for the Logstore, we recommend that you stop writing data to the Logstore before you delete the Logstore. In addition, make sure that all data in the Logstore is shipped.
-   If you are not authorized to delete a Logstore by using your Alibaba Cloud account, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm?spm=a2796.7919406.0.dcontactus3.676a2d23RjosdV#/ticket/add/?productId=1210) to delete the project.

1.  Choose **Log Storage** \> **Logstores**, find the Logstore that you want to delete, and then choose **![Modify a Logstore](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5023359951/p52318.png)** \> **Delete**.

    **Warning:** After you delete a Logstore, all log data in the Logstore is deleted and cannot be restored. Proceed with caution.

2.  In the dialog box that appears, click **OK**.


