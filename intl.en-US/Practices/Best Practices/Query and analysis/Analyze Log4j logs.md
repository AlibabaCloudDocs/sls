# Analyze Log4j logs

This topic describes how to analyze Log4j logs in the Log Service console. The logs of an e-commerce company are used as an example.

-   Log4j logs are collected. For more information, see [Collect Log4j logs](/intl.en-US/Data Collection/Collect common logs/Collect Log4j logs.md).
-   The indexing feature is enabled for the Logstore and indexes are configured. For more information, see [Enable and configure the index feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

    The following figure shows the indexes that are used in this example.

    ![Field query](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1111144851/p5909.png)


Log4j is an open source project of Apache. Log4j allows you to specify the output destination and format of logs. You can also specify the severity level of logs. The severity levels of logs are classified into ERROR, WARN, INFO, and DEBUG in descending order. The output destination specifies whether logs are sent to the console or files. The output format specifies the format of logs.

In this example, an e-commerce company wants to obtain the best solution for the platform. The company needs to analyze information that includes behavioral data, such as logon methods, logon time, logon duration, accessed pages, access duration, average order time, and consumption level, platform stability, system errors, and data security. Log Service provides multiple log collection methods and the log analysis feature. Sample logs are collected by Log Service, as shown in the following examples.

-   The following log indicates the logon information:

    ```
    level:  INFO  
    location:  com.aliyun.log4jappendertest.Log4jAppenderBizDemo.login(Log4jAppenderBizDemo.java:38)
    message:  User login successfully. requestID=id4 userID=user8  
    thread:  main  
    time:  2018-01-26T15:31+0000
    ```

-   The following log indicates the purchase information:

    ```
    level:  INFO  
    location:  com.aliyun.log4jappendertest.Log4jAppenderBizDemo.order(Log4jAppenderBizDemo.java:46)
    message:  Place an order successfully. requestID=id44 userID=user8 itemID=item3 amount=9  
    thread:  main  
    time:  2018-01-26T15:31+0000
    ```


## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click a project.

3.  On the **Log Management** \> **Logstores** tab, choose **![Management icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9484688951/p52166.png)** \> **Search & Analysis** to the right of a Logstore.

4.  In the upper-right corner of the page, click **15 Minutes \(Relative\)** to specify a time range for the query.

    You can select a relative time or time frame. You can also specify a custom time range.

    **Note:** The query results may contain logs that are generated 1 minute earlier or later than the specified time range.

5.  Enter a query statement in the search box, and then click **Search & Analyze**.

    For more information about query statements, see [Statement format](/intl.en-US/Index and query/Overview.md).

    -   You can enter the following query statement to view the statistics of the three locations where the most errors occurred in the last hour:

        ```
        level: ERROR | select location ,count(*) as count GROUP BY  location  ORDER BY count DESC LIMIT 3
        ```

    -   You can enter the following query statement to view the number of log entries that are generated in each severity level in the last 15 minutes:

        ```
        | select level ,count(*) as count GROUP BY level ORDER BY count DESC
        ```

    -   You can enter the following query statement to view the statistics of the three user IDs that log on to the platform for the most number of times in the last hour:

        ```
        login | SELECT regexp_extract(message, 'userID=(? <userID>[a-zA-Z\d]+)', 1) AS userID, count(*) as count GROUP BY userID ORDER BY count DESC LIMIT 3
        ```

    -   You can enter the following query statement to view the total payment amount of each user ID in the last 15 minutes:

        ```
        order | SELECT regexp_extract(message, 'userID=(? <userID>[a-zA-Z\d]+)', 1) AS userID, sum(cast(regexp_extract(message, 'amount=(? <amount>[a-zA-Z\d]+)', 1) AS double)) AS amount GROUP BY userID
        ```


