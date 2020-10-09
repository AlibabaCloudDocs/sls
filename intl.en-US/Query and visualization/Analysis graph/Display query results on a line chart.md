# Display query results on a line chart

This topic describes how to configure a line chart to display query results.

## Background information

A line chart is used to analyze the value changes of fields based on an ordered data type, which is a time range in most cases. You can use a line chart to analyze the following change characteristics of field values over a period:

-   Increment or decrement
-   Increment or decrement rate
-   Increment or decrement pattern, for example, periodicity
-   Peak value and trough value

Line charts are applicable to analyzing field value changes over time. You can also use a line chart to analyze the value changes of multiple fields in multiple lines over the same period and reveal the relationship between the fields. For example, the values of multiple fields are positively or negatively associated with each other.

Each line chart consists of the following elements:

-   X-axis
-   Left Y-axis
-   Optional. Right Y-axis
-   Data point
-   Line of trend change
-   Legend

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Click the target project in the Projects section.

3.  Choose **Log Management** \> **Logstores**, click the management icon of the target Logstore, and then select **![Management icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9484688951/p52166.png)** \> **Search & Analysis**.

4.  Enter a query statement in the search box, select a time range, and then click **Search & Analyze**.

5.  Click ![Line chart](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7766895951/p93114.png) to display the query results in a line chart.

6.  On the **Properties** tab, configure the properties of the line chart.

    **Note:** In a line chart, each line must contain more than two data points. Otherwise, the data trend cannot be analyzed. We recommend that you configure no more than five lines in a line chart.

    |Parameter|Description|
    |:--------|:----------|
    |X Axis|The data on the X-axis, which is usually a time sequence.|
    |Left Y Axis|The numeric data on the left Y-axis. You can configure one or more fields for the left Y-axis.|
    |Right Y Axis|The numeric data on the right Y-axis. You can configure one or more fields for the right Y-axis. The layer of the right Y-axis is higher than that of the left Y-axis.|
    |Column Marker|The column on the left or right Y-axis that is selected as a histogram.|
    |Legend|The position where the legend is located in the chart. Valid values: Top, Bottom, Left, and Right.|
    |Format Left Y-axis|The format in which data configured on the left Y-axis and right Y-axis is displayed.|
    |Format Right Y-axis|
    |Left Y-axis Start Value|The custom start value of the left Y-axis and right Y-axis.|
    |Right Y-axis Start Value|
    |Left Y-axis Maximum Value|The custom maximum value of the left Y-axis and right Y-axis.|
    |Right Y-axis Maximum Value|
    |Line Type|The type of a line displayed in the line chart. Valid values: **Straight Line** and **Curve**.|
    |Show Markers|If you turn on this switch, the data points are displayed.|
    |Metric Filter|If you turn on this switch, you can filter metrics to view specific types of data.|
    |Legend Width|Sets the width of the legend.|
    |X-axis Scale Density|Specifies the scale density of the X-axis. Value range: 3 to 30.|
    |Scatter Column|Selects the Y-axis column where the scatter is located.|
    |Scatter Numeric Column|The numeric value on the scatter column.|
    |Scatter Size|Sets the value range of the scatter.|
    |Anomaly Point Column|The column where anomaly points are located. You can set the **Anomaly Point Lower Limit** and **Anomaly Point Upper Limit** for a column.     -   **Anomaly Point Lower Limit**: Field values that are lower than the lower limit are highlighted in red.
    -   **Anomaly Point Upper Limit**: Field values that are higher than the upper limit are highlighted in red. |
    |Upper Limit Column|The area that is formed based on the values of fields.|
    |Lower Limit Column|
    |Time Series|The time series is based on fields whose values are sorted in chronological order.|
    |Time Format|The time format of the time series fields.|
    |Margin|The distance between the axis and the borders of the chart, including **Top Margin**, **Bottom Margin**, **Right Margin**, and **Left Margin**.|


## Example of a simple line chart

To query the page views \(PVs\) of the IP address `10.0.0.0` in the last 24 hours, execute the following statement:

```
remote_addr: 10.0.0.0 | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i') 
as time, count(1) as PV group by time order by time limit 1000
```

Select `time` for the X-axis, `PV` for the left Y-axis, and Bottom for legend. Adjust the margins based on your needs.

![Simple line chart](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0823359951/p5709.png)

## Example of a dual Y-axis line chart

To query the PVs and unique visitors \(UVs\) in the last 24 hours, execute the following statement:

```
* | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i') as time, count(1) as PV, approx_distinct(remote_addr) as UV group by time order by time limit 1000
```

Select `time` for the X-axis, `PV` for the left Y-axis, `UV` for the right Y-axis, and `PV` for column marker.

![Dual Y-axis line chart](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0823359951/p5710.png)

