# Display query results on a bar chart

This topic describes how to configure a horizontal bar chart to display query results.

## Background information

A bar chart is a horizontal column chart that is used to analyze the top N values of fields. A bar chart is configured in a similar way to a column chart.

Each bar chart consists of the following elements:

-   X-axis \(vertical\)
-   Y-axis \(horizontal\)
-   Rectangular bar
-   Legend

Each rectangular bar has a fixed height and a variable width. The variable width indicates a value. You can use a grouped bar chart to display the data if multiple columns of data are configured for the Y-axis.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project that you want to manage.

3.  Choose **Log Storage** \> **Logstores**, and then click the Logstore that you want to manage.

4.  Enter a query statement in the search box, specify a time range, and then click **Search & Analyze**.

5.  On the Graph tab, click the ![Bar chart - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1976895951/p93116.png) icon.

6.  On the **Properties** tab, configure the properties of the bar chart.

    **Note:**

    -   Bar charts can be used to display query results if less than 20 log entries are returned. You can use a LIMIT clause to control the number of rectangular bars. Analysis results may not be clearly displayed if the chart contains a large number of rectangular bars. You can use an `ORDER BY` clause to analyze the top N values of fields. We recommend that you map less than five columns of data to the Y-axis.
    -   You can use a grouped bar chart to display query results. However, the values represented by each rectangular bar in a group must be positively or negatively associated with each other.
    |Parameter|Description|
    |:--------|:----------|
    |X Axis|The categorical data.|
    |Y Axis|The numeric data. You can select one or more fields for the Y-axis.|
    |Legend|The position where the legend is located in the chart. Valid values: Top, Bottom, Left, and Right.|
    |Format X-axis|The format in which the data selected for the X-axis is displayed.|
    |Legend Width|The width of the Legend.|
    |Y-axis Scale Density|The scale density of the Y-axis. Valid values: 3 to 30.|
    |Show Values|If you turn on the Show Values switch, the value of each column is displayed.|
    |Margin|The distance between the axis and the borders of the chart. This includes the Top Margin, Bottom Margin, Left Margin, and Right Margin.|


## Examples

To analyze the top 10 visited request URIs \(`request_uri`\), execute the following query statement:

```
* | select  request_uri, count(1) as count group by request_uri order by count desc limit 10
```

![Examples](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1823359951/p5717.png)

