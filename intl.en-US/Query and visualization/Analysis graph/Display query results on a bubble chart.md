# Display query results on a bubble chart

This topic describes how to configure a bubble chart to display query results.

## Background information

A bubble chart shows the correlation between different categories of data. The correlation is displayed. This allows you to compare the position and area of the bubbles on the chart. You can use this type of chart to analyze the correlation between different categories of data.

A bubble chart consists of the following basic elements:

-   X-axis
-   Y-axis
-   Bubbles

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project that you want to manage.

3.  Choose **Log Storage** \> **Logstores**, and then click the Logstore that you want to manage.

4.  Enter a query statement in the search box, specify a time range, and then click **Search & Analyze**.

5.  On the Graph tab, click the **![Bubble chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6505565951/p120918.png)** icon.

6.  On the **Properties** tab, configure the properties of the bubble chart.

    |Parameter|Description|
    |:--------|:----------|
    |X Axis|The sequential data. In most cases, time series is selected.|
    |Y Axis|The numeric data. You can select one or more fields for the Y-axis.|
    |Numeric Column|You can select a field and specify a bubble size to indicate the field value.|
    |Shape|The shape of the bubble. Default value: Circle.|
    |Bubble Size|The size of the bubble. Valid values: 0 to 32.|
    |Bubble Color|The color of the bubble.|


## Examples

To analyze data exceptions based on bubble sizes, execute the following query statement:

```
* | select date_format(from_unixtime(__time__ - __time__% 60), '%H:%i') as minute, case 
when method = 'GET' and COUNT(1) > 9000 then 3 when method = 'GET' and COUNT(1) > 8500 then 2 when method = 'GET' and COUNT(1) > 8000 then 1 
when method = 'POST' and COUNT(1) > 3000 then 3
when method = 'DELETE' and COUNT(1) > 1500 then 3
else 0 end as level,  method group by minute, method order by minute asc limit 1000
```

![Bubble Chart - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7505565951/p120919.png)

