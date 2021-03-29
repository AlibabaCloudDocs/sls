# 接入OpenCensus Trace数据

您可以通过OpenCensus SDK将OpenCensus平台上的Trace数据发送至OpenTelemetry Collector，再通过OpenTelemetry Collector转发至日志服务。本文介绍通过OpenTelemetry Collector转发Trace数据到日志服务的操作步骤。

已创建Trace实例。更多信息，请参见[创建Trace实例]()。

## 操作步骤

1.  安装OpenTelemetry Collector。

    1.  [下载OpenTelemetry Collector](https://github.com/open-telemetry/opentelemetry-collector-contrib/releases)。

    2.  配置OpenTelemetry Collector。

        1.  创建config.yaml文件。
        2.  在config.yaml文件中添加如下代码。

            如下代码中的变量需根据实际情况替换。关于变量的详细说明，请参见[表 1](#table_dii_mmp_0x8)。

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
                  receivers: [opencensus]    #接收端配置为[opencensus](https://github.com/open-telemetry/opentelemetry-collector/tree/main/receiver/opencensusreceiver)。
                  exporters: [alibabacloud_logservice/sls-trace]    #发送端配置为[alibabacloud\_logservice/sls-trace](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/master/exporter/alibabacloudlogserviceexporter)。
                  # for debug
                  #exporters: [logging/detail,alibabacloud_logservice/sls-trace]
            ```

            |变量|说明|示例|
            |--|--|--|
            |$\{endpoint\}|日志服务的接入地址，格式为$\{project\}.$\{region-endpoint\}，其中：            -   $\{project\}：日志服务Project名称。
            -   $\{region-endpoint\}：Project访问域名，支持公网和阿里云内网（经典网络、VPC）。更多信息，请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。
|test-project.cn-hangzhou.log.aliyuncs.com|
            |$\{project\}|日志服务Project名称。|test-project|
            |$\{instance\}|Trace服务实例名称。|test-traces|
            |$\{access-key-id\}|阿里云账号AccessKey ID。建议您使用只具备日志服务Project写入权限的RAM用户的AccessKey（包括AccessKey ID和AccessKey Secret）。授予RAM用户向指定Project写入数据权限的具体操作，请参见[授权](/intl.zh-CN/开发指南/访问控制RAM/RAM自定义授权场景.md)。如何获取AccessKey的具体操作，请参见[访问密钥](/intl.zh-CN/开发指南/API 参考/访问密钥.md)。

|无|
            |$\{access-key-secret\}|阿里云账号AccessKey Secret。建议您使用只具备日志服务Project写入权限的RAM用户的AccessKey。

|无|

    3.  启动OpenTelemetry Collector。

        ```
        ./otelcontribcol_linux_amd64 --config="./config.yaml"
        ```

2.  配置OpenCensus。

    将OpenCensus的输出端地址改为OpenTelemetry Collector监听的地址。例如OpenTelemetry Collector的地址为$\{collector-host\}，则将OpenCensus的输出端地址设置为$\{collector-host\}:6850。


-   [查看Trace实例详情]()
-   [查询和分析Trace数据]()

