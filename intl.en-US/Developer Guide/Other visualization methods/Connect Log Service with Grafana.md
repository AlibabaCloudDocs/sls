# Connect Log Service with Grafana

This topic describes how to use Grafana to analyze and visualize NGINX logs that are collected by Log Service.

-   NGINX logs are collected. For more information, see [Collect NGINX logs](/intl.en-US/Data Collection/Logtail collection/Text logs/Collect NGINX logs.md).
-   The indexing feature is enabled and configured. For more information, see [Collect and analyze NGINX access logs](/intl.en-US/Practices/Best Practices/Collection/Collect and analyze NGINX access logs.md).

## Architecture

The following architecture shows the process from log collection to log analysis.

![the process from log collection to log analysis](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13155/15469401615856_en-US.png)

## Procedure

1.  Install Grafana. For more information, visit the [Official Grafana documentation](http://docs.grafana.org/installation/).

    Run the following commands to install Grafana on Ubuntu.

    ```
    wget https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana_4.5.2_amd64.deb
    sudo apt-get install -y adduser libfontconfig
    sudo dpkg -i grafana_4.5.2_amd64.deb
    ```

    To use a pie chart, run the following command to install the pie chart plug-in.

    ```
    grafana-cli plugins install grafana-piechart-panel
    ```

2.  Install the Log Service plug-in.

    Find the directory where Grafana plug-ins resides. Install Grafana plug-ins in the /var/lib/grafana/plugins/ directory of Ubuntu and restart the grafana-server service.

    Run the following commands to install plug-ins and restart the grafana-server service.

    ```
    cd /var/lib/grafana/plugins/
    git clone https://github.com/aliyun/aliyun-log-grafana-datasource-plugin
    service grafana-server restart
    ```

3.  Add a Log Service data source.

    **Note:** If you deploy Grafana on your localhost, Grafana is installed on port 3000 by default. Enable port 3000 in a browser before the configuration.

    1.  Log on to Grafana.

    2.  In the left-side navigation pane, choose **![configuration icon ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3884600061/p165730.png) icon** \> **Data Sources**.

    3.  On the Data Sources tab, click **Add data source**, **LogService**, and then click **Select**. The following table describes the parameters.

        |Parameter|Description|
        |:--------|:----------|
        |Name|The data source name.|
        |HTTP|        -   Example: `http://dashboard-demo.cn-hangzhou.log.aliyuncs.com`. In the preceding example, `dashboard-demo` indicates a project name, and `cn-hangzhou.log.aliyuncs.com` indicates the endpoint of the region where the project resides. When you configure a data source, replace the values with your project name and endpoint. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).
        -   Select Server or Browser from the Access drop-down list.
        -   Whitelisted: specifies a whitelist. |
        |Auth|Use the default setting.|
        |log service details|The settings of Log Service. Specify a project, Logstore, and AccessKey pair. You can specify the AccessKey pair of an Alibaba Cloud account or RAM user.|

    4.  Click **Save & Test**.

4.  Create a dashboard.

    1.  In the left-side navigation pane, choose **![User_guide_add_sign ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3884600061/p165734.png) icon** \> **Dashboards** to create a dashboard.

    2.  Set template variables.

        In Grafana, you can set template variables. A panel changes based on the specified variables. You can set multiple time ranges and domains.

        1.  In the upper-right corner of the New dashboard page, select a time range, and then choose **![configuration icon ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3884600061/p165730.png) icon** \> **Variables** \> **Add variable** to set a variable.
        2.  You must specify a name for a variable, for example, myinterval. When you use a variable in a search condition, the variable name must start with $, for example, `$myinterval`. After you set the following parameters, click **Add**.

            |Parameter|Description|
            |---------|-----------|
            |Name|The variable name, for example, myinterval.|
            |Type|Select **Interval**.|
            |Lable|Enter time interval.|
            |Internal Options|            -   Enter **1m,10m,30m,1h,6h,12h,1d,7d,14d,30d** in the `value` field.
            -   Turn on the **Auto Option** switch. |

        3.  Configure a domain name template

            You can attach multiple domain names to a virtual private server \(VPS\). If multiple domain names are attached to a VPS, you must view the distribution of requests that are issued to these domains.

            On the Variable page, click **New**. The following table describes the parameters.

            |Parameter|Description|
            |:--------|:----------|
            |Name|The variable name, for example, hostname.|
            |Type|Select **Custom**.|
            |Lable|Enter domain name.|
            |Custom Options|Enter **\*,www.host.com,www.host0.com,www.host1.com** in the `Values separated by` field. This setting specifies that you can show the distribution of requests that are issued to all domains. You can also show requests that are issued to the `www.host.com`, `www.host0.com`, or `www.host1.com` domain.|
            |Selection Options|Use the default setting.|

        4.  In the left-side navigation pane, click **Save** to save the template.
    3.  Add panels.

        -   PV and UV
            1.  Click the **![LogService_User_Guide_add_new_panel](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3884600061/p165738.png)** icon in the upper-right corner of the page.
            2.  On the New Panel card, click **Add Query**.
            3.  Click the **![LogService_User_Guide_visual_icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4884600061/p165739.png)** icon, and select **Graph** from the **Visualization** drop-down list to add a graph panel.
            4.  Click the **![LogService_User_Guide_title_icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4884600061/p165742.png)** icon, and enter **UV & PV** in the **Title** field.
            5.  Click the **![LogService_User_Guide_Query_Icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4884600061/p165744.png)** icon, select **LogService** from the **Query** drop-down list, and then set the following parameters.

                ![grafana_pv_uv](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4884600061/p165766.png)

                |Parameter|Description|
                |:--------|:----------|
                |Query|                ```
$hostname| select approx_distinct(remote_addr) as uv ,count(1) as pv , __time__ - __time__ % $$myinterval as time group by time order by time limit 1000
                ```

In the preceding search statement, the value of the hostname variable is replaced with a specified domain name, and the value of the myinterval variable is replaced with a time interval. Note: two $ signs precede myinterval and one $ sign precedes hostname.|
                |X-Column|time|
                |Y-Column|uv,pv|

                If a significant difference exists between the value of UV and the value of PV, you can use a dual Y-axis chart to show UV and PV. Click the line in position 1, click **Y-Axis** in position 2, and then turn on the **Use right y-axis** switch.

                ![Use right y-axis](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4884600061/p165765.png)

        -   Inbound and outbound bandwidth

            Add a panel that shows inbound and outbound bandwidth. For more information, see [PV and UV](#table_rh6_y1y_9va).

            The following table lists the parameters.

            |Parameter|Description|
            |:--------|:----------|
            |Query|            ```
$hostname | select sum(body_byte_sent) as net_out, sum(request_length) as net_in,__time__ - __time__ % $$myinterval as time group by __time__ - __time__ % $$myinterval limit 10000
            ``` |
            |X-Column|Time|
            |Y-Column|net\_in,net\_out|

        -   Percentage of HTTP methods

            Add a panel that shows the percentage of HTTP methods. For more information, see [PV and UV](#table_rh6_y1y_9va).

            Click the **![LogService_User_Guide_visual_icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4884600061/p165739.png)** icon, and select **Pie Chart** from the **Visualization** drop-down list to add a pie chart. The following table lists the parameters.

            |Parameter|Description|
            |:--------|:----------|
            |Query|            ```
$hostname | select count(1) as pv ,method group by method
            ``` |
            |X-Column|pie|
            |Y-Column|method,pv|

        -   Percentage of HTTP status codes

            Add a panel that shows the percentage of HTTP status codes. For more information, see [PV and UV](#table_rh6_y1y_9va).

            Click the **![LogService_User_Guide_visual_icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4884600061/p165739.png)** icon, and select **Pie Chart** from the **Visualization** drop-down list to add a pie chart. The following table lists the parameters.

            |Parameter|Description|
            |:--------|:----------|
            |Query|            ```
$hostname | select count(1) as pv ,status group by status
            ``` |
            |X-Column|pie|
            |Y-Column|status,pv|

        -   Hotspot page sources

            Add a panel that shows hotspot page sources. For more information, see [PV and UV](#table_rh6_y1y_9va).

            Click the **![LogService_User_Guide_visual_icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4884600061/p165739.png)** icon, and select **Pie Chart** from the **Visualization** drop-down list to add a pie chart. The following table lists the parameters.

            |Parameter|Description|
            |:--------|:----------|
            |Query|            ```
$hostname | select count(1) as pv , referer group by referer order by pv desc
            ``` |
            |X-Column|pie|
            |Y-Column|referer,pv|

        -   Top latency pages

            Add a panel that shows top latency pages. For more information, see [PV and UV](#table_rh6_y1y_9va).

            Click the **![LogService_User_Guide_visual_icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4884600061/p165739.png)** icon, and select **Table** from the **Visualization** drop-down list to add a table. This table lists the URL and latency of each page. The following table lists the parameters.

            |Parameter|Description|
            |:--------|:----------|
            |Query|            ```
$hostname | select URL as top_latency_URL ,request_time order by request_time desc limit 10
            ``` |
            |X-Column|Null.|
            |Y-Column|top\_latency\_url,request\_time|

        -   Hotspot pages

            Add a panel that shows hotspot pages. For more information, see [PV and UV](#table_rh6_y1y_9va).

            Click the **![LogService_User_Guide_visual_icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4884600061/p165739.png)** icon, and select **Table** from the **Visualization** drop-down list to add a table. The following table lists the parameters.

            |Parameter|Description|
            |:--------|:----------|
            |Query|            ```
$hostname | select count(1) as pv, split_part(URL,'?',1) as path group by split_part(URL,'?',1) order by pv desc limit 20
            ``` |
            |X-Column|Null.|
            |Y-Column|path,pv|

        -   Top non-200 request pages

            Add a panel that shows top non-200 request pages. For more information, see [PV and UV](#table_rh6_y1y_9va).

            Click the **![LogService_User_Guide_visual_icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4884600061/p165739.png)** icon, and select **Table** from the **Visualization** drop-down list to add a table. The following table lists the parameters.

            |Parameter|Description|
            |:--------|:----------|
            |Query|            ```
$hostname not status:200| select count(1) as pv , url group by url order by pv desc
            ``` |
            |X-Column|Null.|
            |Y-Column|url,pv|

        -   Average latency between the frontend and backend

            Add a panel that shows the average latency between the frontend and backend. For more information, see [PV and UV](#table_rh6_y1y_9va).

            The following table lists the parameters.

            |Parameter|Description|
            |:--------|:----------|
            |Query|            ```
$hostname | select avg(request_time) as response_time, avg(upstream_response_time) as upstream_response_time ,__time__ - __time__ % $$myinterval as time group by __time__ -  __time__ % $$myinterval limit 10000
            ``` |
            |X-Column|time|
            |Y-Column|upstream\_response\_time,response\_time|

        -   Client statistics

            Add a panel that shows client statistics. For more information, see [PV and UV](#table_rh6_y1y_9va).

            Click the **![LogService_User_Guide_visual_icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4884600061/p165739.png)** icon, and select **Pie Chart** from the **Visualization** drop-down list to add a pie chart. The following table lists the parameters.

            |Parameter|Description|
            |:--------|:----------|
            |Query|            ```
$hostname  | select count(1) as pv, case when  regexp_like(http_user_agent , 'okhttp') then 'okhttp' when  regexp_like(http_user_agent ,  'iPhone') then 'iPhone' when regexp_like(http_user_agent ,  'Android')  then 'Android' else 'unKnown' end as http_user_agent group by  http_user_agent order by pv desc limit 10
            ``` |
            |X-Column|pie|
            |Y-Column|http\_user\_agent,pv|

        -   Logs panel

            Add a logs panel. For more information, see [PV and UV](#table_rh6_y1y_9va).

            Click the **![LogService_User_Guide_visual_icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4884600061/p165739.png)** icon, and select **Logs** from the **Visualization** drop-down list to add a logs panel. Set the **Logs Per Page** field.

            **Note:** Each page can show a maximum of 100 logs. Enter 100 in the **Logs Per Page** field.

        -   Graph panel

            Add a graph panel. For more information, see [PV and UV](#table_rh6_y1y_9va).

            The following table lists the parameters.

            |Parameter|Description|
            |:--------|:----------|
            |Query|            ```
$hostname  | select to_unixtime(time) as time,status,count from (select time_series(__time__, '1m', '%Y-%m-%d %H:%i', '0')  as time,status,count(*) as count from log group by status,time order by time limit 10000)
            ``` |
            |X-Column|time|
            |Y-Column|count,status|

        -   Worldmap panel

            Add a worldmap panel. For more information, see [PV and UV](#table_rh6_y1y_9va).

            Click the **![LogService_User_Guide_visual_icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4884600061/p165739.png)** icon, and select **Worldmap Panel** from the **Visualization** drop-down list to add a worldmap panel. The following table lists the parameters on the Queries tab.

            |Parameter|Description|
            |:--------|:----------|
            |Query|            ```
$hostname  | select   count(1) as pv ,geohash(ip_to_geo(arbitrary(remote_addr))) as geo,ip_to_country(remote_addr) as country  from log group by country having geo <>'' limit 1000
            ``` |
            |X-Column|map|
            |Y-Column|country,geo,pv|

            The following table lists the parameters on the Visualization tab.

            |Parameter|Description|
            |:--------|:----------|
            |Location Data|geohash|
            |Location Name Field|country|
            |geo\_point/geohash Field|geo|
            |Metric Field|pv|

    4.  In the upper-right corner of the page, click the **![LogService_User_Guide_Save_icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5884600061/p165781.png)** icon to save the dashboard.

5.  View the results.

    Go to the homepage of the dashboard to view the results. For more information, see [Demo](http://47.96.36.117:3000/dashboard/db/nginxfang-wen-tong-ji?orgId=1).

    On the top of the dashboard page, you can select a time range, time granularity, or domain name. The configuration of the NGINX access statistics dashboard is complete. This dashboard allows you to obtain the required information.


