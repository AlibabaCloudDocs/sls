# View the details of a trace

After you import trace data to Log Service, you can view the trace data, including the trace trail map and span data.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Open the Trace Analysis page.

    1.  In the Log Application section, click **Trace Service**.

    2.  In the trace instance list, click the specified instance.

    3.  In the left-side navigation bar, click Trace Analysis.

3.  On the Trace Analysis tab, click the specific trace data.

4.  View the details of the trace.

    ![Trace data details](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8826262261/p254100.png)

    |Ordinal number|Description|
    |--------------|-----------|
    |1|The trace map shows the distribution of tracking links and spans. The following list describes the details:    -   Different colors represent different services. In this example, the blue color represents the front-end service.
    -   The length of the black line in the trace map represents the time that is consumed by a span.
    -   The timeline represents the time range of the trace. |
    |2|Each row in this section represents a span and shows the hierarchical relationship between the parent span and the child span. Each parent span is preceded by a number, which indicates the number of child spans owned by the parent span. In this section, you can perform the following operations:    -   Click the ![Number](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8826262261/p254319.png) icon to collapse or expand a span.
    -   Select a span and click the ![Focus icon](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8826262261/p254320.png) icon. The system displays only the data of the span.
    -   Click the ![Defocus icon](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8826262261/p254336.png) icon to defocus a span. |
    |3|When you click a span, the following information is displayed:    -   Service: the name of the service to which the span belongs.
    -   Call: the name of the API operation.
    -   Property: the metadata of the span.
    -   Resource: the resources that must be called to generate the span.
    -   Details: the details of the span. For more information about field descriptions, see [Trace data formats](/intl.en-US/Trace/Trace data formats.md).
    -   Log: the log content that is related to the span. |


