# Display query results on a line chart

This topic describes how to configure a line chart to display query results.

## Background information

A line chart is used to analyze the value changes of fields based on an ordered data type. In most cases, this analysis is based on a specified time range. You can use a line chart to analyze the following change characteristics of field values over a specified period:

-   Increment or decrement
-   Increment or decrement rate
-   Increment or decrement pattern, for example, periodicity
-   Peak value and valley value

Line charts are used to analyze field value changes over a time period. You can also use a line chart to analyze the value changes of multiple fields in multiple lines over the same time period. This allows you to analyze the relationship between the different fields. For example, the values of multiple fields can display positive, negative, or inverse trends.

Each line chart consists of the following elements:

-   X-axis
-   Left Y-axis
-   Right Y-axis \(optional\)
-   Data point
-   Line of trend change
-   Legend

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project that you want to manage.

3.  Choose **Log Storage** \> **Logstores**, and then click the Logstore that you want to manage.

4.  Enter a query statement in the search box, specify a time range, and then click **Search & Analyze**.

5.  On the Graph tab, click the ![Line chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7766895951/p93114.png) icon.

6.  On the **Properties** tab, configure the properties of the line chart.

    **Note:** In a line chart, each line must contain more than two data points. Otherwise, the data trend cannot be analyzed. We recommend that you select less than five lines in a line chart.

    |Parameter|Description|
    |:--------|:----------|
    |X Axis|The sequential data. In most cases, time series is selected.|
    |Left Y Axis|The numeric data. You can select one or more fields for the left Y-axis.|
    |Right Y Axis|The numeric data. You can select one or more fields for the right Y-axis. The layer of the right Y-axis is higher than that of the left Y-axis.|
    |Column Marker|The column on the left or right Y-axis that is selected as a histogram.|
    |Legend|The position where the legend is located in the chart. Valid values: Top, Bottom, Left, and Right.|
    |Format Left Y-axis|The format in which data selected for the left Y-axis and right Y-axis is displayed.|
    |Format Right Y-axis|
    |Left Y-axis Start Value|The custom minimum value of the left Y-axis and right Y-axis.|
    |Right Y-axis Start Value|
    |Left Y-axis Maximum Value|The custom maximum value of the left Y-axis and right Y-axis.|
    |Right Y-axis Maximum Value|
    |Line Type|The type of line that is displayed in the line chart. Valid values: **Straight Line** and **Curve**.|
    |Show Markers|If you turn on the Show Markers switch, the data points are displayed.|
    |Metric Filter|If you turn on the Metric Filter switch, you can filter metrics to view specific types of data.|
    |Legend Width|The width of the legend.|
    |X-axis Scale Density|The scale density of the X-axis. Valid values: 3 to 30.|
    |Scatter Column|The Y-axis column where scatters are located.|
    |Scatter Numeric Column|The numeric value on the scatter column.|
    |Scatter Size|The value range of the scatters.|
    |Anomaly Point Column|The column where anomaly points are located. You can set the **Anomaly Point Lower Limit** and **Anomaly Point Upper Limit** parameters for a column.     -   **Anomaly Point Lower Limit**: Field values that are lower than the lower limit are highlighted in red.
    -   **Anomaly Point Upper Limit**: Field values that are higher than the upper limit are highlighted in red. |
    |Upper Limit Column|The area that is formed based on the values of fields.|
    |Lower Limit Column|
    |Time Series|A series of data points that are listed in chronological order.|
    |Time Format|The time format of the time series fields.|
    |Margin|The distance between the axis and the borders of the chart. This includes the Top Margin, Bottom Margin, Left Margin, and Right Margin.|


## Example of a simple line chart

To query the page views \(PVs\) of the IP address `10.0.0.0` in the last 24 hours, execute the following query statement:

```
remote_addr: 10.0.0.0 | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i') 
as time, count(1) as PV group by time order by time limit 1000
```

Select `time` for the X-axis, `PV` for the left Y-axis, and Bottom for Legend. Adjust the margins based on your business requirements.

![Simple line chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0823359951/p5709.png)

## Example of a dual Y-axis line chart

To query the PVs and unique visitors \(UVs\) in the last 24 hours, execute the following query statement:

```
* | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i') as time, count(1) as PV, approx_distinct(remote_addr) as UV group by time order by time limit 1000
```

Select `time` for the X-axis, `PV` for the left Y-axis, `UV` for the right Y-axis, and `UV` for Column Marker.

![Dual Y-axis line chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0823359951/p5710.png)

