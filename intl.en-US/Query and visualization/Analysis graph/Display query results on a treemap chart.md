# Display query results on a treemap chart

This topic describes how to configure a treemap chart to display query results.

## Background information

A treemap chart includes multiple rectangles that represent the data volume. A larger rectangle area represents a larger proportion of the categorical data.

Rectangles in a treemap chart are sorted.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project that you want to manage.

3.  Choose **Log Storage** \> **Logstores**, and then click the Logstore that you want to manage.

4.  Enter a query statement in the search box, specify a time range, and then click **Search & Analyze**.

5.  On the Graph tab, click the ![Treemap chart - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4271906951/p93129.png) icon.

6.  On the **Properties** tab, configure the properties of the treemap chart.

    |Parameter|Description|
    |:--------|:----------|
    |**Legend Filter**|The field that includes categorical data.|
    |**Value Column**|The numeric value of a field. A greater field value represents a larger rectangle area.|


## Examples

To query the distribution of hostnames in NGINX logs, execute the following query statement:

```
* | select host, count(1) as count group by host order by count desc limit 1000 
```

Select `host` for **Legend Filter** and `count` for **Value Column**.

![Treemap chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4271906951/p9594.png)

