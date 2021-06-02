# Import trace data from OpenCensus to Log Service

You can send trace data from OpenCensus to the OpenTelemetry Collector by using OpenCensus SDK, and then forward the data to Log Service by using the OpenTelemetry Collector. This topic describes how to forward trace data to Log Service by using the OpenTelemetry Collector.

A trace instance is created. For more information, see [Create a trace instance]().

## Procedure

1.  Install the OpenTelemetry Collector.

    1.  Click [here](https://github.com/open-telemetry/opentelemetry-collector-contrib/releases) to download the OpenTelemetry Collector.

    2.  Configure the OpenTelemetry Collector.

        1.  Create a file named config.yaml.
        2.  Add the following code to the config.yaml file.

            Replace the variables in the following code with the actual values. For more information about the variables, see [Table 1](#table_dii_mmp_0x8).

            ```
            receivers:
              opencensus:
                endpoint: 0.0.0.0:6850
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
                  receivers: [opencensus]    # Set the receivers parameter to [opencensus](https://github.com/open-telemetry/opentelemetry-collector/tree/main/receiver/opencensusreceiver). 
                  exporters: [alibabacloud_logservice/sls-trace]    # Set the exporters parameter to [alibabacloud\_logservice/sls-trace](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/master/exporter/alibabacloudlogserviceexporter). 
                  # for debug
                  #exporters: [logging/detail,alibabacloud_logservice/sls-trace]
            ```

            |Variable|Description|Example|
            |--------|-----------|-------|
            |$\{endpoint\}|The endpoint of Log Service. The format is $\{region-endpoint\}. $\{region-endpoint\} is the endpoint of the project. You can access Log Service by using an endpoint of the Internet, the classic network, or a virtual private cloud \(VPC\). For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|cn-hangzhou.log.aliyuncs.com|
            |$\{project\}|The name of the Log Service project.|test-project|
            |$\{instance\}|The name of the trace instance.|test-traces|
            |$\{access-key-id\}|The AccessKey ID of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a Resource Access Management \(RAM\) user who has only the write permissions on the Log Service project. An AccessKey pair consists of an AccessKey ID and an AccessKey secret. For information about how to grant the write permissions on a specific project to a RAM user, see [Use custom policies to grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md). For information about how to obtain an AccessKey pair, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).

|None|
            |$\{access-key-secret\}|The AccessKey secret of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a RAM user who has only the write permissions on the Log Service project.

|None|

    3.  Start the OpenTelemetry Collector.

        ```
        ./otelcontribcol_linux_amd64 --config="./config.yaml"
        ```

2.  Configure OpenCensus.

    Change the endpoint of OpenCensus to the endpoint on which the OpenTelemetry Collector listens. For example, if the endpoint of the OpenTelemetry Collector is $\{collector-host\}, you must set the endpoint of OpenCensus to $\{collector-host\}:6850.


-   [View the details of a trace instance]()
-   [Query and analyze trace data]()

