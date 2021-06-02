# Query and analyze trace data

After you import trace data, you can query the trace data and set statistics by group.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Open the Trace Analysis page.

    1.  In the Log Application section, click **Trace Service**.

    2.  In the trace instance list, click the specified instance.

    3.  In the left-side navigation bar, click Trace Analysis.

3.  Set query conditions, select a time range, and then click **Search & Analyze**.

    Log Service provides preset query conditions for multiple fields, such as Service, Operation, Duration, Status, Attribute, and Resource. You only need to select these query conditions based on your business requirements. For more information, see [Trace data formats](/intl.en-US/Trace/Trace data formats.md).

    **Note:**

    -   The value of the **Duration** parameter in the query condition is measured in milliseconds. However, the unit of the condition expression is microseconds. For example, if you set the **Duration** parameter to 10 ms, this value is displayed as **duration \>= 10000** in the filter condition.
    -   By default, the value of the **Duration** parameter is a left-closed and right-open interval.
    -   The **Attribute** and **Resource** fields are of the JSON object type. Log Service allows you to filter the fields by key and value in this field.
    For example, you want to query the trace data of the last hour for the user service that has a latency of more than 10 ms. You can set the filter conditions shown in the following figure.

    ![Filter conditions of trace data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7606262261/p254120.png)

4.  On the Trace Analysis tab, view the query results.

    ![Query results](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7606262261/p254134.png)

5.  Set statistics by group

    1.  On the Trace Analysis tab, click **Statistics by Group**.

    2.  Select a condition for the statistics. In this example, select **service**.

    3.  View the statistical results.

        Log Service lists the information of each service by **service**, for example, the number of spans, queries per second \(QPS\), and average latency.

        ![Statistical results](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7606262261/p254149.png)


