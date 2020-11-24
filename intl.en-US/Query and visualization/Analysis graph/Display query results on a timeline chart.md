# Display query results on a timeline chart

This topic describes how to configure a timeline chart to display query results.

## Background information

A timeline chart records one or more events in chronological order. You can view data at specific time points. The data points in a timeline chart are continuous and accurate.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project that you want to manage.

3.  Choose **Log Storage** \> **Logstores**, and then click the Logstore that you want to manage.

4.  Enter a query statement in the search box, specify a time range, and then click **Search & Analyze**.

5.  On the Graph tab, click the ![Timeline chart - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7754588951/p94027.png) icon.

6.  On the **Properties** tab, configure the properties of the timeline chart.

    |Parameter|Description|
    |:--------|:----------|
    |Time Column|The field to be displayed on the time column. In most cases, the time field is selected.|
    |Display Columns|The field to be displayed on the display columns. You can select one or more fields for the display columns.|
    |Prompt Columns|The fields that are prompted when you move the pointer over a field value on the display columns.|
    |Display Count|The number of events to be displayed on the current page.|
    |Alternate Display|Specifies whether to use an alternate method to display field values on the time column and field values on the display columns. If you turn on the **Alternate Display** switch, the field values on the time column and field values on the display columns are alternatively displayed.|
    |Highlight Settings|Specifies whether to highlight fields. If you turn on the **Highlight Settings** switch, you can click **Add Highlight Rules** to set a highlight rule for fields.|


## Examples

To display the different statuses of the COVID-19 pandemic in real time, execute the following query statement:

```
type: news | select news_time as "release time", author as "publisher", title as "title", source as "origin",
case when strpos(url, 'https://') = 1 then substr(url, 9) when  strpos(url, 'http://') = 1 then substr(url, 8) else url end as url from log l right join 
(select max(version) as version from log) r on  l.version =  r.version order by news_time asc limit 50
```

![Timeline chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7754588951/p127140.png)

