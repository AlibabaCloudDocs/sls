# Display query results on a bar chart

This topic describes how to configure a horizontal bar chart to display query results.

## Background information

A bar chart is a horizontal column chart that is used to analyze the top N values of fields. It is similar in configuration to a column chart.

Each bar chart consists of the following elements:

-   X-axis \(vertical\)
-   Y-axis \(horizontal\)
-   Rectangular bar
-   Legend

Each rectangular bar has a fixed height and a variable width that indicates a value. You can use a grouped bar chart to display the data if multiple columns of data are configured for the Y-axis.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Click the target project in the Projects section.

3.  Choose **Log Management** \> **Logstores**, click the management icon of the target Logstore, and then select **![Management icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9484688951/p52166.png)** \> **Search & Analysis**.

4.  Enter a query statement in the search box, select a time range, and then click **Search & Analyze**.

5.  Click ![Bar chart - 001](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1976895951/p93116.png) to display the query results in a bar chart.

6.  On the **Properties** tab, configure the properties of the bar chart.

    **Note:**

    -   Bar charts are suitable to display query results if the number of returned log entries is no greater than 20. You can use a `LIMIT` clause to control the number of rectangular bars. Analysis results may not be clearly displayed if the chart contains excessive rectangular bars. You can use an `ORDER BY` clause to analyze the top N values of fields. In addition, we recommend that you configure no more than five fields for the Y-axis.
    -   You can use a grouped bar chart to display query results. However, the values represented by each rectangular bar in a group must be positively or negatively associated with each other.
    |Parameter|Description|
    |:--------|:----------|
    |X Axis|The categorical data.|
    |Y Axis|The numeric data. You can configure one or more fields for the Y-axis.|
    |Legend|The position where the legend is located in the chart. Valid values: Top, Bottom, Left, and Right.|
    |Format X-axis|The format in which data configured for the X-axis is displayed.|
    |Legend Width|Sets the width of the Legend.|
    |Y-axis Scale Density|Specifies the scale density of the Y-axis. Value range: 3 to 30.|
    |Margin|The distance between the axis and the borders of the chart, including **Top Margin**, **Bottom Margin**, **Right Margin**, and **Left Margin**.|


## Example

To analyze the `request_uri` with the top 10 number of visits and display the analysis results in a bar chart, execute the following statement:

```
* | select  request_uri, count(1) as count group by request_uri order by count desc limit 10
```

![Examples](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1823359951/p5717.png)

