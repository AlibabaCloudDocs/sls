# Connect Log Service with DataV

DataV is an Alibaba Cloud data visualization service. DataV allows you to build professional visualization applications by using a graphical user interface \(GUI\) with ease. You can use DataV to visualize log analysis data. This topic describes how to connect Log Service with DataV and show data on a dashboard.

-   Log data is collected. For more information, see [Data collection](/intl.en-US/Data Collection/Log collection methods.md).
-   The index feature is enabled and configured. For more information, see [Enable and configure the index feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

Rea-time dashboards are widely used in large online promotions. Real-time data dashboards are based on a stream computing architecture. The architecture consists of the following modules:

-   Data collection: collects data from each source in real time.
-   Intermediate storage: uses Kafka Queues to decouple production systems and consumption systems.
-   Real-time computing: subscribes to real-time data and uses computing rules to compute data on the dashboard.
-   Result storage: stores the computing results in SQL and NoSQL databases.
-   Visualization: calls API operations to obtain results and visualize the results.

Alibaba Group provides multiple services to support these modules, as shown in the following figure.

![Data dashboard-related services](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6992767751/p5861.png)

You can connect Log Service with DataV by calling the API operations that are related to the log search and analytics feature. Then, you can use DataV to show data on a dashboard.

![Connect Log Service with DataV](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6992767751/p5862.png)

## Features

The following computing methods are supported:

-   Real-time computing \(streaming computing\): fixed computing and dynamic data.
-   Offline computing \(data warehouse and offline computing\): dynamic computing and fixed data.

For scenarios that have a high-level requirement of timeliness, Log Service allows you to enable the real-time data indexing feature on logs that are stored in LogHub. You can use the log search and analytics feature to query and analyze these logs in an efficient manner. This method has the following benefits:

-   Fast: Billions of data records can be processed and queried within one second. Each query statement can have a maximum of five specified conditions. Hundreds of millions of data records can be analyzed and aggregated within one second without the need to wait and predict the results. Each query statement can have a maximum of five aggregate functions and a group by clause.
-   Real-time display: Up to 99.9% of logs can be displayed on the data dashboard within one second after these logs are generated.
-   Dynamic data refresh: When you modify analysis methods or import data to Logstores, the display result is refreshed in real time.

This method has the following limits:

-   Data volume: Up to 10 billion lines of data can be computed at a time. If you need to compute more data, you must set multiple time ranges.
-   Flexibility: Only SQL-92 syntax is supported. User-defined functions \(UDFs\) are not supported.

## Procedure

1.  Create a DataV data source.

    1.  Log on to the [DataV console](https://datav.aliyun.com/).

    2.  On the Data Sources tab, click **Add Source**. The following table describes the parameters.

        |Parameter|Description|
        |:--------|:----------|
        |The type of the database.|In the Add Data Source dialog box, select **Log Service** from the Type drop-down list.|
        |Name|Enter a name for the data source, for example, log\_service\_api.|
        |AppKey|The AccessKey ID of the Alibaba Cloud account, or the AccessKey ID of a RAM user that has the read permission on Log Service.|
        |AppSecret|The AccessKey secret of the Alibaba Cloud account, or the AccessKey secret of a RAM user that has the read permission on Log Service.|
        |Endpoint|The endpoint of the region where the Log Service project resides. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|

2.  Open a canvas.

    -   On the Projects tab, select a project, and click **Edit**.
    -   On the Projects tab, click **Create Project**. For more information about how to create a project, see [Create a project by using a template](/intl.en-US/Manage Projects/Create a project.md).
3.  Create a line chart and add a filter.

    1.  Create a line chart.

        In the Widgets pane, choose **Line** \> **Line Chart**. In the right-side Line Chart pane, click the Data tab, and click **Set**. In the **Configure Datasource** pane, set the required parameters. The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |Data Source Type|Select **Log Service** from the drop-down list.|
        |Select Data Source|Select the [log\_service\_api](#step_yam_6sd_nm5) data source that you created in Step 1.|
        |Query|The following code shows a sample query.        ```
{
 "projectName": "dashboard-demo",
 "logStoreName": "access-log",
 "topic": "",
 "from": ":from",
 "to": ":to",
 "query": "*| select approx_distinct(remote_addr) as uv ,count(1) as pv , date_format(from_unixtime(date_trunc('hour',__time__) ) ,'%Y/%m/%d %H:%i:%s')   as time group by time  order by time limit 1000" ,
 "line": 100,
 "offset": 0
}
        ``` |

        The following table describes the parameters in the preceding query.

        |Parameter|Description|
        |:--------|:----------|
        |projectName|The name of the project.|
        |logstoreName|The name of the Logstore.|
        |topic|The log topic. Do not set this parameter if you have not set a log topic.|
        |from, to|The from and to parameters refer to the start time and end time during which logs are obtained.**Note:** In this example, the parameters are set to `:from` and `:to`. When you test your parameter settings, you can enter a UNIX timestamp, for example, 1509897600. After the test, change the UNIX timestamp to `:from` and `:to`. Then, you can specify a time range in the URL parameters. For example, the URL in the preview is `http://datav.aliyun.com/screen/86312`. After the URL `http://datav.aliyun.com/screen/86312?from=1510796077&to=1510798877` is opened, the values are computed based on the specified time. |
        |query|The query criteria. For more information about the query syntax, see [Real-time log analysis](/intl.en-US/Index and query/Real-time log analysis.md).**Note:** The time in the query must be in the yyyy/MM/dd HH:mm:ss format, for example, 2017/07/11 12:00:00. You must use the following syntax to align the time to the hour. Then, you can convert the time into the specified format.

        ```
date_format(from_unixtime(date_trunc('hour',__time__) ) ,'%Y/%m/%d%H:%i:%s')
        ``` |
        |line|Enter the default value 100.|
        |offset|Enter the default value 0.|

    2.  After the configuration is complete, click **Data Response Result**.

    3.  Create a filter.

        In the Configure Datasource dialog box, select **Data Filter**, and click the plus \(+\) icon next to the **Add Filter** drop-down list.

        Enter a function in the New Filter field by using the following example syntax:

        ```
        return Object.keys(data).map((key) => {
        let d= data[key];
        d["pv"] = parseInt(d["pv"]);
        return d;
        });
        ```

        -   In the filter, convert the result that is used by the y-axis into the INT type. In this example, the y-axis indicates the PV, and the pv column must be converted.
        -   The results contain the t and pv columns. You can set the x-axis to t and the y-axis to pv.
4.  Create a pie chart and add a filter.

    1.  Create a **carousel pie chart**.

        In the Widgets pane, choose **Pie** \> **Carousel Pie Chart**. In the right-side Carousel Pie Chart pane, click the Data tab, and click **Set**. In the **Configure Datasource** pane, set the required parameters. The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |Data Source Type|Select **Log Service** from the drop-down list.|
        |Select Data Source|Select the [log\_service\_api](#step_yam_6sd_nm5) data source that you created in Step 1.|
        |Query|The following code shows a sample query.        ```
{
 "projectName": "dashboard-demo",
 "logStoreName": "access-log",
 "topic": "",
 "from": 1509897600,
 "to": 1509984000,
 "query": "*| select count(1) as pv ,method group by method" ,
 "line": 100,
 "offset": 0
}
        ```

The parameters in the preceding code are described in Step 3.|

    2.  After the configuration is complete, click **Data Response Result**.

    3.  Add a filter.

        In the Configure Datasource pane, click **Data Filter**, and then click the plus sign \(+\) next to the **Add Filter** field.

        Enter a function in the New Filter field by using the following example syntax:

        ```
        return Object.keys(data).map((key) => {
        let d= data[key];
        d["pv"] = parseInt(d["pv"]);
        return d;
        })
        ```

        Enter method in the type field and pv in the value field for the carousel pie chart.

5.  Use callback functions to retrieve a time range in real time. Perform the following steps to show the real-time logs of the last 15 minutes.

    1.  Create a static data source with the default settings, and add a filter. Example:

        ```
        return [{
          value:Math.round(Date.now()/1000)
        }];
        ```

        ```
        return [{
          value:Math.round((Date.now() - 24 * 60 * 60 * 1000)/1000)
        }];
        ```

    2.  On the Interaction tab, select **Enable** in the Trigger Event when Number Changed field, and add the required values to the Bound Variable column.

    3.  On the Data tab, the following example shows how to use the `:from` and `:to` parameters to implement callback functions.

        ```
        {
         "projectName": "dashboard-demo",
         "logStoreName": "access-log",
         "topic": "",
         "from": ":from",
         "to": ":to",
         "query": "*| select count(1) as pv ,referer group by pv desc limit 30" ,
         "line": 100,
         "offset": 0
        }
        ```

6.  Preview and publish the DataV dashboard.

    1.  Click the **Preview** icon to preview the DataV dashboard.

    2.  Click the **Publish** icon to publish the DataV dashboard.


## Example

You need to collect the page view \(PV\) statistics of your website across China during the Computing Conference and show the data on a dashboard. You have configured full log data collection and enabled the log search and analytics feature in Log Service. You need to only enter a query statement in the Query field to obtain the PV statistics. During this period, the requirement often changes. In this example, the following changes are made:

-   Original requirement: On the first day of the conference, you need the statistics of unique visitors \(UVs\) for the present day.

    You need to query the values of the forward field under NGINX in all the access logs. The forward field records one or more IP addresses of each visitor. Each log has a forward field. You can use the following statement that includes the `approx_distinct(forward)` function to remove repeated IP addresses and obtain the UV statistics in a time range from 00:00 for the first day of the conference to the present time.

    ```
    * | select approx_distinct(forward) as uv
    ```

-   First change: On the second day of the conference, you need the statistics of PVs of the yunqi.aliyun.com domain.

    You can add the following filter condition that starts with host to the statement:

    ```
    host:yunqi.aliyun.com | select approx_distinct(forward) as uv
    ```

-   Second change: If the NGINX access logs contain multiple IP addresses, you can enter the following statement to reserve only the first IP address:

    ```
    host:yunqi.aliyun.com | select approx_distinct(split_part(forward,',',1)) as uv
    ```

-   Third change: On the third day of the conference, you need to remove statistics that are generated by UC browser advertisement from the access statistics.

    You can add the following filter condition that starts with not to the statement:

    ```
    host:yunqi.aliyun.com not URL:uc-iflow  | select approx_distinct(split_part(forward,',',1)) as uv
    ```


