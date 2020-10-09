# Display query results on a flow chart

This topic describes how to configure a flow chart to display query results.

## Background information

The flow chart, also known as ThemeRiver, is a stacked area chart around a central axis. The banded branches with different colors indicate different categorical data. The band width indicates the numeric value. The time information of the data is configured on the X-axis of the chart by default. A flow chart displays data of three parameters.

You can convert a flow chart to a line chart or column chart. The column chart is stacked by default. Each category of data starts from the top of the last column of categorical data.

Each flow chart consists of the following elements:

-   X-axis \(horizontal\)
-   Y-axis \(vertical\)
-   Band

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Click the target project in the Projects section.

3.  Choose **Log Management** \> **Logstores**, click the management icon of the target Logstore, and then select **![Management icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9484688951/p52166.png)** \> **Search & Analysis**.

4.  Enter a query statement in the search box, select a time range, and then click **Search & Analyze**.

5.  Click ![Flow chart - 001](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1167895951/p93124.png) to display the query results in a flow chart.

6.  On the **Properties** tab, configure the properties of the flow chart.

    |Parameter|Description|
    |:--------|:----------|
    |Chart Types|The type of the chart. Valid values: Line Chart, Area Chart, and Column Chart. Default value: Line Chart.|
    |X Axis|The data on the X-axis, which is usually a time sequence.|
    |Y Axis|The numeric data. You can configure one or more fields on the Y-axis.|
    |Aggregate Column|The information required to be aggregated in the third dimension.|
    |Chart Details Column|Move the pointer over the chart to display detailed information.|
    |Legend|The position where the legend is located in the chart. Valid values: Top, Bottom, Left, and Right.|
    |Format|The format in which data is displayed.|
    |Show Markers|If you turn on this switch, the data points are displayed.|
    |Legend Width|Sets the width of the legend.|
    |X-axis Scale Density|Sets the scale density of the X-axis.|
    |Metric Filter|If you turn on this switch, you can filter metrics to view specific types of data.|
    |Add Line|If you turn on this switch, you can add a horizontal line to the flow chart.Click **Add Line** to set the color of the line and the value on the Y-axis where the line is located. |
    |Time Series|Set the X-axis to time series fields.|
    |Time Format|The time format of the time series fields.|
    |Margin|The distance between the axis and the borders of the chart, including **Top Margin**, **Bottom Margin**, **Right Margin**, and **Left Margin**.|


## Example

The flow chart is suitable to display data of three parameters, including the time, categories, and numeric values.

```
* | select date_format(from_unixtime(__time__ - __time__% 60), '%H:%i:%S') as minute, count(1) as c,  request_method group by minute, request_method order by minute asc limit 100000
```

Select `minute` for X-axis, `c` for Y-axis, and `request_method` for Aggregate Column.

![flow chart](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7823359951/p5737.png)

