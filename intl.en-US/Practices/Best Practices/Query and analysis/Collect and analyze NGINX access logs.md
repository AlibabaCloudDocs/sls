# Collect and analyze NGINX access logs

Log service allows you to collect and analyze NGINX access logs. This topic describes how to monitor, analyze, diagnose, and optimize access to a website.

Log data is collected. For more information, see [Collect NGINX logs](/intl.en-US/Data Collection/Logtail collection/Text logs/Collect NGINX logs.md).

The indexing feature is enabled and configured. For more information, see [Enable and configure the index feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

NGINX is a free, open-source, and high-performance HTTP server that you can use to build and host websites. NGINX access logs can be collected and analyzed. In traditional methods such as CNZZ, a JavaScript script is inserted into the frontend page of a website and is triggered when a user visits the website. However, this method can record only access requests. Stream computing, offline computing, and offline analysis can also be used to analyze NGINX access logs. However, those methods require a dedicated environment and it can be difficult to balance time efficiency and flexibility during log analysis.

In the Log Service console, you can create a collection configuration to collect NGINX access logs by using the data import wizard. Then, Log Service creates indexes and an NGINX dashboard to help you collect and analyze NGINX access logs. The dashboard displays metrics such as Distribution of IP Addresses, HTTP Status Codes, Request Methods, page view \(PV\) and unique visitor \(UV\) Statistics, Inbound and Outbound Traffic, User Agents, Top 10 Request URLs, Top 10 URIs by Number of Requests, and Top 10 URIs by Request Latency. You can use query statements to analyze the access latency of your website and optimize the performance of your website at the earliest opportunity. You can create alerts to track performance issues, server errors, and traffic changes. If the trigger conditions of an alert are met, alert notifications are sent to the specified recipients.

## Analyze access to a website

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click a project.

3.  In the left-side navigation pane, choose **Log Management** \> **Logstores**. Find the Logstore and click the **\>** icon next to it.

4.  Click the \> icon next to Visual Dashboards, and then click **LogstoreName\_Nginx\_access\_log**.

    The dashboard displays the following metrics:

    -   **Distribution of IP Addresses**: collects statistics on the distribution of IP addresses by executing the following SQL statement:

        ```
        * | select count(1) as c, ip_to_province(remote_addr) as address group by address limit 100
        ```

        ![Distribution of IP Addresses](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3161631061/p5898.png)

    -   **HTTP Status Codes**: calculates the percentage of each HTTP status code returned in the last 24 hours by executing the following SQL statement:

        ```
         * | select count(1) as pv,
                 status
                 group by status
        ```

        ![HTTP Status Codes](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4701635751/p5901.png)

    -   **Request Methods**: calculates the percentage of each request method used in the last 24 hours by executing the following SQL statement:

        ```
        * | select count(1) as pv ,request_method group by request_method
        ```

        ![Request Methods](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4701635751/p5900.png)

    -   **User Agents**: calculates the percentage of each user agent used in the last 24 hours by executing the following SQL statement:

        ```
        * | select count(1) as pv, case when http_user_agent like '%Chrome%' then 'Chrome' when http_user_agent like '%Firefox%' then 'Firefox' when http_user_agent like '%Safari%' then 'Safari' else 'unKnown' end as http_user_agent  group by case when http_user_agent like '%Chrome%' then 'Chrome' when http_user_agent like '%Firefox%' then 'Firefox' when http_user_agent like '%Safari%' then 'Safari' else 'unKnown' end   order by pv desc limit 10
        ```

        ![User Agents](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5701635751/p5902.png)

    -   **Top 10 Request URLs**: indicates the top 10 request URLs with the most PVs in the last 24 hours by executing the following SQL statement:

        ```
        * | select count(1) as pv , http_referer  group by http_referer order by pv desc limit 10
        ```

        ![Top 10 Request URLs](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5701635751/p5903.png)

    -   **Inbound and Outbound Traffic**: collects statistics on the inbound and outbound traffic by executing the following SQL statement:

        ```
        * | select sum(body_bytes_sent) as net_out, sum(request_length) as net_in ,date_format(date_trunc('hour', __time__), '%m-%d %H:%i')  as time group by date_format(date_trunc('hour', __time__), '%m-%d %H:%i') order by time limit 10000
        ```

        ![Inbound and Outbound Traffic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3161631061/p164544.png)

    -   **PV and UV Statistics**: calculates the number of PVs and UVs by executing the following SQL statement:

        ```
        *| select approx_distinct(remote_addr) as uv ,count(1) as pv , date_format(date_trunc('hour', __time__), '%m-%d %H:%i')  as time group by date_format(date_trunc('hour', __time__), '%m-%d %H:%i')  order by time limit 1000
        ```

        ![PV and UV Statistics](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4701635751/p5897.png)

    -   **Predicted PV**: predicts the number of PVs in the next 4 hours by executing the following SQL statement:

        ![Predicted PV](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3161631061/p164557.png)

    -   **Top 10 URLs by Number of Requests**: indicates the top 10 requested URLs with the most PVs in the last 24 hours by executing the following SQL statement:

        ```
        * | select count(1) as pv, split_part(request_uri,'?',1) as path  group by path order by pv desc limit 10
        ```

        ![Top 10 URLs by Number of Requests](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4701635751/p5899.png)


## Diagnose and optimize access to a website

In addition to some default access metrics, you must also diagnose access requests based on NGINX access logs. This allows you to locate requests that have high latency on specific pages. You can use the quick analysis feature on the Search & Analysis page. For more information, see [Query logs](/intl.en-US/Index and query/Query logs.md).

-   Count the average latency and highest latency every 5 minutes to obtain the overall latency by executing the following SQL statement:

    ```
      * | select from_unixtime(__time__ -__time__% 300) as time, 
              avg(request_time) as avg_latency ,
              max(request_time) as max_latency  
              group by __time__ -__time__% 300
    ```

-   Locate the requested page with the highest latency and optimize the response speed of the page by executing the following SQL statement:

    ```
      * | select from_unixtime(__time__ - __time__% 60) , 
              max_by(request_uri,request_time)  
              group by __time__ - __time__%60
    ```

-   Divide all the requests into 10 groups by access latency and count the number of requests based on different latency ranges by executing the following SQL statement:

    ```
    * |select numeric_histogram(10,request_time)
    ```

-   Count the top 10 requests with the highest latency and the latency of each request by executing the following SQL statement:

    ```
    * | select max(request_time,10)
    ```

-   Optimize the requested page with the highest latency.

    Assume that the /url2 page has the highest latency. To optimize the response speed of the /url2 page, count the following metrics for the /url2 page: number of PVs and UVs, number of times that each request method is used, number of times that each HTTP status code is returned, number of times that each browser type is used, average latency, and highest latency.

    ```
       request_uri:"/url2" | select count(1) as pv,
              approx_distinct(remote_addr) as uv,
              histogram(method) as method_pv,
              histogram(status) as status_pv,
              histogram(user_agent) as user_agent_pv,
              avg(request_time) as avg_latency,
              max(request_time) as max_latency
    ```


## Create alert rules

You can create alert rules to track performance issues, server errors, and traffic changes. For more information, see [Create an alert rule](/intl.en-US/Query and visualization/Alarm/Create an alert rule.md).

-   Server alerts

    You need to focus on server errors whose HTTP status code is 500. You can execute the following SQL statement to query the error number c per unit time and set the trigger condition of the alert rule to c \> 0.

    ```
    status:500 | select count(1) as c
    ```

    **Note:** For services with high access traffic, 500 errors could occur on occasion. In this case, you can set the **Notification Trigger Threshold** parameter to 2. It indicates that an alert is triggered only when conditions are met two consecutive times.

    ![Server alerts](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3161631061/p164693.png)

-   Performance alerts

    You can create alert rules if the latency increases when the server is running. For example, you can calculate the latency of all the write requests `Post` of the operation `/adduser` by executing the following SQL statement. Then set the alert rule to l \> 300000. It indicates that an alert is sent when the average latency exceeds 300 ms.

    ```
    Method:Post and URL:"/adduser" | select avg(Latency) as l
    ```

    You can use the average latency value to create an alert. However, high latency values are averaged to lower values, so this might not reflect the true situation. You can use the percentile in mathematical statistics \(the highest latency is 99%\) as the trigger condition.

    ```
    Method:Post and URL:"/adduser" | select approx_percentile(Latency, 0.99) as p99
    ```

    You can calculate the latency of every minute in a day \(1,440 minutes\), the 50th percentile latency, and the 90th percentile latency.

    ```
    * | select avg(Latency) as l, approx_percentile(Latency, 0.5) as p50, approx_percentile(Latency, 0.99) as p99, date_trunc('minute', time) as t group by t order by t desc limit 1440
    ```

    ![Check whether performance issues exist](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0758600061/p32474.png)

-   Traffic alerts

    A sudden decrease or increase of traffic in a short time period is abnormal. You can calculate the traffic change ratio and create alert rules to monitor sudden traffic changes. Sudden traffic changes are detected based on the following metrics:

    -   Previous time period: compares data in the current time period with that in the previous time period.
    -   Same time period on the previous day: compares data in the current time period with that of the same time period of the previous day.
    -   Same time period of the previous week: compares data in the current time period with that of the same time period of the previous week.
    Last window is used in the following example to calculate the traffic change ratio. In this example, the time range is set to 5 minutes.

    1.  Define a calculation window.

        Define a window of 1 minute to calculate the inbound traffic size of this minute.

        ```
        * | select sum(inflow)/(max(__time__)-min(__time__)) as inflow , __time__-__time__%60  as window_time from log group by window_time order by window_time limit 15
        ```

        The result indicates that the average inbound traffic is evenly distributed in every window.

        ![Define a calculation window](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8884688951/p32476.png)

    2.  Calculate value differences in the window.
        -   Calculate the difference between the maximum or minimum traffic size and the average traffic size in the window. The max\_ratio metric is used as an example.

            The calculated max\_ratio is 1.02. You can set the alert rule to max\_ratio \> 1.5. It indicates that an alert is sent when the change ratio exceeds 50%.

            ```
             * | select max(inflow)/avg(inflow) as max_ratio from (select sum(inflow)/(max(__time__)-min(__time__)) as inflow , __time__-__time__%60  as window_time from log group by window_time order by window_time limit 15) 
            ```

            ![Calculate value differences in the window](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0758600061/p32477.png)

        -   Calculate the latest\_ratio metric to check whether the latest value fluctuates.

            Use the max\_by function to calculate the maximum traffic size in the window. In this example, lastest\_ratio is 0.97.

            ```
             * | select max_by(inflow, window_time)/1.0/avg(inflow) as lastest_ratio from (select sum(inflow)/(max(__time__)-min(__time__)) as inflow , __time__-__time__%60  as window_time from log group by window_time order by window_time limit 15) 
            ```

            **Note:** The calculation result of the max\_by function is of the character type. It must be converted to the numeric type. To calculate the relative ratio of changes, you can replace the SELECT clause with \(1.0-max\_by\(inflow, window\_time\)/1.0/avg\(inflow\)\) as lastest\_ratio.

            ![Calculate value differences in the window](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0758600061/p32478.png)

        -   Calculate the fluctuation ratio. It is the change ratio between the current value and the previous value of the window.

            ![Calculate value differences in the window](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0758600061/p32479.png)

            Use the window function \(lag\) for calculation. Extract the current inbound traffic and the inbound traffic of the previous cycle to calculate the difference by using lag\(inflow, 1, inflow\)over\(\). Then, divide the calculated difference value by the current value to obtain the change ratio. In this example, a relatively major decrease occurs in traffic at 11:39, with a change ratio of more than 40%.

            **Note:** To define an absolute change ratio, you can use the ABS function to calculate the absolute value and unify the calculation result.

            ```
             * | select (inflow- lag(inflow, 1, inflow)over() )*1.0/inflow as diff, from_unixtime(window_time) from (select sum(inflow)/(max(__time__)-min(__time__)) as inflow , __time__-__time__%60  as window_time from log group by window_time order by window_time limit 15) 
            ```

            ![Calculate value differences in the window (2)](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0758600061/p32480.png)


