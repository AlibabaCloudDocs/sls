# Display query results on a pie chart

This topic describes how to configure a pie chart to display query results.

## Background information

A pie chart is used to indicate the percentages of different data types and compare different data types based on the arc length of each slice. You can use a pie chart to analyze the percentages of different field values based on the arc length of each slice. A pie chart is divided into multiple segments based on the percentages of various field values. The entire chart displays all field values. Each segment displays the percentage and the numeric value of a field. The sum of percentages from all segments is equal to 100%.

Each pie chart consists of the following elements:

-   Segment
-   Percentage in the text format
-   Legend

## Types

Log Service provides three types of pie charts: a standard pie chart, donut chart, and polar area chart.

-   Donut chart

    A donut chart is a standard pie chart with a hollow center. A donut chart has the following benefits:

    -   In addition to the information that a standard pie chart displays, a donut chart displays the total number of occurrences of all filed values.
    -   You can view the differences between the number of occurrences of the same value in two charts based on the ring length. This is more intuitive than comparing two standard pie charts.
-   Polar area chart

    A polar area chart is a column chart in the polar coordinate system. Each category of field values is represented by a segment with the same radian. The radius of a segment indicates the number of occurrences of a field value. Compared with a standard pie chart, a polar area chart has the following benefits:

    -   Pie charts are suitable to display query results if less than 10 log entries are returned. Polar area charts are suitable to display query results if the number of returned log entries ranges from 10 to 30.
    -   The area is the square of a radius. Therefore, the display of the polar area chart enlarges the differences among multiple types of data. This is best suited for a comparison of similar values.
    -   A circle can be used to display a periodic pattern. Therefore, you can use a polar area chart to analyze value change characteristics in different periods, such as weeks and months.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project that you want to manage.

3.  Choose **Log Storage** \> **Logstores**, and then click the Logstore that you want to manage.

4.  Enter a query statement in the search box, specify a time range, and then click **Search & Analyze**.

5.  On the Graph tab, click the ![Pie chart - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0750906951/p93117.png) icon.

6.  On the **Properties** tab, configure the properties of the pie chart.

    **Note:**

    -   Pie charts and donut charts can be used to display query results if less than 10 log entries are returned. You can use a LIMIT clause to control the number of segments. Analysis results may not be clearly displayed if the chart contains a large number of segments of different colors.
    -   We recommend that you use a polar area chart or column chart if the number of returned log entries exceeds 10.
    |Parameter|Description|
    |:--------|:----------|
    |Chart Types|The type of the chart. Valid values: Pie Chart, Donut Chart, and Polar Area Chart. Default value: Pie Chart.|
    |Legend Filter|The categorical data.|
    |Value Column|The values that correspond to different types of data.|
    |Legend|If you turn on the **Show Legend** switch, you can set this parameter to adjust the position of the legend in the chart.|
    |Format|The format in which data is displayed.|
    |Tick Text Format|Valid values: **Percentage** and **Category: Percentage**.|
    |Legend Width|The width of the legend.|
    |Margin|The distance between the axis and the borders of the chart. This includes the Top Margin, Bottom Margin, Left Margin, and Right Margin.|


## Example of a standard pie chart

To analyze the percentages of the `request_uri` field values, execute the following query statement:

```
* | select request_uri as uri , count(1) as c group by uri limit 10
```

![Pie chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0750906951/p5719.png)

## Example of a donut chart

To analyze the percentages of the `request_uri` field values, execute the following query statement:

```
* | select request_uri as uri , count(1) as c group by uri limit 10
```

![Donut chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0750906951/p5721.png)

## Example of a polar area chart

To analyze the percentages of the `request_uri` field values, execute the following query statement:

```
* | select request_uri as uri , count(1) as c group by uri limit 10
```

![Polar area chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0472026061/p5722.png)

