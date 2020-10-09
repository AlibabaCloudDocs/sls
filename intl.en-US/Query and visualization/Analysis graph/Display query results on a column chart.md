# Display query results on a column chart

This topic describes how to configure a column chart to display query results.

## Background information

A column chart uses vertical or horizontal bars to present categorical values and count the number of values in each category. You can also use multiple rectangular bars to display values of the same category. Each rectangular bar represents a group or stack in the category. This way, you can perform fine-grained analysis on the value differences in a category.

Each column chart consists of the following elements:

-   X-axis \(horizontal\)
-   Y-axis \(vertical\)
-   Rectangular bar
-   Legend

By default, column charts in Log Service use vertical bars. Each rectangular bar has a fixed width and a variable height that indicates a value. You can use a grouped column chart to display the data if multiple columns of data are configured for the Y-axis.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Click the target project in the Projects section.

3.  Choose **Log Management** \> **Logstores**, click the management icon of the target Logstore, and then select **![Management icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9484688951/p52166.png)** \> **Search & Analysis**.

4.  Enter a query statement in the search box, select a time range, and then click **Search & Analyze**.

5.  Click ![Column chart - 001](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9376895951/p93115.png) to display the query results in a column chart.

6.  On the **Properties** tab, configure the properties of the column chart.

    **Note:** Column charts are suitable to display query results if the number of returned log entries is no greater than 20. You can use a `LIMIT` clause to control the number of rectangular bars. Analysis results may not be clearly displayed if the chart contains excessive rectangular bars. In addition, we recommend that you configure no more than five fields for the Y-axis.

    |Parameter|Description|
    |:--------|:----------|
    |X Axis|The categorical data.|
    |Y Axis|The numeric data. You can configure one or more fields for the Y-axis.|
    |Legend|The position where the legend is located in the chart. Valid values: Top, Bottom, Left, and Right.|
    |Format|The format in which data configured for the Y-axis is displayed.|
    |Y-axis Minimum Value|Sets the minimum value of the Y-axis.|
    |Y-axis Maximum Value|Sets the maximum value of the Y-axis.|
    |Legend Width|Sets the width of the legend.|
    |X-axis Scale Density|Specifies the scale density of the X-axis. Value range: 3 to 30.|
    |Stacking|If you turn on this switch, the Y-axis data is stacked.|
    |Show Values|If you turn on this switch, the value of each column is displayed.|
    |Margin|The distance between the axis and the borders of the chart, including **Top Margin**, **Bottom Margin**, **Right Margin**, and **Left Margin**.|


## Example of a simple column chart

To query the number of visits for each `http_referer` in the specified time range, execute the following statement:

```
* | select  http_referer, count(1) as count group by http_referer
```

Select `http_referer` for the X-axis and `count` for the Y-axis.

![Simple column chart](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1823359951/p5713.png)

## Example of a grouped column chart

To query the number of visits and the average bytes for each `http_referer` in the specified time range, execute the following statement:

```
* | select  http_referer, count(1) as count, avg(body_bytes_sent) as avg group by http_referer
```

Select `http_referer` for the X-axis. Select `count` and `avg` for the Y-axis.

![Grouped column chart](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1823359951/p5714.png)

