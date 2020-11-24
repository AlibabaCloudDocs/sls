# Display query results on an area chart

This topic describes how to configure an area chart to display query results.

## Background information

An area chart is built based on a line chart. The colored section between a line and the axis is an area. The color is used to highlight the trend. Similar to a line chart, an area chart shows the numeric value changes over a specified time period. It is used to highlight the overall data trend. Both the line chart and the area chart display the trend and relationship between numeric values instead of specific values.

Each area chart consists of the following elements:

-   X-axis \(horizontal\)
-   Y-axis \(vertical\)
-   Area segment

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project that you want to manage.

3.  Choose **Log Storage** \> **Logstores**, and then click the Logstore that you want to manage.

4.  Enter a query statement in the search box, specify a time range, and then click **Search & Analyze**.

5.  On the Graph tab, click the ![Area chart - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6696895951/p93118.png) icon.

6.  On the **Properties** tab, configure the properties of the area chart.

    **Note:** In an area chart, a single area segment must contain more than two data points. Otherwise, the data trend cannot be analyzed. We recommend that you select less than five area segments in an area chart.

    |Parameter|Description|
    |:--------|:----------|
    |X Axis|The sequential data. In most cases, time series is selected.|
    |Y Axis|The numeric data. You can select one or more fields for the Y-axis.|
    |Legend|The position where the legend is located in the chart. Valid values: Top, Bottom, Left, and Right.|
    |Format|The format in which data is displayed.|
    |Y-axis Minimum Value|The custom minimum value of the Y-axis.|
    |Y-axis Maximum Value|The custom maximum value of the Y-axis.|
    |Legend Width|The width of the legend.|
    |X-axis Scale Density|The scale density of the X-axis.|
    |Metric Filter|If you turn on the Metric Filter switch, you can filter metrics to view specific types of data.|
    |Stacking|If you turn on the Stacking switch, the Y-axis data is stacked.|
    |Margin|The distance between the axis and the borders of the chart. This includes the Top Margin, Bottom Margin, Left Margin, and Right Margin.|


## Example of a simple area chart

To query the page views \(PVs\) of the IP address `10.0.0.0` in the last 24 hours, execute the following query statement:

```
remote_addr: 10.0.0.0 | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i') as time, count(1) as PV group by time order by time limit 1000
```

Select `time` for the X-axis and `PV` for the Y-axis.

![1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6374122061/p129973.png)

## Example of a cascade chart

```
* | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i') as time, count(1) as PV, approx_distinct(remote_addr) as UV group by time order by time limit 1000
```

Select `time` for the X-axis. Select `PV` and `UV` for the Y-axis.

![2](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6374122061/p129974.png)

