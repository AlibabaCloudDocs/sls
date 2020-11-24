# Display query results on a flow chart

This topic describes how to configure a flow chart to display query results.

## Background information

The flow chart, also known as ThemeRiver, is a stacked area chart around a central axis. The banded branches with different colors indicate different categorical data. The band width indicates the numeric value. The time information of the data is mapped to the X-axis by default. A flow chart can display the data of three parameters.

You can select the line chart or column chart for the Chart Types parameter. The column chart is stacked by default. Each category of data starts from the top of the last column of categorical data.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project that you want to manage.

3.  Choose **Log Storage** \> **Logstores**, and then click the Logstore that you want to manage.

4.  Enter a query statement in the search box, specify a time range, and then click **Search & Analyze**.

5.  On the Graph tab, click the ![Flow chart - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1167895951/p93124.png) icon.

6.  On the **Properties** tab, configure the properties of the flow chart.

    -   Area chart

        |Parameter|Description|
        |:--------|:----------|
        |Chart Types|The type of the chart. In this example, query results are displayed on an area chart.|
        |X Axis|The sequential data. In most cases, time series is selected.|
        |Y Axis|The numeric data. You can select one or more fields for the Y-axis.|
        |Aggregate Column|The field information required to be aggregated as the third point for comparison.|
        |Chart Details Column|The fields that are prompted when you move the pointer over a field value on the Y-axis.|
        |Legend|The position where the legend is located in the chart. Valid values: Top, Bottom, Left, and Right.|
        |Format|The format in which data is displayed.|
        |Legend Width|The width of the legend.|
        |X-axis Scale Density|The scale density of the X-axis.|
        |Metric Filter|If you turn on the Metric Filter switch, you can filter metrics to view specific types of data.|
        |Time Series|Set the X-axis to time series fields.|
        |Time Format|The time format of the time series fields.|
        |Margin|The distance between the axis and the borders of the chart. This includes the Top Margin, Bottom Margin, Left Margin, and Right Margin.|

    -   Line chart

        |Parameter|Description|
        |:--------|:----------|
        |Chart Types|The type of the chart. In this example, query results are displayed on a line chart.|
        |X Axis|The sequential data. In most cases, time series is selected.|
        |Y Axis|The numeric data. You can select one or more fields for the Y-axis.|
        |Aggregate Column|The field information required to be aggregated as the third point for comparison.|
        |Chart Details Column|The fields that are prompted when you move the pointer over a field value on the Y-axis.|
        |Legend|The position where the legend is located in the chart. Valid values: Top, Bottom, Left, and Right.|
        |Format|The format in which data is displayed.|
        |Show Markers|If you turn on the Show Markers switch, the data points are displayed.|
        |Legend Width|The width of the legend.|
        |X-axis Scale Density|The scale density of the X-axis.|
        |Metric Filter|If you turn on the Metric Filter switch, you can filter metrics to view specific types of data.|
        |Add Line|If you turn on the Add Line switch, you can add a horizontal line to the flow chart.You can click **Add Line** to set the color of the line and the value on the Y-axis where the line is located. |
        |Time Series|Set the X-axis to time series fields.|
        |Time Format|The time format of the time series fields.|
        |Margin|The distance between the axis and the borders of the chart. This includes the Top Margin, Bottom Margin, Left Margin, and Right Margin.|

    -   Column chart

        |Parameter|Description|
        |:--------|:----------|
        |Chart Types|The type of the chart. In this example, query results are displayed on a column chart.|
        |Grouping|If you turn on the Grouping switch, grouped columns are displayed. If you turn off the Grouping switch, the Y-axis data is stacked.|
        |X Axis|The sequential data. In most cases, time series is selected.|
        |Y Axis|The numeric data. You can select one or more fields for the Y-axis.|
        |Aggregate Column|The field information required to be aggregated as the third point for comparison.|
        |Chart Details Column|The fields that are prompted when you move the pointer over a field value on the Y-axis.|
        |Legend|The position where the legend is located in the chart. Valid values: Top, Bottom, Left, and Right.|
        |Format|The format in which data is displayed.|
        |Legend Width|The width of the legend.|
        |X-axis Scale Density|The scale density of the X-axis.|
        |Metric Filter|If you turn on the Metric Filter switch, you can filter metrics to view specific types of data.|
        |Time Series|Set the X-axis to time series fields.|
        |Time Format|The time format of the time series fields.|
        |Margin|The distance between the axis and the borders of the chart. This includes the Top Margin, Bottom Margin, Left Margin, and Right Margin.|

    -   Scatter with smooth lines and markers

        |Parameter|Description|
        |:--------|:----------|
        |Chart Types|The type of the chart. In this example, query results are displayed in a table in which the data scatters with smooth lines and markers.|
        |Row|The selected field and its values are the first column of the chart.|
        |Value Column|The values of a selected field in the aggregate column.|
        |Aggregate Column|The field information required to be aggregated as the third point for comparison.|
        |Chart Details Column|The fields that are prompted when you move the pointer over a field value on the Y-axis.|
        |Format|The format in which data is displayed.|
        |Items per Page|The number of entries to be displayed on each page.|
        |Disable Sorting|Specifies whether to disable the sorting feature.|
        |Disable Search|Specifies whether to disable the search feature.|
        |Metric Filter|If you turn on the Metric Filter switch, you can filter metrics to view specific types of data.|
        |Time Series|Set the X-axis to time series fields.|
        |Time Format|The time format of the time series fields.|


## Examples

The flow chart is suitable to display data of three parameters, including the time, categories, and numeric values. In this example, you can execute the following query statement:

```
* | select date_format(from_unixtime(__time__ - __time__% 60), '%H:%i:%S') as minute, count(1) as c,  request_method group by minute, request_method order by minute asc limit 100000
```

Select **minute** for the X-axis, **c** for the Y-axis, and **request\_method** for Aggregate Column.

![Flow chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7823359951/p5737.png)

