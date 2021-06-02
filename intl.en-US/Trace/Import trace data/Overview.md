# Overview

You can import cloud native trace data from OpenTelemetry to Log Service. You can also import trace data from other tracing systems to Log Service. This topic describes the methods that can be used to connect to the Trace application of Log Service.

## Import methods

![Import trace data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5246262261/p250850.png)

Log Service supports the following methods to import trace data:

-   Use OpenTelemetry, Jaeger, Zipkin, and OpenCensus to import trace data to Log Service. However, only an HTTPS or gRPC method can be used when you import trace data from Jaeger to Log Service.
-   Use the OpenTelemetry Collector to forward trace data from OpenTelemetry, Jaeger, Zipkin, OpenCensus, AWS X-Ray, and Splunk SignalFX to Log Service. In this case, all protocols are supported for Jaeger.
-   Use Logtail to forward trace data from SkyWalking to Log Service.
-   Use a custom protocol to import trace data to Log Service. Then, you can convert the format of the trace data to the OpenTelemetry format by using the data transformation feature of Log Service.

## Instructions on selecting import methods

When you select an import method to import trace data to Log Service, we recommend that you take note of the following instructions:

-   Use OpenTelemetry to import trace data.

    The OpenTelemetry protocol is a globally recognized standard that is used to import trace data. To connect with all required components, multiple open source software applications comply with the OpenTelemetry protocol.

-   Comply with the OpenTracing or OpenTelemetry protocol to connect with other open source systems.
-   If you do not use an open source standard protocol, use the same import method for all services from which trace data is imported in your tracing system. Otherwise, the collected trace data may be incomplete.

## Details of import methods

Log Service supports multiple import methods that have different automation levels of instrumentation and import complexity. These import methods are listed for some common tracing platforms, such as OpenTelemetry, SkyWalking, Jaeger, and Zipkin.

-   Import methods for trace data in different programming languages

    You can import trace data to Log Service by performing automatic instrumentation or semi-automatic instrumentation.

    -   Automatic instrumentation: Developers do not need to modify related frameworks or code. Tracing systems automatically set up instrumentation.
    -   Semi-automatic instrumentation: Developers need to manually install dependencies or modify related code.
    |Language|Import method|Automation level|Import complexity|
    |--------|-------------|----------------|-----------------|
    |Java|[Import trace data by using OpenTelemetry](/intl.en-US/Trace/Import trace data/New import methods/Import trace data from Java applications to Log Service by using OpenTelemetry SDK for Java.md)|Automatic|Low|
    |[Forward trace data by using the OpenTelemetry Collector](/intl.en-US/Trace/Import trace data/Integrate existing import methods/Import trace data from OpenTelemetry to Log Service.md)|Automatic|Medium|
    |[Import trace data from SkyWalking](/intl.en-US/Trace/Import trace data/Integrate existing import methods/Import trace data from SkyWalking.md)|Automatic|Medium|
    |Golang|[Import trace data by using OpenTelemetry](/intl.en-US/Trace/Import trace data/New import methods/Import trace data from Golang applications to Log Service by using OpenTelemetry SDK for Golang.md)|Semi-automatic|Low|
    |[Forward trace data by using the OpenTelemetry Collector](/intl.en-US/Trace/Import trace data/Integrate existing import methods/Import trace data from OpenTelemetry to Log Service.md)|Semi-automatic|Low|
    |Python|[Import trace data by using OpenTelemetry](/intl.en-US/Trace/Import trace data/New import methods/Import trace data from Python applications to Log Service by using OpenTelemetry SDK for Python.md)|Semi-automatic|Medium|
    |[Forward trace data by using the OpenTelemetry Collector](/intl.en-US/Trace/Import trace data/Integrate existing import methods/Import trace data from OpenTelemetry to Log Service.md)|Semi-automatic|Medium|
    |NodeJS|[Import trace data by using OpenTelemetry](/intl.en-US/Trace/Import trace data/New import methods/Import trace data from Node.js applications to Log Service by using OpenTelemetry SDK for JavaScript.md)|Semi-automatic|Medium|
    |[Forward trace data by using the OpenTelemetry Collector](/intl.en-US/Trace/Import trace data/Integrate existing import methods/Import trace data from OpenTelemetry to Log Service.md)|Semi-automatic|Medium|
    |PHP|[Import trace data by using Zipkin]()|Manual|High|
    |C++|[Import trace data by using Jaeger](/intl.en-US/Trace/Import trace data/New import methods/Import trace data from C++ applications to Log Service by using Jaeger SDK for C++.md)|Manual|High|
    |C\#|[Import trace data by using OpenTelemetry](/intl.en-US/Trace/Import trace data/New import methods/Import trace data from C# applications to Log Service by using OpenTelemetry SDK for .NET.md)|Semi-automatic|Medium|
    |[Forward trace data by using the OpenTelemetry Collector](/intl.en-US/Trace/Import trace data/Integrate existing import methods/Import trace data from OpenTelemetry to Log Service.md)|Semi-automatic|Medium|
    |[Import trace data from SkyWalking](/intl.en-US/Trace/Import trace data/Integrate existing import methods/Import trace data from SkyWalking.md)|Automatic|Medium|
    |Rust|[Import trace data by using OpenTelemetry](/intl.en-US/Trace/Import trace data/New import methods/Import trace data from Rust applications to Log Service by using OpenTelemetry SDK for Rust.md)|Manual|High|
    |[Forward trace data by using the OpenTelemetry Collector](/intl.en-US/Trace/Import trace data/Integrate existing import methods/Import trace data from OpenTelemetry to Log Service.md)|Manual|High|
    |Ruby|[Import trace data by using OpenTelemetry](/intl.en-US/Trace/Import trace data/New import methods/Import trace data from Ruby applications to Log Service by using OpenTelemetry SDK for Ruby.md)|Manual|High|
    |[Forward trace data by using the OpenTelemetry Collector](/intl.en-US/Trace/Import trace data/Integrate existing import methods/Import trace data from OpenTelemetry to Log Service.md)|Manual|High|

-   Import methods for trace data from different platforms

    |Tracing platform|Import method|Import complexity|
    |----------------|-------------|-----------------|
    |OpenTelemetry|[Import trace data from OpenTelemetry](/intl.en-US/Trace/Import trace data/Integrate existing import methods/Import trace data from OpenTelemetry to Log Service.md)|Low|
    |[Forward trace data by using the OpenTelemetry Collector](/intl.en-US/Trace/Import trace data/Integrate existing import methods/Import trace data from OpenTelemetry to Log Service.md)|Medium|
    |Jaeger|[Import trace data from Jaeger](/intl.en-US/Trace/Import trace data/Integrate existing import methods/Import trace data from Jaeger to Log Service.md)|Low|
    |[Forward trace data by using the OpenTelemetry Collector](/intl.en-US/Trace/Import trace data/Integrate existing import methods/Import trace data from Jaeger to Log Service.md)|Medium|
    |Zipkin|[Import trace data from Zipkin](/intl.en-US/Trace/Import trace data/Integrate existing import methods/Import trace data from Zipkin.md)|Low|
    |[Forward trace data by using the OpenTelemetry Collector](/intl.en-US/Trace/Import trace data/Integrate existing import methods/Import trace data from Zipkin.md)|Medium|
    |SkyWalking|[Forward trace data by using Logtail](/intl.en-US/Trace/Import trace data/Integrate existing import methods/Import trace data from SkyWalking.md)|Medium|
    |OpenCensus|[Forward trace data by using the OpenTelemetry Collector](/intl.en-US/Trace/Import trace data/Integrate existing import methods/Import trace data from OpenCensus to Log Service.md)|Medium|
    |AWS X-Ray|[Forward trace data by using the OpenTelemetry Collector](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/awsxrayreceiver)|High|
    |Splunk SignalFX|[Forward trace data by using the OpenTelemetry Collector](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/signalfxreceiver)|High|


## Scenarios

-   Build a tracing system

    If your system is connected to the Trace application for the first time, we recommend that you use OpenTelemetry to upload your trace data to Log Service. However, the related programming language may not support OpenTelemetry import methods or the import methods may not meet your requirements. In this case, you can use the import methods that support the OpenTracing or OpenCensus protocol to import data from Jaeger or Zipkin.

-   Upgrade an existing tracing system

    If your current system uses a tracing service, you can select an import method based on the actual scenario.

    -   The tracing system is stably running.
        -   If trace data can be uploaded to the OpenTelemetry Collector in the tracing system, you can use the OpenTelemetry Collector to forward the trace data to Log Service.
        -   If the tracing system uses a custom protocol or another protocol that is not the OpenTelemetry or OpenTracing protocol, you can print trace data to a file. Then, you can upload the file to Log Service by using Logtail, and use the data transformation feature to convert the data format to the OpenTelemetry format.
    -   The tracing system does not meet your requirements or needs to be upgraded.
        -   If the tracing system uses the OpenTracing or OpenCensus protocol, you can smooth and migrate trace data. In this case, you must upload trace data from the original system to the OpenTelemetry Collector, and then forward the trace data to Log Service. During this process, the original protocol is replaced by the OpenTelemetry protocol. Then, the OpenTelemetry protocol is used to import the trace data to Log Service.
        -   If the tracing system uses another protocol, you must replace the protocol. Otherwise, trace data may be incomplete during the replacement process.
-   Deploy on-premises tracing systems

    If you deploy your business applications in a data center, and only some gateway machines connect to the Internet or Express Connect circuits, you can deploy the OpenTelemetry Collector on these gateway machines. Then, you can send trace data from other machines to the gateway machines, and then forward the trace data to Log Service by using the OpenTelemetry Collector.


