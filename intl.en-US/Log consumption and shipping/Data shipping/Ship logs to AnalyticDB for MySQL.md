# Ship logs to AnalyticDB for MySQL

After you collect logs by using Log Service, you can ship the logs to AnalyticDB for MySQL for data storage and analysis. This topic describes how to ship logs from Log Service to AnalyticDB for MySQL.

## Prerequisites

-   Logs are collected to a destination Logstore. For more information, see [Log collection methods](/intl.en-US/Data Collection/Log collection methods.md).
-   The following operations are performed in the AnalyticDB for MySQL console:
    1.  An AnalyticDB for MySQL cluster is created in the region where the Log Service project resides. For more information, see [t1854366.dita\#task1307]().

        **Note:** Log Service can ship logs only in the same region.

    2.  A database account is created. For more information, see [Create a database account]().
    3.  A database is created. For more information, see [Create a database]().
    4.  If you need to access AnalyticDB for MySQL clusters over the Internet, you must first apply for a public endpoint. For more information, see [Apply for or release a public endpoint]().
    5.  A database table is created. For more information, see [CREATE TABLE]().

## Create a log shipping task

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the destination project.

3.  On the **Log Storage** \> **Logstores** tab, click the \> icon next to the destination Logstore. Then, choose **Data Transformation** \> **Export**, and click **+** next to **AnalyticDB**.

4.  In the Shipping Notes dialog box, click **Ship**.

5.  You must create the AliyunServiceRoleForAnalyticDBForMySQL service linked role when you create a log shipping task to AnalyticDB for MySQL for the first time.

    1.  In the Create Service Linked Role dialog box, click AliyunServiceRoleForAnalyticDBForMySQL.

        ![Create a service linked role](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2549276061/p179192.png)

    2.  In the Create Service Linked Role dialog box, click **OK**.

6.  On the LogHub - Data Shipper page, set the parameters and click **OK**.

    ![Data Shipper](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2549276061/p179204.png)

    |Parameter|Description|
    |---------|-----------|
    |**Shipper Name**|The name of a log shipping task.|
    |**Shipper Description**|The description of a log shipping task.|
    |**Cluster Version**|The version of the AnalyticDB for MySQL cluster. Version 3.0 is selected in this example.|
    |**Cluster Name**|Select the AnalyticDB for MySQL cluster that you have created.|
    |**DatabaseName**|Select the database that you have created in the AnalyticDB for MySQL cluster.|
    |**Table Name**|Select the table that you have created in the AnalyticDB for MySQL cluster.|
    |**Account Name**|The account of the database that you have created in the AnalyticDB for MySQL cluster.|
    |**Account Password**|The account password of the database that you have created in the AnalyticDB for MySQL cluster.|
    |**Field Mapping**|Log Service extracts all log fields of the last ten minutes and maps these fields to the destination fields in the table in AnalyticDB for MySQL. Enter the names of log fields in the left field. Enter the names of log fields in the table of the AnalyticDB for MySQL database in the right field.![Log Shipper task 2](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2549276061/p76519.jpg) |
    |**Delivery Start Time**|The start time of a log shipping task. After you collect logs by using Log Service, you can ship the logs to AnalyticDB for MySQL for data storage and analysis in real time. |
    |**Filter Dirty Data**|Dirty data includes data whose data type fails to be converted and whose required fields are empty.    -   If you turn **off** this switch and dirty data is detected, the log shipping task stops. Proceed with caution.
    -   If you turn **on** this switch, dirty data is filtered out. |


After you create a log shipping task, you can manage the task in the Log Service console. You can view the task details, modify the shipping rules, and start, stop, or delete the task. For more information, see [t13186.md\#]().

## View log data

After logs are shipped to AnalyticDB for MySQL, you can execute the [SELECT]() statement to query log data.

