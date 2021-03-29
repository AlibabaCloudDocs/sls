# 接入OpenTelemetry Trace数据

您可以通过直接发送方式或OpenTelemetry Collector转发方式，将OpenTelemetry平台上的Trace数据发送到日志服务。

已创建Trace实例。更多信息，请参见[创建Trace实例]()。

## 直接发送

使用OpenTelemetry协议直接发送Trace数据到日志服务时，您需要在OpenTelemetry平台上配置接入点信息和鉴权信息，详细说明如下：

-   接入点信息

    -   HTTPS协议的接入点为$\{endpoint\}/opentelemetry/v1/traces，例如https://test-project.cn-hangzhou-intranet.log.aliyuncs.com/opentelemetry/v1/traces。
    -   gRPC协议的接入点为$\{endpoint\}:10010，例如test-project.cn-hangzhou-intranet.log.aliyuncs.com:10010。

        **说明：** 为保证传输安全性，使用gRPC协议时必须开启TLS。

    其中，$\{endpoint\}需根据实际情况替换，详细说明如下表所示。

    |变量|说明|示例|
    |--|--|--|
    |$\{endpoint\}|接入地址，格式为$\{project\}.$\{region-endpoint\}，其中：    -   $\{project\}：日志服务Project名称。
    -   $\{region-endpoint\}：Project访问域名，支持公网和阿里云内网（经典网络、VPC）。更多信息，请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。
|test-project.cn-hangzhou.log.aliyuncs.com|

-   鉴权信息

    您可以在gRPC协议、HTTPS协议的Header中或者OpenTelemetry协议的Resource字段中配置鉴权信息，具体字段及详细说明如下表所示。

    |OpenTelemetry Resource|gRPC/HTTPS Header Key|说明|示例|
    |----------------------|---------------------|--|--|
    |sls.otel.project|x-sls-otel-project|日志服务Project。|test-project|
    |sls.otel.instanceid|x-sls-otel-instance-id|Trace服务实例ID。|test-otel|
    |sls.otel.akid|x-sls-otel-ak-id|阿里云账号AccessKey ID。建议您使用只具备日志服务Project写入权限的RAM用户的AccessKey（包括AccessKey ID和AccessKey Secret）。授予RAM用户向指定Project写入数据权限的具体操作，请参见[授权](/intl.zh-CN/开发指南/访问控制RAM/RAM自定义授权场景.md)。如何获取AccessKey的具体操作，请参见[访问密钥](/intl.zh-CN/开发指南/API 参考/访问密钥.md)。

|无|
    |sls.otel.aksecret|x-sls-otel-ak-secret|阿里云账号AccessKey Secret。建议您使用只具备日志服务Project写入权限的RAM用户的AccessKey。

|无|


## 通过OpenTelemetry Collector转发

1.  [下载OpenTelemetry Collector](https://github.com/open-telemetry/opentelemetry-collector-contrib/releases)。

2.  配置OpenTelemetry Collector。

    1.  创建config.yaml文件。
    2.  在config.yaml文件中添加如下代码。

        如下代码中的变量需根据实际情况替换。关于变量的详细说明，请参见[表 2](#table_wzj_90p_dvw)。

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
              receivers: [otlp]           #接收端配置为[otlp](https://github.com/open-telemetry/opentelemetry-collector/tree/main/receiver/otlpreceiver)。
              exporters: [alibabacloud_logservice/sls-trace]   #发送端配置为[alibabacloud\_logservice/sls-trace](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/master/exporter/alibabacloudlogserviceexporter)。
              # for debug
              #exporters: [logging/detail,alibabacloud_logservice/sls-trace]
            metrics:
              receivers: [otlp]
              exporters: [alibabacloud_logservice/sls-metric]
            logs:
              receivers: [otlp]
              exporters: [alibabacloud_logservice/sls-log]
        ```

        |变量|说明|示例|
        |--|--|--|
        |$\{endpoint\}|日志服务的接入地址，格式为$\{region-endpoint\}。其中，$\{region-endpoint\}为Project访问域名，支持公网和阿里云内网（经典网络、VPC）。更多信息，请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。|cn-hangzhou.log.aliyuncs.com|
        |$\{project\}|日志服务Project名称。|test-project|
        |$\{instance-id\}|Trace服务实例ID。|test-traces|
        |$\{access-key-id\}|阿里云账号AccessKey ID。建议您使用只具备日志服务Project写入权限的RAM用户的AccessKey。授予RAM用户向指定Project写入数据权限的具体操作，请参见[授权](/intl.zh-CN/开发指南/访问控制RAM/RAM自定义授权场景.md)。如何获取AccessKey的具体操作，请参见[访问密钥](/intl.zh-CN/开发指南/API 参考/访问密钥.md)。

|无|
        |$\{access-key-secret\}|阿里云账号AccessKey Secret。建议您使用只具备日志服务Project写入权限的RAM用户的AccessKey。

|无|

3.  启动OpenTelemetry Collector。

    ```
    ./otelcontribcol_linux_amd64 --config="./config.yaml"
    ```


-   [查看Trace实例详情]()
-   [查询和分析Trace数据]()

