# Import trace data from Zipkin

You can directly send the trace data from Zipkin to Log Service. You can also forward trace data from Zipkin to Log Service by using OpenTelemetry Collector.

A trace instance is created. For more information, see [Create a trace instance]().

## Directly send trace data

When you use the Zipkin protocol to directly send trace data to Log Service, you need to configure endpoint information and authentication information on Zipkin.

**Note:** To ensure transmission security, we recommend that you send trace data over HTTPS.

-   Endpoint information

    -   \(Recommended\) V2 protocol: An HTTPS endpoint is in the $\{endpoint\}/zipkin/api/v2/spans format, for example, https://test-project.cn-hangzhou-intranet.log.aliyuncs.com/zipkin/api/v2/spans.
    -   V1 protocol: An HTTPS endpoint is in the$\{endpoint\}/zipkin/api/v1/spans format, for example https://test-project.cn-hangzhou.log.aliyuncs.com/zipkin/api/v1/spans.
    You must replace the $\{endpoint\} variable with the actual value. The following table describes the details of the variable.

    |Variable|Description|Example|
    |--------|-----------|-------|
    |$\{endpoint\}|The endpoint, in the format of $\{project\}.$\{region-endpoint\}, where:    -   $\{project\}: The name of the Log Service project.
    -   $\{region-endpoint\}: the domain name used to access the project. Internet and Alibaba Cloud internal network are supported. Alibaba Cloud internal network includes the classic network and virtual private network \(VPC\). For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).
|test-project.cn-hangzhou.log.aliyuncs.com|

-   Authentication information

    You can configure the authentication information in the HTTPS header. The following table describes the specific fields.

    |HTTPS Header Key|Description|Example|
    |----------------|-----------|-------|
    |x-sls-otel-project|The name of a Log Service project.|test-project|
    |x-sls-otel-instance-id|The ID of the trace instance.|test-traces|
    |x-sls-otel-ak-id|The AccessKey ID of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a Resource Access Management \(RAM\) user who has only the write permissions on the Log Service project. An AccessKey pair consists of an AccessKey ID and an AccessKey secret. For more information about how to grant the write permissions on a specific project to a RAM user, see [Authorize](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md). For information about how to obtain an AccessKey pair, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).

|None|
    |x-sls-otel-ak-secret|The AccessKey secret of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a RAM user who has only the write permissions on the Log Service project.

|None|


## Forward trace data by using OpenTelemetry Collector

You can send trace data from Zipkin to OpenTelemetry Collector by using the Zipkin SDK, and then forward the data to Log Service by using OpenTelemetry Collector. This method supports data transmission over HTTP or HTTPS.

1.  Install OpenTelemetry Collector.

    1.  [Download OpenTelemetry Collector](https://github.com/open-telemetry/opentelemetry-collector-contrib/releases).

    2.  Configure OpenTelemetry Collector.

        1.  Create a config.yaml file.
        2.  Add the following code to the config.yaml file.

            You must replace the variables in the following code based on your actual scenario. For more information about the variables, see [Table 2](#table_ett_bwh_tl4).

            ```
            receivers:
              zipkin:
                endpoint: 0.0.0.0:9411
            exporters:
              logging/detail:
                loglevel: debug
              alibabacloud_logservice/sls-trace:
                endpoint: "$\{endpoint\}"
                project: "$\{project\}"
                logstore: "$\{instance\}-traces"
                access_key_id: "$\{access-key-id\}"
                access_key_secret: "$\{access-key-secret\}"
            
            service:
              pipelines:
                traces:
                  receivers: [zipkin]      # Set the receivers parameter to[zipkin](https://github.com/open-telemetry/opentelemetry-collector/tree/main/receiver/zipkinreceiver). 
                  exporters: [alibabacloud_logservice/sls-trace] # Set the exporters parameter to[alibabacloud\_logservice/sls-trace](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/master/exporter/alibabacloudlogserviceexporter). 
                  # for debug
                  #exporters: [logging/detail,alibabacloud_logservice/sls-trace]
            ```

            |Parameter|Description|Example|
            |---------|-----------|-------|
            |$\{endpoint\}|The endpoint of Log Service, in the format of $\{region-endpoint\}, where $\{region-endpoint\} is the domain name used to access the project. Internet and Alibaba Cloud internal network are supported. Alibaba Cloud internal network includes the classic network and VPC. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|cn-hangzhou.log.aliyuncs.com|
            |$\{project\}|The name of the Log Service project.|test-project|
            |$\{instance\}|The name of the trace instance.|test-traces|
            |$\{access-key-id\}|The AccessKey ID of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a RAM user who has only the write permissions on the Log Service project. For more information about how to grant the write permissions on a specific project to a RAM user, see [Authorize](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md). For more information about how to abtain an AccessKey pair, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).

|None|
            |$\{access-key-secret\}|The AccessKey secret of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a RAM user who has only the write permissions on the Log Service project.

|None|

    3.  Start OpenTelemetry Collector.

        ```
        ./otelcontribcol_linux_amd64 --config="./config.yaml"
        ```

2.  Configure Zipkin.

    Change the output address of Zipkin to the address that is monitored by OpenTelemetry Collector. For example, if the address of OpenTelemetry Collector is $\{collector-host\}, set the output address of Zipkin to $\{collector-host\}:9411.


-   [View the details of a trace instance]()
-   [Query and analyze trace data]()

