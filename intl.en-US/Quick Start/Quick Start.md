# Quick Start

This topic describes how to collect logs of Alibaba Cloud Elastic Compute Service \(ECS\) instances in the Log Service console. This topic also describes how to query and analyze the collected logs.

-   An ECS instance is available. For more information, see [ECS quick start](/intl.en-US/Quick Start/Manage an ECS instance in the console (express version).md).
-   Logs are available on the ECS instance.

    In this example, the logs are stored in the /var/log/nginx/access.log file and the sample log is `127.0.0.1|#|-|#|13/Apr/2020:09:44:41 +0800|#|GET /1 HTTP/1.1|#|0.000|#|74|#|404|#|3650|#|-|#|curl/7.29.0`. [Delimiter mode](/intl.en-US/Data Collection/Logtail collection/Text logs/Collect DSV formatted logs.md) is used in this example to collect the sample log.


## Step 1: Create a project and a Logstore

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Create a project.

    1.  In the Projects section, click **Create Project**.

    2.  In the Create Project dialog box, set the parameters. The following table describes the required parameters. You can use the default values for other parameters.

        |Parameter|Description|
        |---------|-----------|
        |Project Name|The name of the project. The name must be unique in a region and cannot be modified after the project is created.|
        |Region|The data center of the project. We recommend that you select the region where the ECS instance resides. Then, you can use the internal network of Alibaba Cloud to accelerate log collection.After you create a project, you cannot migrate the project to another region or modify the region to which the project belongs. |

    3.  Click **OK**.

3.  Create a Logstore.

    After you create a project, you are prompted to create a Logstore.

    In the Create Logstore dialog box, set the parameters. The following table describes the required parameters. You can use the default values for other parameters.

    |Parameter|Description|
    |---------|-----------|
    |Logstore Name|The name of the Logstore. The name must be unique in the project to which the Logstore belongs. After the Logstore is created, the name cannot be modified.|
    |Shards|Log Service uses shards to read and write data.Each shard supports a write capacity of 5 MB/s and 500 times/s and a read capacity of 10 MB/s and 100 times/s. If one shard can meet your business requirements, you can set the **Shards** parameter to **1**. No fees are incurred if you only create one Logstore and use one shard. |
    |Automatic Sharding|If you enable this feature, Log Service increases the number of shards if the number of read and write requests exceeds the capacity of the existing shards.You can turn off the **Automatic Sharding** switch if the existing number of shards can meet your business requirements. |


## Step 2: Collect logs

1.  In the Import Data section, select **Delimiter Mode - Text Log**.

2.  Select the project and Logstore that you have created and click **Next**.

3.  Install Logtail.

    1.  On the ECS Instances tab, select the destination ECS instance and click **Execute Now**.

    2.  Confirm that the **Execution Status** is **Success** and click **Complete Installation**.

4.  Create an IP address-based machine group and click **Next**.

    The following table describes the required parameters. You can use the default values for other parameters.

    |Parameter|Description|
    |---------|-----------|
    |Name|The name of the machine group. The name must be unique in a project and cannot be modified after the machine group is created.|
    |IP addresses|The IP address of the ECS instance. Separate multiple IP addresses with line feeds. **Note:** Windows and Linux servers cannot be added to the same machine group. |

5.  Select and move the destination machine group from **Source Server Groups** to **Applied Server Groups**, and then click **Next**.

    **Note:** If you want to apply a machine group immediately after it is created, the heartbeat status of the machine group may be **FAIL**. This is because the machine group has not been connected to Log Service. In this case, you can click **Automatic Retry**. If the problem persists, see [What can I do if the Logtail client has no heartbeat?]()

6.  Create a Logtail configuration file and click **Next**.

    The following table describes the required parameters. You can use the default values for other parameters.

    |Parameter|Description|
    |---------|-----------|
    |Config Name|The name of the Logtail configuration file. The name must be unique in a project and cannot be modified after the file is created.|
    |Log Path|The directory and name of the log file. The file names can be complete names or names that contain wildcards. For more information, visit [Wildcard matching](http://man7.org/linux/man-pages/man7/glob.7.html). The log files in all levels of subdirectories under a specified directory are monitored if the log files match the specified pattern. Examples:

    -   /apsara/nuwa/… /\*.log indicates that the files whose extension is .log in the /apsara/nuwa directory and its subdirectories are monitored.
    -   /var/logs/app\_\*/\*.log indicates that each file that meets the following conditions is monitored: The file name contains .log. The file is stored in a subdirectory \(at all levels\) of the /var/logs directory. The name of the subdirectory matches the app\_\* pattern.
**Note:**

    -   By default, each log file can be collected by using only one Logtail configuration file.
    -   You can include only asterisks \(\*\) and question marks \(?\) as wildcard characters in the log path. |
    |Log Sample|Enter a valid sample log entry that is collected from an actual scenario. Example:    ```
127.0.0.1|#|-|#|13/Apr/2020:09:44:41 +0800|#|GET /1 HTTP/1.1|#|0.000|#|74|#|404|#|3650|#|-|#|curl/7.29.0
    ``` |
    |Delimiter|Select a delimiter based on the log format. Example: `|#|`.**Note:** If you select **Hidden Characters** as the quote, you must enter a character in the following format: `0xthe hexadecimal ASCII code of the non-printable character`. For example, to use the non-printable character whose hexadecimal ASCII code is 01, you must enter **0x01**. |
    |Extracted Content|Log Service extracts the log content based on the specified sample log and delimiter. The extracted log content is delimited into values. You must specify a key for each value.|

    Click **Next** to complete the configuration and use Logtail to collect logs.

    **Note:**

    -   The Logtail configurations require at most 3 minutes to take effect.
    -   If an error occurs when you use Logtail to collect logs, see [Diagnose collection errors]().
7.  Preview the data and click **Next**.

    By default, Log Service enables the Full Text Index to query and analyze logs. For more information, see [Enable and configure the indexing feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

    **Note:**

    -   The index is applicable only to the log data that is newly written.
    -   To query and analyze logs, you must enable the Full Text Index or Field Search. If you enable both of them, the settings of Field Search prevail.

## Step 3: Query and analyze logs

1.  In the Projects section, click the project in which you want to query and analyze logs.

2.  On the **Log Storage** \> **Logstores** tab, click the Logstore where logs are stored.

3.  Enter a query statement in the search box, set a time range, and then click **Search & Analyze**.

    Example: You can run the following query statement to view the location of IP addresses of the previous day. You can also display the query result on a chart.

    -   Query statement

        ```
        * | select count(1) as c, ip_to_province(remote_addr) as address group by address limit 100
        ```

    -   Query result

        The following figure shows that 329 IP addresses were located in Guangdong province and 313 IP addresses were located in Beijing on the previous day. Log service allows you to display the query result on a chart. For more information, see [Chart overview](/intl.en-US/Query and visualization/Analysis graph/Overview.md).

        ![Query result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6140049061/p203039.png)


## What to do next

-   Ship logs: You can ship collected logs to Object Storage Service \(OSS\), MaxCompute, and other storage or computing services. For more information, see [LogShipper](/intl.en-US/Log consumption and shipping/Data shipping/Overview.md).
-   Consume logs: You can consume collected logs. For more information, see [Log consumption](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Overview.md).
-   Data transformation: You can perform multiple operations on the collected logs. For example, you can standardize, enrich, distribute, and aggregate the collected logs. For more information, see [Data transformation](/intl.en-US/Data Transformation/Overview.md).

