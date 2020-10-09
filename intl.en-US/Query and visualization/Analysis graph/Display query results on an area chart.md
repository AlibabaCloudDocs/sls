# Display query results on an area chart

This topic describes how to configure an area chart to display query results.

## Background information

An area chart is constructed based on a line chart. The colored section between a line and the axis is an area. The color highlights the trend. Similar to a line chart, an area chart emphasizes numeric value changes over time and is used to highlight the overall data trend. Both the line chart and the area chart display the trend and relationship of numeric values instead of specific values.

Each area chart consists of the following elements:

-   X-axis \(horizontal\)
-   Y-axis \(vertical\)
-   Area segment

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Click the target project in the Projects section.

3.  Choose **Log Management** \> **Logstores**, click the management icon of the target Logstore, and then select **![Management icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9484688951/p52166.png)** \> **Search & Analysis**.

4.  Enter a query statement in the search box, select a time range, and then click **Search & Analyze**.

5.  Click ![Area chart - 001](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6696895951/p93118.png) to display the query results in an area chart.

6.  On the **Properties** tab, configure the properties of the area chart.

    **Note:** In an area chart, a single area segment must contain more than two data points. Otherwise, the data trend cannot be analyzed. We recommend that you configure no more than five area segments in an area chart.

    |Parameter|Description|
    |:--------|:----------|
    |X Axis|The data on the X-axis, which is usually a time sequence.|
    |Y Axis|The numeric data. You can configure one or more fields for the Y-axis.|
    |Legend|The position where the legend is located in the chart. Valid values: Top, Bottom, Left, and Right.|
    |Format|The format in which data is displayed.|
    |Y-axis Minimum Value|Sets the minimum value on the Y-axis.|
    |Y-axis Maximum Value|Sets the maximum value of the Y-axis.|
    |Legend Width|Sets the width of the legend.|
    |X-axis Scale Density|Sets the scale density of the X-axis.|
    |Metric Filter|If you turn on this switch, you can filter metrics to view specific types of data.|
    |Margin|The distance between the axis and the borders of the chart, including **Top Margin**, **Bottom Margin**, **Right Margin**, and **Left Margin**.|
    |Stacking|If you turn on this switch, the Y-axis data is stacked.|


## Example of an area chart

To query the page views \(PVs\) of the IP address `10.0.0.0` in the last 24 hours, execute the following statement:

```
remote_addr: 10.0.0.0 | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i') as time, count(1) as PV group by time order by time limit 1000
```

Select `time` for X-axis and `PV` for Y-axis.

![1](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6374122061/p129973.png)

## Example of a cascade chart

```
* | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i') as time, count(1) as PV, approx_distinct(remote_addr) as UV group by time order by time limit 1000
```

Select `time` for X-axis. Select `PV` and `UV` for Y-axis.

![2](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6374122061/p129974.png)

