# Display query results on a lifecycle chart

This topic describes how to configure a lifecycle chart to display query results.

## Background information

A lifecycle chart displays the time that is required to load tasks. A rectangular block in a lifecycle chart indicates when the loading of a task starts and ends. The length of the rectangular block indicates the time that is required to load the task. The start position that is mapped to the X-axis indicates the start time. The end position that is mapped to the X-axis indicates the end time. The value on the Y-axis indicates the task name.

We recommend that you limit the number to 20 tasks. Otherwise, the rectangular blocks will be narrow and the analysis results are not clear.

Each lifecycle chart consists of the following elements:

-   X-axis start value
-   X-axis end value
-   Y-axis
-   Rectangular block

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project that you want to manage.

3.  Choose **Log Storage** \> **Logstores**, and then click the Logstore that you want to manage.

4.  Enter a query statement in the search box, specify a time range, and then click **Search & Analyze**.

5.  On the Graph tab, click the ![Lifecycle chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3765565951/p125240.png) icon.

6.  On the **Properties** tab, configure the properties of the lifecycle chart.

    |Parameter|Description|
    |:--------|:----------|
    |X-axis Start Value|The start time field.|
    |X-axis End Value|The end time field.|
    |Y Axis|The field that contains the task names.|


## Examples

To view the time when each task starts and ends, execute the following query statement:

```
* | select min(__time__) startTime, max(__time__) as endTime , request_uri group by request_uri
```

![Lifecycle Chart - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3765565951/p125241.png)

