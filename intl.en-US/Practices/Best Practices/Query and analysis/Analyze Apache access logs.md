# Analyze Apache access logs

Log Service allows you to collect Apache access logs to obtain data such as page views \(PVs\), unique visitors \(UVs\), IP address distribution, error requests, and client types. You can monitor and analyze access to your website by using Apache access logs.

Apache access logs are collected. For more information, see [Collect Apache logs](/intl.en-US/Data Collection/Logtail collection/Text logs/Collect Apache logs.md).

Indexes are created by using the data import wizard. For information about how to modify indexes, see [Enable and configure the index feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

Apache is a web server software that is used to build and host websites across platforms. Apache access logs can be collected and analyzed.

In the Log Service console, you can create a collection configuration to collect Apache access logs by using the data import wizard. Then, Log Service creates indexes and an Apache dashboard to collect and analyze Apache access logs. The dashboard displays metrics such as Distribution of IP Addresses, HTTP Status Codes, Request Methods, PV and UV Statistics, Inbound and Outbound Traffic, User Agents, Top 10 Request URLs, Top 10 URIs by Number of Requests, and Top 10 URIs by Request Latency.

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click a project.

3.  In the left-side navigation pane, choose **Log Management** \> **Logstores**. Find the Logstore and click the **\>** icon next to it.

4.  Click the \> icon next to Visual Dashboards, and then click **LogstoreName\_apache\_access\_log**.

    The dashboard displays the following metrics:

    -   **Distribution of IP Addresses**: indicates the distribution of IP addresses by executing the following SQL statement:

        ```
        * | select ip_to_province(remote_addr) as address, count(1) as c group by ip_to_province(remote_addr) limit 100
        ```

        ![Distribution of IP Addresses](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8266443061/p9389.png)

    -   **HTTP Status Codes**: indicates the percentage of each HTTP status code returned within the last day by executing the following SQL statement:

        ```
        * | select status, count(1) as pv group by status
        ```

        ![HTTP Status Codes](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8266443061/p9388.png)

    -   **Request Methods**: indicates the percentage of each request method used within the last day by executing the following SQL statement:

        ```
        * | select request_method, count(1) as pv group by request_method
        ```

        ![Request Methods](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8266443061/p9387.png)

    -   **PV and UV Statistics**: indicates the number of PVs and UVs by executing the following SQL statement:

        ```
        * | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i')  as time, count(1) as pv, approx_distinct(remote_addr) as uv group by date_format(date_trunc('hour', __time__), '%m-%d %H:%i') order by time limit 1000
        ```

        ![PV and UV Statistics](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8266443061/p9385.png)

    -   **Inbound and Outbound Traffic**: indicates the inbound and outbound traffic by executing the following SQL statement:

        ```
        * | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i') as time, sum(bytes_sent) as net_out, sum(bytes_received) as net_in group by time order by time limit 10000
        ```

        ![Inbound and Outbound Traffic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8266443061/p9391.png)

    -   **User Agents**: indicates the percentage of each user agent used within the last day by executing the following SQL statement:

        ```
        * | select case when http_user_agent like '%Chrome%' then 'Chrome' when http_user_agent like '%Firefox%' then 'Firefox' when http_user_agent like '%Safari%' then 'Safari' else 'unKnown' end as http_user_agent, count(1) as pv group by case when http_user_agent like '%Chrome%' then 'Chrome' when http_user_agent like '%Firefox%' then 'Firefox' when http_user_agent like '%Safari%' then 'Safari' else 'unKnown' end   order by pv desc limit 10
        ```

        ![User Agents](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8266443061/p9390.png)

    -   **Top 10 Request URLs**: indicates the top 10 request URLs that have the most PVs within the last day by executing the following SQL statement:

        ```
        * | select  http_referer, count(1) as pv group by http_referer order by pv desc limit 10
        ```

        ![Top 10 Request URLs](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8266443061/p10098.png)

    -   **Top 10 URLs by Number of Requests**: indicates the top 10 requested URLs that have the most PVs within the last day by executing the following SQL statement:

        ```
        * | select split_part(request_uri,'?',1) as path,  count(1) as pv group by split_part(request_uri,'?',1) order by pv desc limit 10
        ```

        ![Top 10 URLs by Number of Requests](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4826635751/p9386.png)

    -   **Top 10 URLs by Request Latency**: indicates the top 10 requested URLs with the highest latency within the last day by executing the following SQL statement:

        ```
        * | select request_uri as top_latency_request_uri,
                    request_time_sec 
                    order by request_time_sec desc limit 10 10
        ```

        ![Top 10 URLs by Request Latency](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4826635751/p10099.png)


