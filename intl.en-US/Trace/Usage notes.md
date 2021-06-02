# Usage notes

Log Service provides the distributed tracing feature based on the native OpenTelemetry protocol. You can use this feature to import, store, analyze, visualize trace data, configure alerts for trace data, and manage trace data based on artificial intelligence \(AI\). This topic describes the background information, import methods, assets, and expenses that are related to the distributed tracing feature \(the Trace application\) of Log Service.

## Background information

In modern IT systems, especially cloud-native systems and microservice systems, an external request often requires multiple internal services, middleware, and machines to call each other. During the call process, various issues may occur and lead to external service failure or increased latency. This affects user experience. To identify and analyze issues, you can use the distributed tracing feature.

Distributed tracing can provide the call relationships, latency, and results of an entire service call link. This feature is suitable for cloud-native systems, distributed systems, microservices, and other systems that consist of multiple interactive services.

[OpenTelemetry](https://opentelemetry.io/) is a global standard of distributed tracing, and is compatible with [OpenTracing](https://opentracing.io/) and [OpenCensus](https://opencensus.io/) clients. OpenTelemetry consists of a collection of APIs, SDKs, and tools. You can use OpenTelemetry to instrument, generate, collect, and export various telemetry data, including traces, logs, and metrics.

## Trace application of Log Service

OpenTelemetry defines data formats, and generates, collects, and sends data. However, OpenTelemetry does not analyze or visualize data, or configure alerts for data. The Trace application of Log Service is implemented based on the OpenTelemetry protocol. You can use the application to collect trace data from OpenTelemetry and other platforms, such as Jaeger, Zipkin, and SkyWalking. You can also store, analyze, and visualize trace data, and configure alerts for trace data.

![Architecture of the Trace application](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1146262261/p249796.png)

-   Multiple import methods
    -   You can import trace data over multiple protocols such as OpenTelemetry, Jaeger, and Zipkin.
    -   You can import trace data in more than 10 programming languages.
    -   You can import trace data from multiple trace platforms.
    -   You can import trace data over the Internet, Alibaba Cloud internal network, and global acceleration \(GA\) network. The Alibaba Cloud internal network includes the classic network and virtual private cloud \(VPC\).
-   Complies with the standard specification of OpenTelemetry Trace 1.0

    The trace data format of Log Service complies with OpenTelemetry Trace 1.0 and meets the format requirements for trace data in cloud-native systems and microservices.

-   High performance

    You can import petabytes of data per day, extract and analyze metrics, precompute data, and sample 100% of trace data in large-scale scenarios.

-   Scalability

    You can customize log storage cycles. The storage capacity of each Logstore can be dynamically scaled to meet your business requirements.

-   Various trace features

    You can visualize trace details, view service details, query trace data, analyze dependencies, and customize SQL analysis based on your specific requirements.

-   High compatibility with downstream applications

    Log Service trace data and calculated metrics are compatible with various stream processing platforms and offline computing engines. The Trace application also supports customized subscription data.

-   Provides multiple built-in AIOps algorithms

    The Trace application can automatically analyze the impact of traces on performance and error rate. This helps developers identify the root causes of various issues in complex scenarios.


## Assets

All assets that are created by using the Trace application of Log Service are stored in a specified project. The project consists of the following assets:

-   Logstore

    **Note:** You cannot update or delete indexes in the following Logstores. Otherwise, the Trace application becomes unavailable.

    -   \{instance\}-traces: stores the reported raw trace data.
    -   \{instance\}-traces-metrics: stores the intermediate results of aggregated metrics after trace data is calculated.
    -   \{instance\}-traces-deps: stores the intermediate results of dependencies after trace data is calculated.
    -   \{instance\}-logs: stores the reported raw logs.
-   Metricstore

    \{instance\}-metrics: stores the reported metrics.

-   Scheduled SQL
    -   \{instance\}-metric\_info: queries the metrics that are used to aggregate trace data.
    -   \{instance\}-service: queries the dependencies that are used to aggregate trace data.
    -   \{instance\}-service\_name\_host: queries the dependencies that are used to aggregate the service, name, and host granularities.
    -   \{instance\}-service\_name\_host\_resource: queries the dependencies that are used to aggregate the service, name, host, resource granularities.
-   Dashboard
    -   Import overview: displays the basic information of imported trace data, such as the number of traces, number of spans, and import status of each service.
    -   Statistics: displays the statistics of imported trace data, such as latency, QPS, and error rate.

## Billing

When you use the Trace application, you are charged basic fees for Log Service resources. The basic fees include the index traffic fee, storage fee, read/write traffic fee, and Internet access fee. For more information about billable items, see [Billable items and billing method](/intl.en-US/Pricing/Billable items and billing method.md).

