# Analyze website logs

You can use the SQL-92 syntax to analyze logs in Log Service. You can also visualize all query results on multiple types of charts, such as table, line chart, column chart, pie chart, flow chart, and map. This topic describes how to analyze website logs in the Log Service console and visualize the query results on charts.

-   Website logs are collected. For more information, see [Log collection methods](/intl.en-US/Data Collection/Log collection methods.md).
-   The indexing feature is enabled for the Logstore and indexes are configured. For more information, see [Enable and configure the index feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

Website logs include important statistical information about website operation and maintenance, such as page views \(PVs\), unique visitors \(UVs\), distribution of accessed regions, and top 10 accessed websites. Log Service provides multiple log collection methods and the log analysis feature. You can analyze logs in real time by using the query statements and SQL-92 syntax. You can also visualize the query results on charts. Log Service also allows you to visualize log analysis results on built-in dashboards or by using open-source visualization tools such as DataV, Grafana, Tableau through Java Database Connectivity \(JDBC\), and Quick BI.

![Architecture](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4270470061/p32503.png)

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click a project.

3.  On the **Log Management** \> **Logstores** tab, choose **![Management icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9484688951/p52166.png)** \> **Search & Analysis** to the right of a Logstore.

4.  In the upper-right corner of the page, click **15 Minutes \(Relative\)** to specify a time range for the query.

    You can select a relative time or time frame. You can also specify a custom time range.

    **Note:** The query results may contain logs that are generated 1 minute earlier or later than the specified time range.

5.  Enter a query statement in the search box, and then click **Search & Analyze**.

    Log Service provides multiple charts to display query results. For more information, see [Charts](/intl.en-US/Query and visualization/Analysis graph/Overview.md).

    -   Table. You can enter the following query statement to display the access statistics of the client IP addresses in the last day and sort the query results in descending order. The remote\_addr field indicates the IP address of a client that sends an access request.

        ```
        * | SELECT remote_addr, count(*) as count GROUP BY remote_addr ORDER BY count DESC
        ```

        ![Table](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4270470061/p32504.png)

    -   Line chart. You can enter the following query statement to display the statistics of PVs, UVs, and average response time in the last 15 minutes:

        ```
        * | select date_format(from_unixtime(__time__ - __time__% 60), '%H:%i:%S') as minutes, approx_distinct(remote_addr) as uv, count(1) as pv, avg(request_time) as avg group by minutes order by minutes asc limit 100000
        ```

        Select minutes for **X Axis**, select pv and uv for **Left Y Axis**, select avg for **Right Y Axis**, and select uv for **Column Marker**. The following figure shows the line chart and properties.

        ![Line chart](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4270470061/p32505.png)

    -   Column chart. You can enter the following query statement to display the number of the source URLs that are accessed in the last 15 minutes. The referer field indicates the HTTP referer header. This field includes the source URL information.

        ```
        * | select referer, count(1) as count group by referer
        ```

        Select referer for **X Axis** and select count for **Y Axis**. The following figure shows the column chart and properties.

        ![Column chart](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5270470061/p32507.png)

    -   Bar chart. You can enter the following query statement to display the statistics of the top 10 accessed websites in the last 15 minutes. The request\_uri field indicates the URI of a request.

        ```
        * | select  request_uri, count(1) as count group by request_uri order by count desc limit 10    
        ```

        Select request\_uri for **X Axis** and select count for **Y Axis**. The following figure shows the bar chart and properties.

        ![Bar chart](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5270470061/p32508.png)

    -   Pie chart. You can enter the following query statement to display the statistics of the accessed websites in the last 15 minutes. The request\_uri field indicates the URI of a request.

        ```
        * | select request_uri as uri , count(1) as c group by uri limit 10
        ```

        Select uri for **Legend Filter** and select c for **Value Column**. The following figure shows the pie chart and properties.

        ![Pie Chart](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5270470061/p32509.png)

    -   Single value chart. You can enter the following query statement to display the number of PVs in the last 15 minutes:

        ```
        * | select count(1) as PV
        ```

        Select PV for **Value Column**. The following figure shows the single value chart and properties.

        ![Single value chart](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5270470061/p32512.png)

    -   Area chart. You can enter the following query statement to display the access statistics of an IP address in the last day:

        ```
        remote_addr: 10.0.XX.XX | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i') as time, count(1) as PV group by time order by time limit 1000
        ```

        Select time for **X Axis**and select PV for **Y Axis**. The following figure shows the area chart and properties.

        ![Area chart](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5270470061/p32513.png)

    -   Map. You can enter the following query statements to display the top 10 locations to which the client IP addresses belong:
        -   Map of China

            ```
            * | select ip_to_province(remote_addr) as address, count(1) as count group by address order by count desc limit 10
            ```

            ![Map of China](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5270470061/p32514.png)

        -   World Map

            ```
            * | select ip_to_country(remote_addr) as address, count(1) as count group by address order by count desc limit 10
            ```

            ![World Map](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5270470061/p32515.png)

        -   AMap

            ```
            * | select ip_to_geo(remote_addr) as address, count(1) as count group by address order by count desc limit 10
            ```

            ![Amap](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5270470061/p32516.png)

    -   Flow chart. You can enter the following query statement to display the times trends of the request methods that are used in the last 15 minutes. The request\_method field indicates the method of an HTTP access request.

        ```
        * | select date_format(from_unixtime(__time__ - __time__% 60), '%H:%i:%S') as minute, count(1) as c, request_method group by minute, request_method order by minute asc limit 100000
        ```

        Select minute for **X Axis**, select c for **Y Axis**, and select request\_method for **Aggregate Column**. The following figure shows the flow chart and properties.

        ![Flow chart](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5270470061/p32518.png)

    -   Word cloud. You can enter the following query statement to display the statistics of the accessed websites in the last 15 minutes. The request\_uri field indicates the URI of a request.

        ```
        * | select request_uri as uri , count(1) as c group by uri limit 100
        ```

        Select c for **Word Column** and select c for **Value Column**. The following figure shows the word cloud and properties.

        ![Word cloud](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6270470061/p32520.png)

6.  Add an analysis chart to a dashboard.

    You can click **Add to New Dashboard** to perform this operation. For more information, see [Add an analysis chart to a dashboard](/intl.en-US/Query and visualization/Dashboard/Add an analysis chart to a dashboard.md).


