# Connect to Log Service by using Grafana

This topic describes how to use Grafana to analyze and visualize NGINX logs that are collected by Log Service.

-   NGINX logs are collected. For more information, see [Collect logs in NGINX mode](/intl.en-US/Data Collection/Logtail collection/Text logs/Collect logs in NGINX mode.md).
-   The indexing feature is enabled and configured. For more information, see [Collect and analyze NGINX access logs](/intl.en-US/Index and query/Best practices/Collect and analyze NGINX access logs.md).

## Step 1: Install Grafana and plug-ins

1.  Install Grafana. For more information, see [Grafana documentation](http://docs.grafana.org/installation/).

    **Note:**

    -   Before you install Grafana on an on-premises machine, you must enable port 3000 in the browser.
    -   Before you use a pie chart, you can run the following command to install the pie chart plug-in:

        ```
        grafana-cli plugins install grafana-piechart-panel
        ```

2.  Install the Log Service plug-in.

    1.  Run the following command to go to the plug-in installation directory of Grafana.

        For example, install the plug-in in the /var/lib/grafana/plugins/ directory on your Ubuntu system.

        ```
        cd /var/lib/grafana/plugins/
        ```

    2.  Run the following command to install the plug-in:

        ```
        git clone https://github.com/aliyun/aliyun-log-grafana-datasource-plugin
        ```

    3.  Run the following command to restart the Grafana service:

        ```
        service grafana-server restart
        ```

3.  Optional. To install Grafana version 7.0 or later, modify the Grafana configuration file.

    1.  Open the configuration file.

        -   The file path in macOS is /usr/local/etc/grafana/grafana.ini.
        -   The file path in Linux is/etc/grafana/grafana.ini
    2.  Set the allow\_loading\_unsigned\_plugins parameter in plugins.

        ```
        allow_loading_unsigned_plugins = aliyun-log-service-datasource,grafana-log-service-datasource
        ```


## Step 2: Configure a data source

1.  Log on to Grafana.

2.  In the left-side navigation pane, choose **![G1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1222762061/p112522.png)** \> **Data Sources**.

3.  On the Data Sources tab, click **Add data source**.

4.  On the Add data source page, click the **Select** button on the **LogService** card.

5.  Configure the data source.

    The following table describes the required parameters.

    |Parameter|Description|
    |:--------|:----------|
    |Name|The name of the data source.|
    |HTTP|Set the URL, Access, and Whitelisted Cookies parameters.    -   URL: The format is `http://Endpoint`. Replace the Endpoint variable based on the actual scenario. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). Example: `http://cn-qingdao.log.aliyuncs.com`.
    -   Access: Select Server or Browser. Default value: Server.
    -   Whitelisted Cookies: Add a whitelist. |
    |Auth|Use the default settings.|
    |log service details|Specify a project name, Logstore name, and AccessKey pair that has the read permissions. To ensure the security of your Alibaba Cloud account, we recommend that you use the AccessKey pair of a RAM user.|

6.  Click **Save & Test**.


## Step 3: Add a dashboard

1.  In the left-side navigation pane, choose **![Icon 1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2914221161/p112912.png)** \> **Dashboards**.

2.  In the New Panel panel, click **Choose Visualization**.

3.  Configure template variables.

    You can configure template variables in Grafana. This way, different results can be displayed in the same chart if you select different variable values.

    1.  Configure a template variable for time ranges.

        1.  In the upper-right corner of the New dashboard page, select a time range and click the ![Icon 2](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2914221161/p112911.png) icon.
        2.  Click **Variables**.
        3.  Click **Add variable**.
        4.  Set the required parameters to configure a template variable. Then, click **Add**.

            The following table describes the required parameters.

            |Parameter|Description|
            |---------|-----------|
            |Name|The variable name, for example, myinterval. When you use a variable in a search condition, the variable name must be in the `$myinterval` format.|
            |Type|Select **Interval**.|
            |Lable|Enter **time interval**.|
            |Values|Enter **1m,10m,30m,1h,6h,12h,1d,7d,14d,30d** in the Values field.|
            |Auto Option|Turn on the **Auto Option** switch and use the default settings for other parameters.|

    2.  Configure a template variable for domains.

        1.  On the Variables page, click **New**.
        2.  Set the required parameters to configure a template variable. Then, click **Add**.

            |Parameter|Description|
            |:--------|:----------|
            |Name|The variable name, for example, hostname. When you use a variable in a search condition, the variable name must be in the `$hostname` format.|
            |Type|Select **Custom**.|
            |Lable|Enter a domain name.|
            |Custom Options|Enter `*,www.host.com,www.host0.com,www.host1.com` in the "Values separated by comma" field. This value indicates that you can view the access to all domains. You can also view the access to `www.host.com`, `www.host0.com`, or `www.host1.com`.|
            |Selection Options|Use the default settings.|

    3.  In the left-side navigation pane, click **Save**.

4.  Add visualized panels.

    -   Add a graph panel to show page view \(PV\) and unique visitor \(UV\) statistics.
        1.  Click the **![Icon 3 ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2914221161/p112908.png)** icon in the upper-right corner.
        2.  In the New Panel panel, click **Add Query**.
        3.  Click the **![Icon 4](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2914221161/p112921.png)** icon and select **Graph** from the **Visualization** drop-down list.
        4.  Click the **![Icon 5](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2914221161/p112926.png)** icon and enter **UV&PV** in the **Title** field.
        5.  Click the **![Icon 6](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2914221161/p112923.png)** icon, select **Logservice** from the **Query** drop-down list, and then set the required parameters. The following table describes the parameters.

            ![grafana-pv&uv01](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5105311161/p112935.png)

            |Parameter|Description|
            |:--------|:----------|
            |Query|The query statement. In this example, the following query statement is executed:            ```
$hostname| select approx_distinct(remote_addr) as uv ,count(1) as pv , __time__ - __time__ % $$myinterval as time group by time order by time limit 1000
            ```

In the query result, `$hostname` is replaced by a specified domain name, and `$$myinterval` is replaced by a specified time interval.

**Note:** Two dollar signs \($$\) precede myinterval and one dollar sign \($\) precedes hostname. |
            |X-Column|Enter **time**.|
            |Y-Column|Enter **uv,pv**.|

        6.  If a significant difference exists between the UV value and the PV value, you can use a dual Y-axis chart to show UV and PV statistics.

            ![grafana-pv&uv02](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5105311161/p112952.png)

        7.  Click the **![Save icon](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2914221161/p113049.png)** icon to save your settings.
    -   Add a graph panel to show inbound and outbound traffic.

        For more information about how to add a panel, see [Add a graph panel to show PV and UV statistics](#step_718_ird_p4y). The following table describes the related parameters.

        |Parameter|Description|
        |:--------|:----------|
        |Query|The query statement. In this example, the following query statement is executed:        ```
$hostname | select sum(body_byte_sent) as net_out, sum(request_length) as net_in,__time__ - __time__ % $$myinterval as time group by __time__ - __time__ % $$myinterval limit 10000
        ```

In the query result, `$hostname` is replaced by a specified domain name, and `$$myinterval` is replaced by a specified time interval.

**Note:** Two dollar signs \($$\) precede myinterval and one dollar sign \($\) precedes hostname. |
        |X-Column|Enter **time**.|
        |Y-Column|Enter **net\_in,net\_out**.|

    -   Add a pie chart panel to show the proportions of HTTP request methods.

        For more information about how to add a panel, see [Add a graph panel to show PV and UV statistics](#step_718_ird_p4y). The following table describes the related parameters.

        |Parameter|Description|
        |:--------|:----------|
        |Query|The query statement. In this example, the following query statement is executed:        ```
$hostname | select count(1) as pv ,method group by method
        ```

In the query result, `$hostname` is replaced by a specified domain name. |
        |X-Column|Enter **pie**.|
        |Y-Column|Enter **method,pv**.|

    -   Add a pie chart panel to show the proportions of HTTP request codes.

        For more information about how to add a panel, see [Add a graph panel to show PV and UV statistics](#step_718_ird_p4y). The following table describes the related parameters.

        |Parameter|Description|
        |:--------|:----------|
        |Query|The query statement. In this example, the following query statement is executed:        ```
$hostname | select count(1) as pv ,status group by status
        ```

In the query result, `$hostname` is replaced by a specified domain name. |
        |X-Column|Enter **pie**.|
        |Y-Column|Enter **status,pv**.|

    -   Add a pie chart panel to show the request sources of frequently accessed pages.

        For more information about how to add a panel, see [Add a graph panel to show PV and UV statistics](#step_718_ird_p4y). The following table describes the related parameters.

        |Parameter|Description|
        |:--------|:----------|
        |Query|The query statement. In this example, the following query statement is executed:        ```
$hostname | select count(1) as pv , referer group by referer order by pv desc
        ```

In the query result, `$hostname` is replaced by a specified domain name. |
        |X-Column|Enter **pie**.|
        |Y-Column|Enter **referer,pv**.|

    -   Add a table panel to show pages whose latency ranks in the top places.

        For more information about how to add a panel, see [Add a graph panel to show PV and UV statistics](#step_718_ird_p4y). The following table describes the related parameters.

        |Parameter|Description|
        |:--------|:----------|
        |Query|The query statement. In this example, the following query statement is executed:        ```
$hostname | select URL as top_latency_URL ,request_time order by request_time desc limit 10
        ```

In the query result, `$hostname` is replaced by a specified domain name. |
        |X-Column|You do not need to set this parameter.|
        |Y-Column|Enter **top\_latency\_url,request\_time**.|

    -   Add a table panel to show hot pages.

        For more information about how to add a panel, see [Add a graph panel to show PV and UV statistics](#step_718_ird_p4y). The following table describes the related parameters.

        |Parameter|Description|
        |:--------|:----------|
        |Query|The query statement. In this example, the following query statement is executed:        ```
$hostname | select count(1) as pv, split_part(URL,'?',1) as path group by split_part(URL,'?',1) order by pv desc limit 20
        ```

In the query result, `$hostname` is replaced by a specified domain name. |
        |X-Column|You do not need to set this parameter.|
        |Y-Column|Enter **path,pv**.|

    -   Add a table panel to show pages that have the most access requests to which a non-200 HTTP status code is returned.

        For more information about how to add a panel, see [Add a graph panel to show PV and UV](#step_718_ird_p4y). The following table describes the related parameters.

        |Parameter|Description|
        |:--------|:----------|
        |Query|The query statement. In this example, the following query statement is executed:        ```
$hostname not status:200| select count(1) as pv , url group by url order by pv desc
        ```

In the query result, `$hostname` is replaced by a specified domain name. |
        |X-Column|You do not need to set this parameter.|
        |Y-Column|Enter **uv,pv**.|

    -   Add a Singlestat panel to show average latency.

        For more information about how to add a panel, see [Add a graph panel to show PV and UV statistics](#step_718_ird_p4y). The following table describes the related parameters.

        |Parameter|Description|
        |:--------|:----------|
        |Query|The query statement. In this example, the following query statement is executed:        ```
$hostname | select avg(request_time) as response_time, avg(upstream_response_time) as upstream_response_time ,__time__ - __time__ % $$myinterval as time group by __time__ -  __time__ % $$myinterval limit 10000
        ```

In the query result, `$hostname` is replaced by a specified domain name, and `$$myinterval` is replaced by a specified time interval.

**Note:** Two dollar signs \($$\) precede myinterval and one dollar sign \($\) precedes hostname. |
        |X-Column|Enter **time**.|
        |Y-Column|Enter **upstream\_response\_time,response\_time**.|

    -   Add a logs panel to show detailed logs.

        For more information about how to add a panel, see [Add a graph panel to show PV and UV statistics](#step_718_ird_p4y). The following table describes the related parameters.

        **Note:** Each page can show a maximum of 100 log entries. The maximum value of the **Logs Per Page** parameter is 100.

    -   Add a pie chart panel to show client statistics.

        For more information about how to add a panel, see [Add a graph panel to show PV and UV statistics](#step_718_ird_p4y). The following table describes the related parameters.

        |Parameter|Description|
        |:--------|:----------|
        |Query|The query statement. In this example, the following query statement is executed:        ```
$hostname  | select count(1) as pv, case when  regexp_like(http_user_agent , 'okhttp') then 'okhttp' when  regexp_like(http_user_agent ,  'iPhone') then 'iPhone' when regexp_like(http_user_agent ,  'Android')  then 'Android' else 'unKnown' end as http_user_agent group by  http_user_agent order by pv desc limit 10
        ```

In the query result, `$hostname` is replaced by a specified domain name. |
        |X-Column|Enter **pie**.|
        |Y-Column|Enter **http\_user\_agent,pv**.|

    -   Add a graph panel to show the total number of requests for each HTTP code. The total number is calculated within 1 minute.

        For more information about how to add a panel, see [Add a graph panel to show PV and UV statistics](#step_718_ird_p4y). The following table describes the related parameters.

        |Parameter|Description|
        |:--------|:----------|
        |Query|The query statement. In this example, the following query statement is executed:        ```
$hostname  | select to_unixtime(time) as time,status,count from (select time_series(__time__, '1m', '%Y-%m-%d %H:%i', '0')  as time,status,count(*) as count from log group by status,time order by time limit 10000)
        ```

In the query result, `$hostname` is replaced by a specified domain name. |
        |X-Column|Enter time.|
        |Y-Column|Enter col1\#:\# col2. col1 is the aggregate column and col2 is one of the other columns.|

    -   Add a Worldmap panel to show the distribution of source IP addresses.

        For more information about how to add a panel, see [Add a graph panel to show PV and UV statistics](#step_718_ird_p4y). The following tables describe the related parameters.

        |Parameter|Description|
        |:--------|:----------|
        |Query|The query statement. In this example, the following query statement is executed:        ```
$hostname  | select   count(1) as pv ,geohash(ip_to_geo(arbitrary(remote_addr))) as geo,ip_to_country(remote_addr) as country  from log group by country having geo <>'' limit 1000
        ```

In the query result, `$hostname` is replaced by a specified domain name. |
        |X-Column|Enter **map**.|
        |Y-Column|Enter **country,geo,pv**.|

        |Parameter|Description|
        |:--------|:----------|
        |Location Data|Select **geohash**.|
        |Location Name Field|Enter **country**.|
        |geo\_point/geohash Field|Enter **geo**.|
        |Metric Field|Enter **pv**.|

5.  View the results.

    On the top of the Dashboard page, you can select a time range. You can also filter the results by time interval and hostname.


## FAQ

-   Where are Grafana logs stored?

    Grafana logs are stored in the following directories:

    -   macOS: /usr/local/var/log/grafana
    -   Linux: /var/log/grafana
-   What can I do if **aliyun-log-plugin\_linux\_amd64: permission denied** appears in logs?

    Grant the execute permissions on the dist/aliyun-log-plugin\_linux\_amd64 directory of the plug-in directory.


