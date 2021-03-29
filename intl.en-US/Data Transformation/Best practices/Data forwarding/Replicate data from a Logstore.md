# Replicate data from a Logstore

Log Service allows you to replicate data from a source Logstore to a destination Logstore. To replicate the data, you can create a data transformation rule for the source Logstore. This topic describes how to replicate data from a source Logstore to a destination Logstore in a typical scenario.

## Scenario

In this example, the access logs of a company are collected and stored in multiple projects of an Alibaba Cloud account. The company wants to replicate the access logs from the logstore-a Logstore of the project-a project to the logstore-b Logstore of the project-b project. Then, the company can query and analyze the data in the project-b project in a centralized manner. To replicate data from the logstore-a Logstore to the logstore-b Logstore, the company can use the replication feature in Log Service.

![copy](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7757896161/p256030.png)

To use the replication feature, you must make sure that the following requirements are met:

-   A Logstore named logstore-a is created and data is written to the Logstore.
-   The performance of the logstore-b Logstore is evaluated, for example, the number of shards. For more information, see [Performance guide](/intl.en-US/Data Transformation/Performance guide.md).
-   A Logstore named logstore-b is created. For more information, see [Manage a Logstore](/intl.en-US/Data Collection/Preparation/Manage a Logstore.md).

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the project-a project.

3.  On the **Log Storage** \> **Logstores** tab, click the logstore-a Logstore.

4.  On the Search & Analysis page, click **Data Transformation** in the upper-right corner to enable the data transformation mode.

5.  On the page that appears, click **Save as Transformation Rule**.

6.  In the Create Data Transformation Rule panel, set the required parameters. The following table describes the parameters.

    |Parameter|Description|
    |---------|-----------|
    |Rule Name|The name of the transformation rule. Enter test.|
    |Authorization Method|Authorizes Log Service to read data from the logstore-a Logstore. The default role is used as an example. Select Default Role.|
    |Target Name|The name of the storage target. Enter test.|
    |Target Region|The region where the destination project resides. Select China \(Hangzhou\).|
    |Target Project|The name of the destination project. Enter project-b.|
    |Target Logstore|The name of the destination Logstore. Enter logstore-b.|
    |Authorization Method|Authorizes Log Service to read data from and write data to the logstore-b Logstore. The default role is used as an example. Select Default Role. |
    |Time Range|The time when the logs are received. The All value indicates that the data in the source Logstore is transformed from the first log entry until the transformation task is manually stopped. Select All.|

    For information about other parameters, see [Create a data transformation rule](/intl.en-US/Data Transformation/Create a data transformation rule.md).

7.  Click **OK**.


In the Projects section, click the project-b project. On the **Log Storage** \> **Logstores** tab, select the logstore-b Logstore. You can view the data that is replicated from the logstore-a Logstore.

![result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7757896161/p256036.png)

