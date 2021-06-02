# Import trace data from Jaeger to Log Service

You can import trace data from Jaeger to Log Service, or forward the trace data to Log Service by using the OpenTelemetry Collector.

A trace instance is created. For more information, see [Create a trace instance]().

## Import trace data from Jaeger

When you use the Jaeger protocol to import trace data to Log Service, you must configure the endpoint and authentication settings in the Jaeger tracing system. The following examples describe the required settings:

-   Endpoint settings

    -   An HTTPS endpoint is in the $\{endpoint\}/jaeger/api/traces format, for example, https://test-project.cn-hangzhou-intranet.log.aliyuncs.com/jaeger/api/traces.
    -   A gRPC endpoint is in the $\{endpoint\}:10010 format, for example, test-project.cn-hangzhou-intranet.log.aliyuncs.com:10010.

        **Note:** To ensure transmission security, you must enable Transport Layer Security \(TLS\) when you use the gRPC protocol.

    You must replace the $\{endpoint\} variable with the actual value. The following table describes the variable.

    |Variable|Description|Example|
    |--------|-----------|-------|
    |$\{endpoint\}|The endpoint. The format is $\{project\}.$\{region-endpoint\}, where:    -   $\{project\}: the name of the Log Service project.
    -   $\{region-endpoint\}: the endpoint of the project. You can access Log Service by using an endpoint of the Internet, the classic network, or a virtual private cloud \(VPC\). For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).
|test-project.cn-hangzhou.log.aliyuncs.com|

-   Authentication settings

    You can configure the authentication settings in the header of the gRPC or HTTPS protocol, or in the Tag field of the Jaeger protocol. The following table describes the required fields.

    |Jaeger Tag|gRPC/HTTPS Header Key|Description|Example|
    |----------|---------------------|-----------|-------|
    |sls.otel.project|x-sls-otel-project|The name of the Log Service project.|test-project|
    |sls.otel.instanceid|x-sls-otel-instance-id|The ID of the trace instance.|test-traces|
    |sls.otel.akid|x-sls-otel-ak-id|The AccessKey ID of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a Resource Access Management \(RAM\) user who has only the write permissions on the Log Service project. An AccessKey pair consists of an AccessKey ID and an AccessKey secret. For information about how to grant the write permissions on a specific project to a RAM user, see [Use custom policies to grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md). For information about how to obtain an AccessKey pair, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).

|None|
    |sls.otel.aksecret|x-sls-otel-ak-secret|The AccessKey secret of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a RAM user who has only the write permissions on the Log Service project.

|None|


## Forward trace data by using the OpenTelemetry Collector

1.  Install the OpenTelemetry Collector.

    1.  Click [here](https://github.com/open-telemetry/opentelemetry-collector-contrib/releases) to download the OpenTelemetry Collector.

    2.  Configure the OpenTelemetry Collector.

        1.  Create a file named config.yaml.
        2.  Add the following code to the config.yaml file.

            Replace the variables in the following code with the actual values. For more information about the variables, see [Table 2](#table_j3y_1ju_nig).

            ```
            receivers:
              jaeger:
                protocols:
                  grpc:
                    endpoint: 0.0.0.0:6831
                  thrift_binary:
                    endpoint: 0.0.0.0:6832
                  thrift_compact:
                    endpoint: 0.0.0.0:6833
                  thrift_http:
                    endpoint: 0.0.0.0:6834
            exporters:
              logging/detail:
                loglevel: debug
              alibabacloud_logservice/sls-trace:
                endpoint: "$\{endpoint\}"
                project: "$\{project\}"
                logstore: "$\{instance\}-traces"
                access_key_id: "$\{service\}"
                access_key_secret: "$\{access-key-secret\}"
            
            service:
              pipelines:
                traces:
                  receivers: [jaeger]        # Set the receivers parameter to [jaeger](https://github.com/open-telemetry/opentelemetry-collector/tree/master/receiver/jaegerreceiver). 
                  exporters: [alibabacloud_logservice/sls-trace]       # Set the exporters parameter to [alibabacloud\_logservice/sls-trace](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/master/exporter/alibabacloudlogserviceexporter). 
                  # for debug
                  #exporters: [logging/detail,alibabacloud_logservice/sls-trace]
            ```

            |Variable|Description|Example|
            |--------|-----------|-------|
            |$\{endpoint\}|The endpoint of Log Service. The format is $\{region-endpoint\}. $\{region-endpoint\} is the endpoint of the project. You can access Log Service by using an endpoint of the Internet, the classic network, or a VPC. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|cn-hangzhou.log.aliyuncs.com:10010|
            |$\{project\}|The name of the Log Service project.|test-project|
            |$\{instance\}|The name of the trace instance.|test-traces|
            |$\{access-key-id\}|The AccessKey ID of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a RAM user who has only the write permissions on the Log Service project. For information about how to grant the write permissions on a specific project to a RAM user, see [Use custom policies to grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md). For information about how to obtain an AccessKey pair, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).

|None|
            |$\{access-key-secret\}|The AccessKey secret of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a RAM user who has only the write permissions on the Log Service project.

|None|

    3.  Start the OpenTelemetry Collector.

        ```
        ./otelcontribcol_linux_amd64 --config="./config.yaml"
        ```

2.  Configure Jaeger.

    Change the endpoint of Jaeger to the endpoint on which the OpenTelemetry Collector listens. For example, if the endpoint of the OpenTelemetry Collector is $\{collector-host\}, you must set the endpoint of Jaeger to $\{collector-host\}:6833.

    **Note:** If an error is returned by the OpenTelemetry Collector because data fails to be parsed, you can switch the four receiving modes of the Jaeger receiver to fix the error.


-   [View the details of a trace instance]()
-   [Query and analyze trace data]()

