# Import trace data from OpenTelemetry to Log Service

You can import trace data from OpenTelemetry to Log Service, or forward the trace data to Log Service by using the OpenTelemetry Collector.

A trace instance is created. For more information, see [Create a trace instance]().

## Import trace data from OpenTelemetry

When you use the OpenTelemetry protocol to import trace data to Log Service, you must configure the endpoint and authentication settings in OpenTelemetry. The following examples describe the required settings:

-   Endpoint settings

    -   An HTTPS endpoint is in the $\{endpoint\}/opentelemetry/v1/traces format, for example, https://test-project.cn-hangzhou-intranet.log.aliyuncs.com/opentelemetry/v1/traces.
    -   A gRPC endpoint is in the $\{endpoint\}:10010 format, for example, test-project.cn-hangzhou-intranet.log.aliyuncs.com:10010.

        **Note:** To ensure transmission security, you must enable Transport Layer Security \(TLS\) when you use the gRPC protocol.

    You must replace the $\{endpoint\} variable with the actual value. The following table describes the variable.

    |Variable|Description|Example|
    |--------|-----------|-------|
    |$\{endpoint\}|The endpoint. The format is $\{project\}.$\{region-endpoint\}, where:    -   $\{project\}: the name of the Log Service project.
    -   $\{region-endpoint\}: the endpoint of the project. You can access Log Service by using an endpoint of the Internet, the classic network, or a virtual private cloud \(VPC\). For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).
|test-project.cn-hangzhou.log.aliyuncs.com|

-   Authentication settings

    You can configure the authentication settings in the header of the gRPC or HTTPS protocol, or in the Resource field of the OpenTelemetry protocol. The following table describes the required fields.

    |OpenTelemetry Resource|gRPC/HTTPS Header Key|Description|Example|
    |----------------------|---------------------|-----------|-------|
    |sls.otel.project|x-sls-otel-project|The name of the Log Service project.|test-project|
    |sls.otel.instanceid|x-sls-otel-instance-id|The ID of the trace instance.|test-otel|
    |sls.otel.akid|x-sls-otel-ak-id|The AccessKey ID of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a Resource Access Management \(RAM\) user who has only the write permissions on the Log Service project. An AccessKey pair consists of an AccessKey ID and an AccessKey secret. For information about how to grant the write permissions on a specific project to a RAM user, see [Use custom policies to grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md). For information about how to obtain an AccessKey pair, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).

|None|
    |sls.otel.aksecret|x-sls-otel-ak-secret|The AccessKey secret of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a RAM user who has only the write permissions on the Log Service project.

|None|


## Forward trace data by using the OpenTelemetry Collector

1.  Click [here](https://github.com/open-telemetry/opentelemetry-collector-contrib/releases) to download the OpenTelemetry Collector.

2.  Configure the OpenTelemetry Collector.

    1.  Create a file named config.yaml.
    2.  Add the following code to the config.yaml file.

        Replace the variables in the following code with the actual values. For more information about the variables, see [Table 2](#table_wzj_90p_dvw).

        ```
        receivers:
          otlp:
            protocols:
              grpc:
                endpoint: "0.0.0.0:55680"
              http:
                endpoint: "0.0.0.0:55681"
        exporters:
          logging/detail:
            loglevel: debug
          alibabacloud_logservice/sls-trace:
            endpoint: "$\{endpoint\}"
            project: "$\{project\}"
            logstore: "$\{instance-id\}-traces"
            access_key_id: "$\{access-key-id\}"
            access_key_secret: "$\{access-key-secret\}"
          alibabacloud_logservice/sls-metric:
            endpoint: "$\{endpoint\}"
            project: "$\{project\}"
            logstore: "$\{instance-id\}-metrics"
            access_key_id: "$\{access-key-id\}"
            access_key_secret: "$\{access-key-secret\}"
           alibabacloud_logservice/sls-log:
            endpoint: "$\{endpoint\}"
            project: "$\{project\}"
            logstore: "$\{instance-id\}-logs"
            access_key_id: "$\{access-key-id\}"
            access_key_secret: "$\{access-key-secret\}"
        
        service:
          pipelines:
            traces:
              receivers: [otlp]           # Set the receivers parameter to [otlp](https://github.com/open-telemetry/opentelemetry-collector/tree/main/receiver/otlpreceiver). 
              exporters: [alibabacloud_logservice/sls-trace]   # Set the exporters parameter to [alibabacloud\_logservice/sls-trace](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/master/exporter/alibabacloudlogserviceexporter). 
              # for debug
              #exporters: [logging/detail,alibabacloud_logservice/sls-trace]
            metrics:
              receivers: [otlp]
              exporters: [alibabacloud_logservice/sls-metric]
            logs:
              receivers: [otlp]
              exporters: [alibabacloud_logservice/sls-log]
        ```

        |Variable|Description|Example|
        |--------|-----------|-------|
        |$\{endpoint\}|The endpoint of Log Service. The format is $\{region-endpoint\}. $\{region-endpoint\} is the endpoint of the project. You can access Log Service by using an endpoint of the Internet, the classic network, or a VPC. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|cn-hangzhou.log.aliyuncs.com|
        |$\{project\}|The name of the Log Service project.|test-project|
        |$\{instance-id\}|The ID of the trace instance.|test-traces|
        |$\{access-key-id\}|The AccessKey ID of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a RAM user who has only the write permissions on the Log Service project. For information about how to grant the write permissions on a specific project to a RAM user, see [Use custom policies to grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md). For information about how to obtain an AccessKey pair, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).

|None|
        |$\{access-key-secret\}|The AccessKey secret of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a RAM user who has only the write permissions on the Log Service project.

|None|

3.  Start the OpenTelemetry Collector.

    ```
    ./otelcontribcol_linux_amd64 --config="./config.yaml"
    ```


-   [View the details of a trace instance]()
-   [Query and analyze trace data]()

