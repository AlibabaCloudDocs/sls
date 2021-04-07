# Comparison between Log Service and the ELK stack in log query and analysis scenarios

This topic describes the main features and benefits of Log Service. In this case, it compares Log Service with the Elasticsearch, Logstash, and Kibana \(ELK\) stack.

## Background information

The ELK stack is a solution that is widely used to analyze logs in real time. For information about the case studies, visit the open source ELK community.

[Log Service](https://www.alibabacloud.com/zh/product/log-service?spm) is a solution dedicated to log query and analysis scenarios. The service is developed based on the monitoring and diagnosis tool that is used to develop the Apsara distributed operating system. To meet the requirement of growth in both users and business, Log Service is improved to support log analysis in Ops scenarios, such as DevOps, Market Ops, and SecOps. The service has surpassed the challenges in scenarios such as Double 11, Ant Financial Double 12, Spring Festival red envelopes, and international business.

## Overview

Apache Lucene is an open source search engine software library that is supported by the Apache Software Foundation. Apache Lucene provides full-text searching, full-text indexing, and text analysis capabilities. Elastic developed Elasticsearch in 2012 based on the Lucene library. It also launched the ELK stack in 2015 as an integrated solution to collect, store, and query logs. Lucene is designed to retrieve information based on documents. Its log processing capabilities are limited in many aspects, such as the data volume, query capability, intelligent grouping \(LogReduce\), and other custom features.

Log Service uses a self-developed log storage engine. In the past three years, Log Service has been applied to tens of thousands of applications. Log Service supports indexing for petabytes of data per day and serves tens of thousands of developers. It can be used to query and analyze data hundreds of millions of times per day. Log Service serves as the log analysis engine for multiple Alibaba Cloud services, such as SQL audit, EagleEye, Cloud Map, FLIGGY Trace, and Ditecting.

Log query is the most basic requirement of DevOps. In the industry research report [50 Most Frequently Used UNIX/Linux Commands](https://www.thegeekstuff.com/2010/11/50-linux-commands/), the tar and grep commands are the top two commands that are used by programmers to query logs.

The following list compares the ELK stack with Log Service in log query and analysis scenarios from five aspects:

-   Ease of use: the convenience to get started and use the service.
-   Features: the query and analysis features.
-   Performance: the query capabilities, analysis capabilities, and latency.
-   Capacity: the data processing capacity and the scalability.
-   Cost: the cost of using features.

## Ease of use

Log analysis is based on the following process:

-   Collection: uses stable method to write data.
-   Configuration: configures data sources.
-   Capacity expansion: expands storage space and scales out servers.
-   Usage: provides query and analysis features. These features are described in the "Features" section of this topic.
-   Export: exports data to other systems for further processing, such as for stream computing and for data backup in Object Storage Service \(OSS\).
-   Multi-tenancy: shares data with other users and ensures data security.

The following table includes a list that is used to compare the ease of use between Log Service and the ELK stack.

|Item|Sub-item|Self-managed ELK stack|Log Service|
|:---|:-------|:---------------------|:----------|
|Data collection|API|Restful API|-   Restful API
-   JDBC |
|Client|Various clients in the ecosystem, including Logstash, Beats, and Fluentd|-   Logtail
-   Other clients such as Logstash |
|Configuration|Resource object|Provides indexes to classify logs.|-   Project
-   Logstore

Provides projects in which Logstores can be created to store logs. |
|Method|API + Kibana|-   API + SDK
-   Console |
|Capacity expansion|Storage|-   Requires more servers.
-   Requires more disks.

|Requires no added servers or disks.|
|Computing|Requires more servers.|Requires no added servers.|
|Configurations|-   Configures servers by using the configuration management system.
-   The beta release of Logstash supports centralized configuration.

|Provides the console and API for configurations, without the need of a configuration management system.|
|Collection point|Applies configurations and installs Logstash on server groups by using the configuration management system.|Provides the console and API for configurations, without the need of a configuration management system.|
|Capacity|Does not support flexible capacity expansion.|Supports flexible and elastic capacity expansion.|
|Export|Method|-   API
-   SDK

|-   API
-   SDK
-   Kafka-like consumer API
-   Consumer API for stream computing engines, such as Spark, Storm, and Flink
-   Consumer API for stream computing class libraries, such as Python and Java class libraries |
|Multi-tenancy|Security|Commercial versions|-   https
-   Encrypted with signatures
-   Multi-tenant data isolation
-   Access control |
|Traffic shaping|No traffic shaping|-   Traffic shaping by project
-   Traffic shaping by shard |
|Multi-tenant|Supported by Kibana|Supported by the provision of accounts and granted by the related permissions|

Comparison results:

-   The ELK stack ecosystem provides multiple tools such as write, installation, and configuration tools.
-   Log Service is a managed service that is easy to access, configure, and implement. You can integrate Log Service with your services and use Log Service within 5 minutes.
-   Log Service is a service that uses the software as a service \(SaaS\) model. This can be used to address challenges in terms of capacity or concurrency. Log Service supports elastic scaling and does not require O&amp;M.

## Query and analysis features

The query feature allows you to query log entries that meet specified search conditions. The analysis feature allows you to calculate and analyze data.

For example, you want to calculate the number and traffic of read requests whose status code is greater than 200 based on IP addresses. You can use one of the following methods to analyze log data:

-   Query log entries and analyze the query results.
-   Analyze all log entries in a Logstore.

```
1. Status in (200,500] and Method:Get* | select count(1) as c, sum(inflow) as sum_inflow, ip group by Ip
2. * | select count(1) as c, sum(inflow) as sum_inflow, ip group by Ip
```

-   **Basic query capabilities**

    The following table includes a list that is used to compare the ELK stack with Log Service based on [Elasticsearch 6.5 Indices](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices.html).

    |Data type|Feature|ELK|Log Service|
    |:--------|:------|:--|:----------|
    |Text|Query by index|Supported|Supported|
    |Delimiter|Supported|Supported|
    |Chinese delimiter|Supported|Supported|
    |Prefix|Supported|Supported|
    |Suffix|Supported|-|
    |Fuzzy query|Supported|Supported by using SQL statements|
    |Wildcard|Supported|Supported by using SQL statements|
    |Numeric|long|Supported|Supported|
    |double|Supported|Supported|
    |Nested|Json|Supported|-|
    |Geo|Geo|Supported|Supported by using SQL statements|
    |IP|Query by IP address|Supported|Supported by using SQL statements|

    Comparison results:

    -   The ELK stack supports more data types and provides stronger native query capabilities than Log Service.
    -   Log Service allows you to use SQL statements instead of fuzzy match or Geo functions to query data. However, the query performance is slightly compromised. The following examples provide further details about how to use SQL statements to query data:
    ```
    To query data that matches the specified substring, execute the following query statement:
    * | select content where content like '%substring%' limit 100
    
    To query data that matches the specified regular expression, execute the following query statement:
    * | select content where regexp_like(content, '\d+m') limit 100
    
    To query parsed JSON-formatted data that matches the specified conditions, execute the following query statement:
    * | select content where json_extract(content, '$.store.book')='mybook' limit 100
    
    To create an index for JSON-formatted data, execute the following query statement:
    field.store.book='mybook'
    ```

-   **Extended query capabilities**

    In log query scenarios, you may need to perform the following operations based on the query results:

    -   If you find an error log entry, you can check the context to identify the parameters that cause the error.
    -   If you find an error, you can run the tail -f command to display raw log entries and run the grep command to search for similar errors.
    -   If you obtain millions of log entries from a query by using keywords, you can filter out 90% of log entries related to known issues and focus on unknown issues.
    To resolve the preceding issues, Log Service provides the following closed-loop solutions:

    -   Contextual query: queries the context of a log entry in the raw log file and displays the results on multiple pages. You do not need to log on to the server to query the context.
    -   LiveTail: uses the tail-f command to display raw log entries in real time.
    -   LogReduce: dynamically groups logs based on different patterns to detect exceptions.
    -   [LiveTail](/intl.en-US/Index and query/Query/LiveTail.md)

        In the traditional O&amp;M model, you must run the `tail -f` command on the server to monitor logs in real time. If you want a more specific result, run the `grep` or `grep -v` command to filter data by keyword. LiveTail provided in the Log Service console allows you to monitor and analyze online log data in real time. This reduces your O&amp;M workloads.

        ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3923961951/p37745.png)

        LiveTail supports the following features:

        -   Supports data collected from Docker and Kubernetes containers, servers, Log4j Appenders, and other data sources.
        -   Monitors log data in real time, and allows you to filter data by keyword.
        -   Delimits log fields to facilitate the queries for log entries that contain specific delimiters.
    -   [LogReduce](/intl.en-US/Index and query/Query/LogReduce.md)

        Large volumes of log data generated every day by rapid business development has created the following issues:

        -   Potential system exceptions are difficult to identify.
        -   Suspicious logons by intruders cannot be detected in real time.
        -   System behavior changes caused by version updates cannot be detected due to large amounts of information. In addition, recorded logs have various formats and have no topics. Therefore, the logs are difficult to group. **LogReduce** provided in Log Service can group logs based on different patterns and provide an overview of the logs. LogReduce supports the following features:
            -   Various formats of logs such as Log4j logs, JSON-formatted logs, and syslog logs are supported.
            -   Before logs are grouped, these logs can be filtered based on specified conditions. Raw log entries can be retrieved based on the signature of log entries grouped in a specific pattern.
            -   Compares the patterns of different time periods.
            -   The precision of log grouping can be adjusted based on your business requirements.
            -   Hundreds of millions of log entries can be grouped in seconds.

## Analysis capabilities

Elasticsearch supports data aggregation based on the doc values. Elasticsearch 6.x supports data grouping and aggregation by using the SQL syntax. Log Service supports the RESTful API and JDBC API and is compatible with the SQL-92 standard. Log Service supports complete SQL statements, including basic aggregate functions. In addition, Log Service allows you to perform JOIN operations on internal and external data sources, and implement machine learning and pattern analysis.

**Note:** The following examples compare the analysis capabilities of the ELK and Log Service. For more information, see [ES 6.5 Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html) and [Real-time log analysis](/intl.en-US/Index and query/Real-time log analysis.md).

In addition to SQL-92 standard functions, Log Service also provides the following features that are specific to log analysis scenarios:

-   [Interval-valued comparison and periodicity-valued comparison functions](/intl.en-US/Index and query/Analysis grammar/Interval-valued comparison and periodicity-valued comparison functions.md)

    You can nest the interval-valued comparison and periodicity-valued comparison functions in SQL statements. You can use this method to calculate the changes of a single field value, multiple field values, and curves in different time windows.

    ```
    * | select compare( pv , 86400) from (select count(1) as pv from log)
    ```

    ```
    *|select t, diff[1] as current, diff[2] as yestoday, diff[3] as percentage from(select t, compare( pv , 86400) as diff from (select count(1) as pv, date_format(from_unixtime(__time__), '%H:%i') as t from log group by t) group by t order by t) s 
    ```

-   [Join internal and external data sources for data query](/intl.en-US/Index and query/Analysis grammar/Use a JOIN statement to query data from a Logstore and a MySQL database.md)

    You can join the data in Log Service with external data sources for data query and analysis. The following list shows the supported data sources and JOIN operations:

    -   You can perform JOIN operations on the data in Logstores, MySQL databases, and OSS buckets \(CSV files\).
    -   You can perform LEFT OUTER JOIN, RIGHT OUTER JOIN, FULL OUTER JOIN, and INNER JOIN operations on data.
    -   You can use SQL statements to query data in external tables and join data in Log Service with external tables.
    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4923961951/p37750.png)

    The following example provides further details about how to join the data in Log Service with external tables:

    ```
    sql
    To create an external table, execute the following query statement:
    * | create table user_meta ( userid bigint, nick varchar, gender varchar, province varchar, gender varchar,age bigint) with ( endpoint='oss-cn-hangzhou.aliyuncs.com',accessid='LTA288',accesskey ='EjsowA',bucket='testossconnector',objects=ARRAY['user.csv'],type='oss')
    
    To join the data in Log Service with the external table, execute the following query statement:
    * | select u.gender, count(1) from chiji_accesslog l join user_meta1 u on l.userid = u.userid group by u.gender
    ```

-   Geolocation functions

    You can use the built-in geolocation functions to identify users based on IP addresses and mobile phone numbers. The following list shows the available geolocation functions:

    -   IP functions: identify the country, province, city, longitude and latitude, and Internet service provider \(ISP\) of an IP address.
    -   Phone number functions: identify the ISP, province, and city where a mobile phone number is registered.
    -   Geohash functions: encode the longitude and latitude of a location.
    The following example provides further details about how to use geolocation functions in query statements:

    ```
    sql
    * | SELECT count(1) as pv, ip_to_province(ip) as province WHERE ip_to_domain(ip) != 'intranet' GROUP BY province ORDER BY pv desc limit 10
    
    * | SELECT mobile_city(try_cast("mobile" as bigint)) as "city", mobile_province(try_cast("mobile" as bigint)) as "province", count(1) as "number of requests" group by "province", "city" order by "number of requests" desc limit 100
    ```

    -   [GeoHash](/intl.en-US/Index and query/Analysis grammar/Geo functions.md)
    -   [IP functions](/intl.en-US/Index and query/Analysis grammar/IP functions.md)
-   [Security check functions](/intl.en-US/Index and query/Analysis grammar/Security check functions.md)

    Log Service provides security check functions based on the globally shared asset library of WhiteHat Security. You can use security check functions to check whether an IP address, domain name, or the URL of a log is secure.

    -   security\_check\_ip
    -   security\_check\_domain
    -   security\_check\_url
-   [Machine learning](/intl.en-US/Index and query/Machine learning syntax and functions/Overview.md) and time series functions

    Log Service provides machine learning and intelligent diagnostic functions. This can be used to perform the following operations:

    -   Automatically learns historical data regularities and predicts the future trends.
    -   Detects imperceptible exceptions in real time, and combines analysis functions to analyze error causes.
    -   Intelligently detects exceptions and inspects the system based on the periodicity-valued comparison function and alerting feature. This feature provides an efficient method to analyze data in scenarios, such as intelligent O&amp;M, security, and operations.
    Machine learning and intelligent diagnostic functions provide the following features:

    -   Prediction: fits a baseline based on the historical data.
    -   Exception detection, change point detection, and inflection point detection: detect exceptions.
    -   Multi-period detection: detects the periodicity of time series data.
    -   Time series clustering: finds time series curves that have different curve shapes.
-   Pattern analysis functions

    Pattern analysis functions provide a fast and efficient method to detect data patterns and identify issues. You can use pattern analysis functions to perform the following operations:

    -   Identifies patterns that frequently occur. For example, you can use pattern analysis functions to identify the ID of the user who has sent 90% of invalid requests.
    -   Identifies the factor that most affects two patterns.
        -   In requests whose latency is greater than 10 seconds, the ratio of combined dimensions that contain an ID is significantly higher than the ratio of other combined dimensions.
        -   The ratio of the ID in Pattern B is lower than the ratio in Pattern A.
        -   Patterns A and B are significantly different from each other.
    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4923961951/p37754.png)


## Performance

The following examples compare the data write, query, and aggregation performance of Log Service and the ELK stack by using the same dataset.

-   Test environment
    -   Test configurations

        |Item|Self-managed ELK stack|Log Service|
        |:---|:---------------------|:----------|
        |Runtime environment|Four Elastic Compute Service \(ECS\) instances, each configured with four CPU cores, 16 GB of memory, and ultra disks or standard SSDs|-|
        |Shard|10|10|
        |Copies|2|3 \(the default value that is invisible to users\)|

    -   Test data

        -   Five columns of the double data type, five columns of the long data type, and five columns of the text data type are tested. The test data is displayed in 256, 512, 768, 1,024, and 1,280 dictionaries.
        -   Fields are randomly sampled from the test data, as shown in the following figure.
        -   Raw data size: 50 GB.
        -   Number of raw log entries: 162,640,232 \(about 160 million\).
        The following example provides further details of the sample test log entry:

        ```
        timestamp:August 27th 2017, 21:50:19.000 
        long_1:756,444 double_1:0 text_1:value_136 
        long_2:-3,839,872,295 double_2:-11.13 text_2:value_475 
        long_3:-73,775,372,011,896 double_3:-70,220.163 text_3:value_3 
        long_4:173,468,492,344,196 double_4:35,123.978 text_4:value_124 
        long_5:389,467,512,234,496 double_5:-20,10.312 text_5:value_1125
        ```

-   Test data write operations

    The Bulk API is called to write batch data to the ELK stack and the PostLogstoreLogs API is called to write batch data to Log Service. The following table includes a list that is used to compare the test results.

    |Item|Sub-item|Self-managed ELK stack|Log Service|
    |:---|:-------|:---------------------|:----------|
    |Latency|Average write latency|40 ms|14 ms|
    |Storage|Data volume of a copy|86G|58G|
    |Expansion rate: Data volume/Raw data size|172%|116%|

    **Note:** The storage fee incurred in Log Service for 50 GB of data includes the fee that is incurred for writing 23 GB of compressed data. It also includes the fee that is incurred for writing 27 GB of indexes.

    Comparison results:

    -   Log Service has a lower data write latency than the ELK stack.
    -   The size of the raw data is 50 GB. The size of the stored data exceeds 50 GB because the test data is random. In most scenarios, the size of the stored data after compression is smaller than the size of the raw data. The data stored in the self-managed ELK stack expands to 86 GB. In this case, the expansion rate is 172%, which is 58% higher than that in Log Service. This expansion rate is close to the recommended value 220% when you write new data to the ELK stack.
-   Test data read operations \(query and analysis\)
    -   Test scenarios

        Log query and aggregation are used as example scenarios. The average latency is calculated in the two scenarios when the number of concurrent read requests is 1, 5, and 10. The following examples provide further details of the two scenarios.

        -   Use aggregate functions \(AVG, MIN, MAX, SUM, and COUNT\) on the five columns of the long data type and group the values of five numeric columns. Then, sort the calculated values by the COUNT value to obtain the first 1,000 rows of data. You can execute the following query statement:

            ```
            select count(long_1) as pv,sum(long_2),min(long_3),max(long_4),sum(long_5) 
              group by text_1 order by pv desc limit 1000
            ```

        -   Use a keyword, for example, value\_126, to obtain the number of log entries that contain the keyword, and display the first 100 rows of data. You can execute the following query statement:

            ```
            value_126
            ```

    -   Test results

        |Scenario|Number of concurrent read requests|Latency of the self-managed ELK stack \(Unit: seconds\)|Latency of Log Service \(Unit: seconds\)|
        |:-------|:---------------------------------|:------------------------------------------------------|:---------------------------------------|
        |Log analysis|1|3.76|3.4|
        |5|3.9|4.7|
        |10|6.6|7.2|
        |Log query|1|0.097|0.086|
        |5|0.171|0.083|
        |10|0.2|0.082|

    -   Comparison results:
        -   Both Log Service and the ELK stack can query and analyze 160 million log entries within seconds.
        -   In the log analysis scenario, the latency is similar between Log Service and the ELK stack. The ELK stack uses SSDs and delivers better I/O performance than Log Service when a large amount of data is read.
        -   In the log query scenario, Log Service has a much shorter latency than the ELK stack. When the number of concurrent requests increases, the latency of the ELK stack increases. However, the latency of Log Service remains stable and slightly decreases.

## Capacity

-   Log Service allows you to index petabytes of data per day and query dozens of terabytes of data within seconds at a time. Log Service supports elastic scaling and scale-out for the processing scale.
-   The ELK stack is applicable to scenarios where data is written in units of GB to TB per day and stored in units of TB. The processing capacity is limited by the following factors:
    -   The size of a cluster: A cluster that consists of about 20 nodes has optimal performance. A large cluster in the industry can contain 100 nodes and is often split into multiple clusters for data processing.
    -   Write capacity: The number of shards cannot be modified after the shards are created. Therefore, the maximum number of available nodes cannot be increased if more write capacity is required due to the increasing throughput.
    -   Storage capacity: If the data size in the primary shard reaches the maximum disk capacity, you must migrate the shard to a larger disk, or allocate more shards to the current disk. In this case, you can create an index, specify more shards, and rebuild existing data.

Use case

Customer A is one of the major news websites in China. This customer has thousands of servers and hundreds of developers. The O&amp;M team used an ELK cluster to process NGINX logs. However, the cluster frequently fails to process large amounts of data.

-   When a user queries a large amount of data, the cluster is unavailable to other users.
-   The cluster is fully occupied during peak hours to collect and process data. This compromises data integrity and the accuracy of query results.
-   When the business grows, out of memory \(OOM\) errors frequently occur due to invalid memory settings and heartbeat synchronization failures. As a result, the query results are unavailable and inaccurate, and the cluster is inoperable for developers.

In June 2018, the O&amp;M team started to use Log Service to fix the preceding issues.

1.  The team used Logtail to collect online logs, and called the API to integrate log collection and server management configurations into the O&amp;M system.
2.  The team embedded the query page of Log Service into the unified logon and O&amp;M platform to isolate business permissions from account permissions.
3.  The team embedded the console page of Log Service into the platform of customer A. This way, the development team can use an efficient method to query logs. The team also configured Grafana plug-ins to monitor business and configured DataV to create dashboards in Log Service.

**Note:** For more information, see the following topics:

-   [Embed console pages and share log data](/intl.en-US/Developer Guide/Other visualization methods/Embed console pages and share log data.md)
-   [Connect to Log Service by using Grafana](/intl.en-US/Developer Guide/Other visualization methods/Connect to Log Service by using Grafana.md)
-   [Connect Log Service with DataV](/intl.en-US/Developer Guide/Other visualization methods/Connect Log Service with DataV.md)
-   [Connect Log Service with Jaeger](/intl.en-US/Developer Guide/Other visualization methods/Connect Log Service with Jaeger.md)

After two months, the team improved O&amp;M in the following aspects:

-   The number of queries per day was significantly increased. Developers used the O&amp;M platform to query and analyze logs. This improved the efficiency of development. The O&amp;M team also revoked the permissions of online logon.
-   In addition to NGINX logs, the O&amp;M platform also imported application logs, mobile device logs, and container logs into Log Service. The amount of processed data increased by 9 times.
-   More applications were developed. For example, Jaeger plug-ins were integrated with Log Service to build a log tracing system. Alerts and charts were configured to detect online errors in real time.
-   Various platforms are connected with the unified O&amp;M platform that provides a centralized method to collect data. This prevents repeated data collections. In addition, the Spark and Flink platforms of the big data team can consume log data in real time.

## Conclusion

Elasticsearch applies to scenarios where you want to update, query, and delete data. Therefore, Elasticsearch is widely used in fields such as data query, data analysis, and application development. The ELK stack applies to log analysis scenarios because it can maximize the flexibility and performance of Elasticsearch. Log Service is designed for log data query and analysis scenarios by providing multiple unique features. The ELK stack covers a wider range of scenarios. However, Log Service provides deeper analysis features for specific scenarios. You can select one of the two services based on your business requirements.

