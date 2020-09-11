# Connect Log Service with Jaeger

This topic describes how to connect Log Service with Jaeger.

Containers and serverless programming improve efficiency in software delivery and deployment. The evolution of the standard architecture includes the following changes:

-   The application architecture changes from single system-based to microservice-based. Business logic changes to calls and requests between microservices.
-   In terms of resources, traditional servers are gradually replaced with virtual resources.

![the evolution made by containers and serverless programming](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13157/15638735765875_en-US.png)

The standard architecture becomes more elastic due to these changes. However, these changes also pose more challenges to operations and maintenance \(O&M\) and diagnostics. To address this issue, a series of development and operations \(DevOps\)-oriented diagnostic and analysis systems emerge. These systems include centralized logging systems, centralized metrics systems, and distributed tracing systems.

In addition to Jaeger, Alibaba Cloud also supports the OpenTracing link tracing service [Tracing Analysis](https://www.aliyun.com/product/xtrace).

## Features of logging, metrics, and tracing systems

-   Logging

    A logging system records discrete events. For example, you can use the debug information or error messages of applications to troubleshoot issues.

-   Metrics

    A metrics system records data that can be aggregated. For example, you can use the depth of a queue as a metric and update the queue when an element is added or removed. You can also create a counter that calculates the number of HTTP requests.

-   Tracing

    A tracing system records the information during a request. For example, a system records the execution process and duration of a remote procedure call \(RPC\). A tracing system allows you to troubleshoot the performance issues of a system with ease.


![logging,metrics, and tracing systems](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13157/15638735765876_en-US.png)

The preceding information allows you to classify the existing systems. For example, Zipkin is a tracing system, and Prometheus is a metrics system and will integrate more and more tracing features. More and more systems will focus on logging and integrate more features of other fields. These systems include ELK and Alibaba Cloud Log Service. The aim is to approach the intersection area, as shown in the preceding figure.

For more information, see [Metrics, tracing, and logging](http://peter.bourgon.org/blog/2017/02/21/metrics-tracing-and-logging.html?spm=a2c4e.11153959.blogcont514488.18.463f30c2m1sv0g).

## Tracing

The following tracing systems are well-known:

-   Google Dapper
-   Google Operations
-   Twitter Zipkin
-   Go Appdash
-   Taobao EagleEye
-   Alibaba Cloud Ditecting
-   Ant Financial Yuntu
-   Shenma sTrace
-   AWS X-Ray

A large variety of distributed tracing systems exist and develop at high speed.

-   Code tracking
-   Data storage
-   Query display

The following figure shows the lifecycle of a distributed call. When a client makes a request, the request is first sent to the load balancer. The request passes through the authentication service and billing service. Then, the request is sent to the requested resources. After the lifecycle is complete, the system returns a result.

![the lifecycle of a distributed call](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13157/15638735775877_en-US.png)

After the system collects and stores the data, the distributed tracing system can be used to generate a timing diagram that contains a timeline. However, during data collection, the system obtains user code in an intrusive manner and the API operations of different systems are incompatible. This poses a significant challenge if you switch tracing systems.

![timing diagram](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13157/15638735775883_en-US.png)

## OpenTracing

The [OpenTracing](http://opentracing.io/?spm=a2c4e.11153959.blogcont514488.21.463f30c2m1sv0g) standard was introduced to prevent API compatibility issues among different distributed tracing systems. OpenTracing is a lightweight standardization layer. This layer is located between applications or class libraries and tracing or log analysis programs.

![OpenTracing and its partners](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13157/15638735775878_en-US.png)

-   Benefits:
    -   OpenTracing is included in Cloud Native Computing Foundation \(CNCF\) and provides standard concepts and data standards for global distributed tracing systems.
    -   OpenTracing provides platform-independent and vendor-independent APIs. This allows developers to add or change the implementation of tracing systems with ease.
-   Data model:

    In OpenTracing, a span is the primary building block of a trace. A trace can be treated as a directed acyclic graph \(DAG\). The DAG consists of multiple spans. The relationships between spans are called references. The following example shows a trace that consists of eight spans.

    ```
    Causal relationships between spans in a single trace.
            [Span A]  <------(the root span)
                |
         +------+------+
         |             |
     [Span B]      [Span C] <------(ChildOf: Span C is a child node of Span A.)
         |             |
     [Span D]      +---+-------+
                   |           |
               [Span E]    [Span F] >>> [Span G] >>> [Span H]
                                           ^
                                           ^
                                           ^
                             (FollowsFrom: Span G is invoked after Span F.)
    ```

    To view more details about a trace, use a timeline-based timing diagram, as shown in the following example.

    ```
    The time relationship between spans in a trace.
    --|-------|-------|-------|-------|-------|-------|-------|->time
     [Span A···················································]
       [Span B··············································]
          [Span D··········································]
        [Span C········································]
             [Span E·······]        [Span F··] [Span G··] [Span H··]
    ```

    Each span includes the following objects:

    -   An operation name.
    -   A start timestamp.
    -   A finish timestamp.
    -   A set of span tags. Span tags are key-value pairs. In a key-value pair, the key must be a string and the value can be a string, Boolean, or numeric value.
    -   A set of span logs. Each log contains a key-value pair and a timestamp. In a key-value pair, the key must be a string and the value can be of an arbitrary type. However, not all OpenTracing tracers must support every value type.
    -   A SpanContext. Each SpanContext contains the following components:
        -   An OpenTracing tracer must transmit the current trace status, for example, trace ID and span ID, across process boundaries based on a specified span.
        -   Baggage items are key-value pairs that are included in a trace. These key-value pairs must be transmitted across process boundaries.
    -   References: indicate the relationships between zero or more related spans. Relationships are established between spans based on a specified SpanContext.
    For more information about the OpenTracing data model, see [t1134099.md\#section\_umf\_lcv\_cfb](/intl.en-US/Product Introduction/Concepts.md).


For more information about the tracers that are supported by OpenTracing, see [OpenTracing](https://opentracing.io/docs/supported-tracers/). Popular OpenTracing tracers include [Jaeger](http://jaeger.readthedocs.io/en/latest/?spm=a2c4e.11153959.blogcont514488.24.463f30c2m1sv0g) and [Zipkin](https://zipkin.io/?spm=a2c4e.11153959.blogcont514488.25.463f30c2m1sv0g).

## Jaeger

Jaeger is an open-source distributed tracing system that is provided by Uber. This system is compatible with the OpenTracing API.

![the system structure of Jeager](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13157/15638735775879_en-US.png)

Jaeger consists of the following components, as shown in the preceding figure.

-   Jaeger client: implements language-specific SDKs that conform to the OpenTracing standard. An application calls an API operation to write data. A client library transmits trace information to an agent based on the sampling policy that is specified in the application.
-   Agent: works as a network daemon that listens to a UDP port. An agent is used to receive span data and send the span data to a collector in batches. An agent is a basic component that is deployed to all hosts. An agent allows clients libraries to connect to a collector without the required connection details.
-   Collector: receives data from agents and writes the data to a data store. A collector is a stateless component. You can run an arbitrary number of jaeger-collectors.
-   Data store: works as a pluggable component that allows you to write data to Cassandra and Elasticsearch.
-   Query: receives query requests, retrieves trace information from a data store, and then shows the result in the user interface \(UI\). The Query service is stateless. You can start multiple instances, and deploy the instances behind NGINX load balancers.

## Jaeger on Alibaba Cloud Log Service

[Jaeger on Alibaba Cloud Log Service](https://github.com/aliyun/aliyun-log-jaeger?spm=a2c4e.11153959.blogcont514488.27.463f30c2rnfGmn) is a Jaeger-based distributed tracing system that stores tracing data to Log Service for persistent storage. The tracing data can be queried and displayed by using the Jaeger API.

![Jaeger on Aliyun Log Service](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13157/15638735785880_en-US.png)

Benefits:

-   Jaeger allows you to only store data to Cassandra and Elasticsearch for persistent storage. You must maintain the stability of data stores and adjust the storage capacity. Jaeger on Alibaba Cloud Log Service uses Log Service to process large amounts of data. This allows you to benefit from the Jaeger distributed tracing technology, and focus on data stores.
-   The Jaeger UI can query and show traces, but requires more support for analysis and troubleshooting. Jaeger on Alibaba Cloud Log Service allows you to use the query and analysis features of Log Service to analyze system issues in an efficient manner.
-   Compared with Jaeger that uses Elasticsearch as the data store, the costs of Log Service are only 13% of the price of Elasticsearch when the pay-as-you-go billing method is adopted. For more information, see [Comparison between Log Service and the ELK stack in log query and analytics scenarios](/intl.en-US/Product Introduction/Benefits/Comparison between Log Service and the ELK stack in log query and analytics scenarios.md).

For more information about how to configure Jaeger on Alibaba Cloud Log Service, visit [GitHub](https://github.com/aliyun/jaeger/blob/master/README_CN.md).

