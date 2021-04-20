# Query and analyze website logs

This topic describes how to query and analyze website logs in the Log Service console.

Website access logs are collected. For more information, see [Collect logs in full regex mode](/intl.en-US/Data Collection/Logtail collection/Text logs/Collect logs in full regex mode.md).

## Step 1: Configure indexes

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the project in which you want to query and analyze logs.

3.  On the **Log Storage** \> **Logstores** tab, click the Logstore where logs are stored.

4.  On the Search & Analysis page of the Logstore, choose **Index Attributes** \> **Attributes**.

    If the indexing feature is not enabled, click **Enable**.

5.  Configure field indexes.

    You can configure indexes in sequence. You can also click **Automatic Index Generation**. Then, Log Service automatically configures indexes based on the first log entry in the Preview Data section.

    **Note:**

    -   The indexing feature is applicable only to the log data that is written to the current Logstore after you configure indexes. If you want to query historical data, you can use the reindexing feature. For more information, see [Reindex logs for a Logstore](/intl.en-US/Index and query/Query/Reindex logs for a Logstore.md).
    -   If you want to use the analysis feature, you must turn on the Enable Analytics switch for the related fields when you configure indexes.
    -   By default, indexes are automatically configured for some reserved fields in Log Service. For more information, see [Reserved fields](/intl.en-US/Product Introduction/Limits/Reserved fields.md).
    ![Field Search section](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1133466161/p210834.png)

6.  Click **OK**.


## Step 2: Query logs

On the Search & Analysis page of the Logstore, enter a search statement in the search box and select a time range. Then, click **Search & Analyze**.

**Note:** The format of a query statement in Log Service is `Search statement | Analytic statement`. A search statement can be executed alone. However, an analytic statement must be executed together with a search statement. The analysis feature is based on search results or all data in the Logstore.

-   To query the log entries that contain Chrome, execute the following search statement:

    ```
    Chrome
    ```

-   To query the log entries whose request duration is greater than 60 seconds, execute the following search statement:

    ```
    request_time > 60
    ```

-   To query the log entries whose request duration ranges from 60 seconds to 120 seconds, execute the following search statement:

    ```
    request_time in [60 120]
    ```

-   To query the log entries that contain successful GET requests \(status code: 200 to 299\), execute the following search statement:

    ```
    request_method : GET and status in [200 299]
    ```

-   To query the log entries whose value of the request\_uri field is /request/path-2, execute the following search statement:

    ```
    request_uri:/request/path-2/file-2
    ```


## Step 3: Analyze logs

On the Search & Analysis page of the Logstore, enter a query statement in the search box and select a time range. Then, click **Search & Analyze**.

**Note:** By default, only 100 rows of data are returned after you execute a query statement. You can use the LIMIT clause to change the number of returned rows. For more information, see [LIMIT syntax](/intl.en-US/Index and query/Analysis grammar/LIMIT syntax.md).

-   Calculate the page views \(PVs\) of a website.

    To calculate the PVs of a website, use the [COUNT function](/intl.en-US/Index and query/Analysis grammar/General aggregate functions.md) in a query statement.

    ```
    * | SELECT COUNT(*) AS PV
    ```

    ![PVs of a website](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1133466161/p224709.png)

-   Calculate the PVs of a website by 1 minute.

    Use the [date\_trunc function](/intl.en-US/Index and query/Analysis grammar/Date and time functions.md) to truncate a time by minute and use the GROUP BY clause to group analysis results by time. Then, use the [COUNT function](/intl.en-US/Index and query/Analysis grammar/General aggregate functions.md) to calculate the number of PVs per minute and use the ORDER BY clause to sort the analysis results by time.

    ```
    * | SELECT COUNT(*) as PV, date_trunc('minute', __time__) as time GROUP BY time ORDER BY time
    ```

    ![PVs of a website](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1133466161/p224710.png)

-   Calculate the number of requests for each request method by 5 minutes.

    Use \_\_time\_\_ - \_\_time\_\_ %300 to truncate a time by 5 minutes and use the GROUP BY clause to group analysis results by time. Then, use the [COUNT function](/intl.en-US/Index and query/Analysis grammar/General aggregate functions.md) to calculate the number of requests every 5 minutes and use the ORDER BY clause to sort the analysis results by time.

    ```
    * | SELECT request_method, COUNT(*) as count, __time__ - __time__ %300 as time GROUP BY time, request_method ORDER BY time
    ```

    ![Number of requests](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2133466161/p224711.png)

-   Compare the number of PVs of the current week with the number of PVs of the last week.

    Use the [COUNT function](/intl.en-US/Index and query/Analysis grammar/General aggregate functions.md) to calculate the total number of PVs. Then, use the [ts\_compare function](/intl.en-US/Index and query/Analysis grammar/Interval-valued comparison and periodicity-valued comparison functions.md) to obtain the ratio of the PVs of the current week to the PVs of the last week. website\_log in the following query statement is the Logstore name:

    ```
    * | SELECT diff[1] as this_week, diff[2] as last_week, time FROM (SELECT ts_compare(pv, 604800) as diff, time FROM (SELECT COUNT(*) as pv, date_trunc('week', __time__) as time FROM website_log GROUP BY time ORDER BY time) GROUP BY time)
    ```

    ![The ratio of the PVs of the current week to the PVs of the last week](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2133466161/p224805.png)

-   Collect the distribution statistics of client IP addresses.

    Use the [ip\_to\_province function](/intl.en-US/Index and query/Analysis grammar/IP functions.md) to obtain the province to which an IP address belongs, and use the GROUP BY clause to group analysis results by province. Then, use the [COUNT function](/intl.en-US/Index and query/Analysis grammar/General aggregate functions.md) to calculate the number of occurrences of each IP address, and use the ORDER BY clause to sort the analysis results by the number of occurrences.

    ```
    * | SELECT COUNT(*) as count, ip_to_province(client_ip) as address GROUP BY address ORDER BY count DESC
    ```

    ![Client distribution](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2133466161/p224714.png)

-   Calculate the top 10 accessed request URIs.

    Use the GROUP BY clause to group analysis results by request URI. Use the [COUNT function](/intl.en-US/Index and query/Analysis grammar/General aggregate functions.md) to calculate the number of access requests for each URI. Then, use the ORDER BY clause to sort the analysis results by the number of access requests.

    ```
    * | SELECT COUNT(*) as PV, request_uri as PATH GROUP BY PATH ORDER BY PV DESC LIMIT 10
    ```

    ![Request URI](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2133466161/p224715.png)

-   Query the log entries whose value of the request\_uri field ends with %file-7.

    **Note:** In query statements, the wildcard characters asterisk \(\*\) and question mark \(?\) are used for fuzzy searches. The wildcard characters must be used in the middle or at the end of a word. If you want to query fields that end with a specific character, you can use the LIKE operator in an analytic statement.

    ```
    * | select * from website_log where request_uri like '%file-7'
    ```

    website\_log in the preceding query statement is the Logstore name.

    ![Fuzzy search](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2133466161/p224716.png)

-   Calculate the statistics of request URIs that are accessed.

    Use the [regexp\_extract function](/intl.en-US/Index and query/Analysis grammar/Regular expression functions.md) to extract the file part from the request\_uri field. Then, use the [COUNT function](/intl.en-US/Index and query/Analysis grammar/General aggregate functions.md) to calculate the number of access requests for each URI.

    ```
    * | SELECT regexp_extract(request_uri, '.*\/(file.*)', 1) file, count(*) as count group by file
    ```

    ![Analyze URIs](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2133466161/p224727.png)

-   Query the log entries whose value of the request\_uri field contains %abc%.

    ```
    * | SELECT * where request_uri like '%/%abc/%%' escape '/'
    ```

    ![Fuzzy search](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1432198161/p240990.png)


## Sample logs

```
__tag__:__client_ip__:192.0.2.0
__tag__:__receive_time__:1609985755
__source__:198.51.100.0
__topic__:website_access_log
body_bytes_sent:4512
client_ip:198.51.100.10
host:example1.com
http_host:example2.com
http_user_agent:Mozilla/5.0 (Macintosh; U; PPC Mac OS X 10_5_8; ja-jp) AppleWebKit/533.20.25 (KHTML, like Gecko) Version/5.0.4 Safari/533.20.27
http_x_forwarded_for:198.51.100.1
instance_id:i-02
instance_name:instance-01
network_type:vlan
owner_id:%abc%-01
referer:example3.com
region:cn-shanghai
remote_addr:203.0.113.0
remote_user:neb
request_length:4103
request_method:POST
request_time:69
request_uri:/request/path-1/file-0
scheme:https
server_protocol:HTTP/2.0
slbid:slb-02
status:200
time_local:07/Jan/2021:02:15:53
upstream_addr:203.0.113.10
upstream_response_time:43
upstream_status:200
user_agent:Mozilla/5.0 (X11; Linux i686) AppleWebKit/534.33 (KHTML, like Gecko) Ubuntu/9.10 Chromium/13.0.752.0 Chrome/13.0.752.0 Safari/534.33
vip_addr:192.0.2.2
vpc_id:3db327b1****82df19818a72
```

