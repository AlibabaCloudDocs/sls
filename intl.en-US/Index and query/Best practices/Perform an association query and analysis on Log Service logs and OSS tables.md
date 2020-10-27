# Perform an association query and analysis on Log Service logs and OSS tables

This topic describes how to use Object Storage Service \(OSS\) tables to perform an association query and analysis in Log Service.

-   Log data is collected. For more information, see [Log collection](/intl.en-US/Data Collection/Log collection methods.md).
-   The indexing feature is enabled for the Logstore and indexes are configured. For more information, see [Enable and configure the index feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).
-   An OSS bucket is created. For more information, see [Create buckets](/intl.en-US/Quick Start/Create buckets.md).

Assume that you have an electronic payment service company and you want to analyze the impact of user age, region, and gender on payment preferences. You have collected payment logs and saved user properties to an OSS bucket by using Log Service. The payment logs contain bills and payment methods. The user properties contain the region, age, and gender information of the users. The query and analysis engine of Log Service can perform an association query and analysis on the data stored in Logstores and external stores, such as MySQL and OSS. You can use the SQL JOIN syntax to associate the user payment logs with the user properties to analyze the payment preferences of these users.

An association query and analysis has the following advantages:

-   Low cost: If you store infrequently updated data in OSS buckets, you only need to pay for the storage service. Data can also be read from OSS buckets over the Internet without incurring traffic fees.
-   Less workload: If you use the lightweight association analysis platform, you do not need to store all data in one storage system.
-   High efficiency: If you use SQL statements to analyze data, you can view the analysis results in seconds. You can also define commonly used analysis results as reports. Then, you can click the reports to view the analysis results.

1.  Create a CSV file and upload it to OSS.

    1.  Create a file named user.csv.

        ![User properties](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9540665951/p41528.png)

    2.  Upload user.csv to OSS. For more information, see [Upload objects](/intl.en-US/Quick Start/Upload objects.md).

2.  Log on to the [Log Service console](https://sls.console.aliyun.com).

3.  In the Projects list, click the project.

4.  On the page that appears, choose **Log Management** \> **Logstores**, and then click the Logstore.

5.  On the page that appears, enter a query statement in the search box, and click **Search & Analyze**.

    Execute the following SQL statement to define a virtual external table, for example, user\_meta1. Map the table to user.csv. If the **result** is **true**, this indicates that the external table is created.

    ```
    * | create table user_meta1 ( userid bigint, nick varchar, gender varchar, province varchar, gender varchar,age bigint) with ( endpoint='example.com',accessid='<youraccessid>',accesskey='<accesskey>',bucket='testossconnector',objects=ARRAY['user.csv'],type='oss')
    ```

    ![External storage table](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5131201061/p8538.png)

    Define the name and table schema of the virtual external table by executing the SQL statements that correspond to the desired parameters. Specify the access and file parameters of OSS by using the WITH clause. The following table describes the parameters.

    |Parameter|Description|
    |:--------|:----------|
    |Name of the external storage table|The name of the virtual external table, for example, user\_meta1.|
    |Table schema|The properties of the external storage table, including the column names and data types, for example, \(userid bigint, nick varchar, gender varchar, province varchar, gender varchar,age bigint\).|
    |endpoint|The access domain name of the OSS bucket. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).|
    |accessid|The AccessKey ID.|
    |accesskey|The AccessKey secret.|
    |bucket|The OSS bucket that stores the CSV file.|
    |objects|The path of the CSV file. **Note:** The values in the objects field are arranged as an array. This field can contain multiple OSS bucket names. |
    |type|Default value: oss. It indicates that the external storage type is OSS.|

6.  Check whether the external storage table is created.

    Execute the following SQL statement. If the returned result is the content that you defined, the external storage table is created.

    ```
    select * from user_meta1
    ```

    ![Result](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6051201061/p8539.png)

7.  Use the SQL JOIN syntax to perform an association query and analysis.

    Execute the following SQL statement to associate the Logstore ID with the OSS table userid: test\_accesslog is the name of the Logstore. l is the alias of the Logstore. user\_meta1 is the name of the external storage table. Replace the parameter values based on your business requirements.

    ```
    * | select * from test_accesslog l join user_meta1 u on l.userid = u.userid
    ```

    Examples of association query and analysis

    -   Analyze the access requests from users of different genders.

        ```
        * | select u.gender, count(1) from test_accesslog l join user_meta1 u on l.userid = u.userid group by u.gender
        ```

        ![Access requests queried by gender](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9540665951/p41547.png)

    -   Analyze the access requests from users of different ages.

        ```
        * | select u.age, count(1) from test_accesslog l join user_meta1 u on l.userid = u.userid group by u.age
        ```

        ![Access request queried by age](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9540665951/p41550.png)

    -   Analyze the access trends for users of different ages in different time periods.

        ```
        * | select date_trunc('minute',__time__) as minute, count(1) ,u.age from test_accesslog l join user_meta1 u on l.userid = u.userid group by u.age,minute
        ```

        ![Access trends analyzed by time](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9540665951/p41551.png)


