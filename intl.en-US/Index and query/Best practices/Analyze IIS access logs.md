# Analyze IIS access logs

Log service allows you to collect and analyze Internet Information Services \(IIS\) access logs. This topic describes how to monitor and analyze access to your website by using IIS access logs. You can obtain data such as page views \(PVs\), unique visitors \(UVs\), client IP distribution, error requests, and inbound and outbound traffic.

IIS logs are collected. For more information, see [Collect IIS logs](/intl.en-US/Data Collection/Logtail collection/Text logs/Collect IIS logs.md).

The indexing feature is enabled and configured. For more information, see [Enable and configure the index feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

IIS is a secure web server that you can use to build and host websites. When you use IIS to build a website, you can collect and analyze IIS access logs.

We recommend that you use the following IIS W3C Extended Log Format:

```
logExtFileFlags="Date, Time, ClientIP, UserName, SiteName, ComputerName, ServerIP, Method, UriStem, UriQuery, HttpStatus, Win32Status, BytesSent, BytesRecv, TimeTaken, ServerPort, UserAgent, Cookie, Referer, ProtocolVersion, Host, HttpSubStatus"
```

The following example shows how to obtain IIS logs in the IIS W3C Extended Log Format:

```
#Software: Microsoft Internet Information Services 7.5
#Version: 1.0
#Date: 2020-09-08 09:30:26
#Fields: date time s-sitename s-ip cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken
2009-11-26 06:14:21 W3SVC692644773 125.67.67. * GET /index.html - 80 - 10.10.10.10 Baiduspider+(+http://www.baidu.com)200 0 64 185173 296 0
```

-   Field prefixes

    |Prefix|Description|
    |:-----|:----------|
    |s-|Indicates a server action.|
    |c-|Indicates a client action.|
    |cs-|Indicates a client-to-server action.|
    |sc-|Indicates a server-to-client action.|

-   Fields

    |Field|Description|
    |:----|:----------|
    |date|The date on which the client sends the request.|
    |time|The time when the client sends the request.|
    |s-sitename|The Internet service name and instance number of the site visited by the client.|
    |s-computername|The name of the server on which the log entry is generated.|
    |s-ip|The IP address of the server on which the log entry is generated.|
    |cs-method|The HTTP request method used by the client, for example, GET or POST.|
    |cs-uri-stem|The URI resource requested by the client.|
    |cs-uri-query|The query string that follows the question mark \(?\) in the HTTP request.|
    |s-port|The port number of the server to which the client is connected.|
    |cs-username|The username that the client uses to access the server.Authenticated users are referenced as `domain\username`. Anonymous users are indicated by a hyphen \(-\). |
    |c-ip|The IP address of the client that sends the request.|
    |cs-version|The protocol version used by the client, for example, HTTP 1.0 or HTTP 1.1.|
    |cs\(User-Agent\)|The browser used by the client.|
    |Cookie|The content of the sent cookie or received cookie. A hyphen \(-\) is used if no cookie is sent or received.|
    |referer|The site that the client last visited.|
    |cs-host|The header name of the host.|
    |sc-status|The HTTP or FTP status code returned by the server.|
    |sc-substatus|The HTTP sub-status code returned by the server.|
    |sc-win32-status|The Windows status code returned by the server.|
    |sc-bytes|The number of bytes sent by the server.|
    |cs-bytes|The number of bytes received by the server.|
    |time-taken|The processing time of the request. Unit: milliseconds.|


1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click a project.

3.  On the **Log Management** \> **Logstores** tab, choose **![Management icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9484688951/p52166.png)** \> **Search & Analysis** to the right of a Logstore.

4.  In the upper-right corner of the page, click **15 Minutes \(Relative\)** to set a time range for the query.

    You can select a relative time, set a time frame, or customize a time range.

    **Note:** The query results may contain logs that are generated one minute earlier or later than the specified time range.

5.  Enter a query statement in the search box, and then click **Search & Analyze**.

    For more information, see [Statement format](/intl.en-US/Index and query/Overview.md).

    -   Collect statistics on the distribution of client IP addresses by executing the following SQL statement:

        ```
        | select ip_to_geo("c-ip") as country, count(1) as c group by ip_to_geo("c-ip") limit 100
        ```

    -   Calculate the number of PVs and UVs by executing the following SQL statement:

        ```
        *| select approx_distinct("c-ip") as uv ,count(1) as pv , date_format(date_trunc('hour', __time__), '%m-%d %H:%i') as time group by date_format(date_trunc('hour', __time__), '%m-%d %H:%i') order by time limit 1000
        ```

        ![PVs and UVs](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4188600061/p6673.png)

    -   Calculate the percentage of each HTTP status code returned by executing the following SQL statement:

        ```
        *| select count(1) as pv ,"sc-status" group by "sc-status"
        ```

        ![Percentages of HTTP status codes](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4188600061/p6674.png)

    -   Collect statistics on the inbound and outbound traffic by executing the following SQL statement:

        ```
        *| select sum("sc-bytes") as net_out, sum("cs-bytes") as net_in ,date_format(date_trunc('hour', time), '%m-%d %H:%i') as time group by date_format(date_trunc('hour', time), '%m-%d %H:%i') order by time limit 10000
        ```

        ![Inbound and outbound traffic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4188600061/p6675.png)

    -   Calculate the percentage of each request method by executing the following SQL statement:

        ```
        *| select count(1) as pv ,"cs-method" group by "cs-method"
        ```

        ![Percentages of request methods](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4188600061/p6676.png)

    -   Calculate the percentage of each browser type by executing the following SQL statement:

        ```
        *| select count(1) as pv, case when "user-agent" like '%Chrome%' then 'Chrome' when "user-agent" like '%Firefox%' then 'Firefox' when "user-agent" like '%Safari%' then 'Safari' else 'unKnown' end as "user-agent" group by case when "user-agent" like '%Chrome%' then 'Chrome' when "user-agent" like '%Firefox%' then 'Firefox' when "user-agent" like '%Safari%' then 'Safari' else 'unKnown' end order by pv desc limit 10
        ```

        ![Percentages of browser types](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4188600061/p6677.png)

    -   Calculate the top 10 pages that have the most PVs by executing the following SQL statement:

        ```
        *| select count(1) as pv, split_part("cs-uri-stem",'?',1) as path group by split_part("cs-uri-stem",'?',1) order by pv desc limit 10
        ```

        ![Top 10 most visited pages](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4188600061/p6678.png)


