# View the details of a trace instance

After you import trace data to Log Service, you can view the service details, trace details, and trace dashboards on the details page of a trace instance.

Trace data is imported. For more information, see [Import trace data](/intl.en-US/Trace/Import trace data/Overview.md).

## View service details

After you import the trace data of an application to Log Service, you can view all services and service calls of the application in the service list. You can view information such as QPS, error rate, average latency, and P50 latency. You can also filter services and service operations by time range.

For example, if you want to monitor the status of a marketplace system, you can import the trace data of the system to Log Service. All services and service calls of the system are displayed in the service list. The services include user, order, frontend, queue-master, shipping, and job-server.

![Service list](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0066262261/p253265.png)

## View trace details

On the Trace Details page, you can view trace details, including the trace trail map and span data. For more information, see [View the details of a trace]().

## Query and analyze trace data

On the Trace Analysis page, you can set query conditions to filter trace data. You can also obtain statistics by service group. For more information, see [Query and analyze trace data](/intl.en-US/Trace/Query and analyze trace data.md).

## View the topological relationship of services

The topology of services shows the dependencies among these services. You can view the topological relationships among these services on the **Trace Analysis** \> **Dependency Analysis** tab or on the Topology Query page.

-   After you select a service, click the ![Aggregation](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0066262261/p254203.png) icon. The system shows only the services that depend on the selected service.
-   Click a service. The system shows the average latency, error rate, and number of spans of the service.

![Topological relationships](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0066262261/p254141.png)

## Query trace logs

Log Service records the logs that are generated when you import trace data. You can query and analyze the logs on the Search & Analysis page. For more information, see [Query logs](/intl.en-US/Index and query/Query logs.md).

## View the trace dashboards

By default, Log Service provides two dashboards. The following table describes the dashboards.

|Dashboard|Description|
|---------|-----------|
|Import Overview|Displays the analysis results of trace data. The results include the number of spans, average latency, services with top 10 latency, services with top 10 requests, and top latency methods.|
|Statistics|Displays the analysis results of trace data. The results include the number of spans, average latency, services with top 10 latency, and services with top 10 requests.|

## View storage settings

After you create a trace instance and import trace data to Log Service, Log Service generates a Logstore and a Metricstore to store the trace data, for example, \{instance\}-traces Logstore and \{instance\}-metrics Metricstore. For more information, see [Assets](/intl.en-US/Trace/Usage notes.md).

In the Storage Setting section, you can view the property of the [Logstore](/intl.en-US/Data Collection/Preparation/Manage a Logstore.md) or [Metricstore](/intl.en-US/Time Series Storage/Manage Metricstores.md).

