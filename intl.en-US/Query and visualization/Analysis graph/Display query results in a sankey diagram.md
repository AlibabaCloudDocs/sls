# Display query results in a sankey diagram

This topic describes how to configure a sankey diagram to display query results.

## Background information

The sankey diagram is a specific type of flow chart. It shows the flow from one set of values to another set of values. Sankey diagrams are applicable to scenarios such as network traffic flows. A sankey diagram contains values of three fields: `source`, `target`, and `value`. The `source` and `target` fields describe the source and target nodes, and the `value` field describes the flows from the `source` node to the `target` node.

Each sankey diagram consists of the following elements:

-   Node
-   Edge

The sankey diagram has the following features:

-   The start flow is equal to the end flow. The overall width of all main edges is equal to the total sum of all branch edges. This allows you to manage and maintain a balanced flow of all traffic.
-   The edge width in a row represents the traffic distribution in a specific status. The edge width in a row represents the distribution of traffic.
-   The width of an edge between two nodes represents the flow volume in a status.

The following table lists the data that can be displayed in a sankey diagram.

|source|target|value|
|:-----|:-----|:----|
|node1|node2|14|
|node1|node3|12|
|node3|node4|5|
|…|..|…|

The following figure shows the data relationships in a sankey diagram.

![Data relationships in a sankey diagram](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8823359951/p5746.png)

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project that you want to manage.

3.  Choose **Log Storage** \> **Logstores**, and then click the Logstore that you want to manage.

4.  Enter a query statement in the search box, specify a time range, and then click **Search & Analyze**.

5.  Click ![Sankey diagram - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9667895951/p93126.png) to display the query results in a sankey diagram.

6.  On the **Properties** tab, configure the properties of the sankey diagram.

    |Parameter|Description|
    |:--------|:----------|
    |Start Column|The start node.|
    |End Column|The end node.|
    |Value Column|The value of traffic between the start node and end node.|
    |Margin|The distance of the axis to the borders of the chart. This includes the **Top Margin**, **Bottom Margin**, **Right Margin**, and **Left Margin**.|


## Example of a simple sankey diagram

If a log entry contains the `source`, `target`, and `value` fields, you can execute a [Nested subquery](/intl.en-US/Index and query/Analysis grammar/Nested subquery.md) statement to obtain the total sum of all `steamValue` values.

```
* | select sourceValue, targetValue, sum(streamValue) as streamValue from (select sourceValue, targetValue,
 streamValue, __time__ from log group by  sourceValue, targetValue, streamValue, __time__ order by __time__ desc) group by sourceValue,
 targetValue
```

![Simple sankey diagram](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8823359951/p5744.png)

## Example of a sankey diagram for layer-7 access logs of Server Load Balance \(SLB\)

Log Service allows you to display query results of [Usage notes](/intl.en-US/Data Collection/Cloud product collection/SLB layer-7 access logs/Usage notes.md) in a sankey diagram.

`* | select COALESCE(client_ip, slbid, host) as source, COALESCE(host, slbid, client_ip) as dest, sum(request_length) as inflow group by grouping sets( (client_ip, slbid), (slbid, host))`

![Nested subquery](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8884026061/p5745.png)

