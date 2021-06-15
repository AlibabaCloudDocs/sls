# 接入Trace数据概述

日志服务支持OpenTelemetry Trace数据的原生接入，还支持通过其他Trace系统接入Trace数据。本文介绍日志服务所支持的Trace数据接入方案。

## 接入方案总览

![Trace数据接入](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7534646161/p250850.png)

日志服务支持如下接入方案。

-   使用OpenTelemetry、Jaeger（目前仅支持https、grpc方式）、Zipkin、OpenCensus等直接将Trace数据接入到日志服务。
-   使用OpenTelemetry Collector转发OpenTelemetry、Jaeger（全协议支持）、Zipkin、OpenCensus、AWS X-Ray、SignalFX（Splunk）等平台上的Trace数据到日志服务。
-   使用Logtail转发SkyWalking的Trace数据到日志服务。
-   使用自定义协议将Trace数据接入到日志服务，并通过日志服务加工功能将Trace数据格式转换为OpenTelemetry格式。

## 选择接入方案的原则

建立Trace方案的重点任务是产生并上传Trace数据，因此选择合适的接入方案非常重要，主要原则如下：

-   推荐使用OpenTelemetry接入方案。

    OpenTelemetry是目前全球公认的Trace接入标准。众多开源软件都会遵循OpenTelemetry标准，便于打通各个组件。

-   遵循OpenTracing、OpenTelemetry标准，便于与其他开源系统打通。
-   如果不使用开源的标准协议，则需确保系统中的各个服务都使用同一个接入方案，否则Trace数据可能不完整。

## 接入方案详情

此处分别从接入方式、Trace自动埋点成熟度、接入复杂性等方面列出日志服务所支持的接入方案。接入方案仅列出常用的Trace平台OpenTelemetry、SkyWalking、Jaeger、Zipkin。

-   各个开发语言中的Trace数据接入方案

    您可以通过自动埋点或半自动埋点方式接入Trace数据到日志服务，两者区别如下：

    -   自动埋点：开发者不需要关心所使用的框架、代码等信息，无需改动代码。Trace系统对支持的框架自动埋点。
    -   半自动埋点：开发者需要手动安装依赖包或进行一定的代码改造。
    |语言|接入方案|自动化程度|接入复杂度|
    |--|----|-----|-----|
    |Java|[OpenTelemetry直接发送](/cn.zh-CN/Trace服务/接入Trace数据/新接入方案/通过OpenTelemetry接入Java Trace数据.md)|自动埋点|低|
    |[通过OpenTelemetry Collector转发](/cn.zh-CN/Trace服务/接入Trace数据/集成现有方案/接入OpenTelemetry Trace数据.md)|自动埋点|中|
    |[通过SkyWalking接入](/cn.zh-CN/Trace服务/接入Trace数据/集成现有方案/接入SkyWalking Trace数据.md)|自动埋点|中|
    |Golang|[OpenTelemetry直接发送](/cn.zh-CN/Trace服务/接入Trace数据/新接入方案/通过OpenTelemetry接入Golang Trace数据.md)|半自动埋点|低|
    |[通过OpenTelemetry Collector转发](/cn.zh-CN/Trace服务/接入Trace数据/集成现有方案/接入OpenTelemetry Trace数据.md)|半自动埋点|低|
    |Python|[OpenTelemetry直接发送](/cn.zh-CN/Trace服务/接入Trace数据/新接入方案/通过OpenTelemetry接入Python Trace数据.md)|半自动埋点|中|
    |[通过OpenTelemetry Collector转发](/cn.zh-CN/Trace服务/接入Trace数据/集成现有方案/接入OpenTelemetry Trace数据.md)|半自动埋点|中|
    |NodeJS|[OpenTelemetry直接发送](/cn.zh-CN/Trace服务/接入Trace数据/新接入方案/通过OpenTelemetry接入NodeJS Trace数据.md)|半自动埋点|中|
    |[通过OpenTelemetry Collector转发](/cn.zh-CN/Trace服务/接入Trace数据/集成现有方案/接入OpenTelemetry Trace数据.md)|半自动埋点|中|
    |PHP|[Zipkin直接发送](/cn.zh-CN/Trace服务/接入Trace数据/新接入方案/通过Zipkin接入PHP Trace数据.md)|手动埋点|较高|
    |C++|[Jaeger直接发送](/cn.zh-CN/Trace服务/接入Trace数据/新接入方案/通过Jaeger接入C++ Trace数据.md)|手动埋点|较高|
    |C\#|[OpenTelemetry直接发送](/cn.zh-CN/Trace服务/接入Trace数据/新接入方案/通过OpenTelemetry接入C# Trace数据.md)|半自动埋点|中|
    |[通过OpenTelemetry Collector转发](/cn.zh-CN/Trace服务/接入Trace数据/集成现有方案/接入OpenTelemetry Trace数据.md)|半自动埋点|中|
    |[通过SkyWalking接入](/cn.zh-CN/Trace服务/接入Trace数据/集成现有方案/接入SkyWalking Trace数据.md)|自动埋点|中|
    |Rust|[OpenTelemetry直接发送](/cn.zh-CN/Trace服务/接入Trace数据/新接入方案/通过OpenTelemetry接入Rust Trace数据.md)|手动埋点|较高|
    |[通过OpenTelemetry Collector转发](/cn.zh-CN/Trace服务/接入Trace数据/集成现有方案/接入OpenTelemetry Trace数据.md)|手动埋点|较高|
    |Ruby|[OpenTelemetry直接发送](/cn.zh-CN/Trace服务/接入Trace数据/新接入方案/通过OpenTelemetry接入Ruby Trace数据.md)|手动埋点|较高|
    |[通过OpenTelemetry Collector转发](/cn.zh-CN/Trace服务/接入Trace数据/集成现有方案/接入OpenTelemetry Trace数据.md)|手动埋点|较高|

-   各个平台中的Trace数据接入方案

    |Trace平台|接入方案|接入复杂度|
    |-------|----|-----|
    |OpenTelemetry|[直接发送](/cn.zh-CN/Trace服务/接入Trace数据/集成现有方案/接入OpenTelemetry Trace数据.md)|低|
    |[通过OpenTelemetry Collector转发](/cn.zh-CN/Trace服务/接入Trace数据/集成现有方案/接入OpenTelemetry Trace数据.md)|中|
    |Jaeger|[直接发送](/cn.zh-CN/Trace服务/接入Trace数据/集成现有方案/接入Jaeger Trace数据.md)|低|
    |[通过OpenTelemetry Collector转发](/cn.zh-CN/Trace服务/接入Trace数据/集成现有方案/接入Jaeger Trace数据.md)|中|
    |Zipkin|[直接发送](/cn.zh-CN/Trace服务/接入Trace数据/集成现有方案/接入Zipkin Trace数据.md)|低|
    |[通过OpenTelemetry Collector转发](/cn.zh-CN/Trace服务/接入Trace数据/集成现有方案/接入Zipkin Trace数据.md)|中|
    |SkyWalking|[通过Logtail转发](/cn.zh-CN/Trace服务/接入Trace数据/集成现有方案/接入SkyWalking Trace数据.md)|中|
    |OpenCensus|[通过OpenTelemetry Collector转发](/cn.zh-CN/Trace服务/接入Trace数据/集成现有方案/接入OpenCensus Trace数据.md)|中|
    |AWS X-Ray|[通过OpenTelemetry Collector转发](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/awsxrayreceiver)|高|
    |SignalFX（Splunk）|[通过OpenTelemetry Collector转发](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/signalfxreceiver)|高|


## 常见的Trace接入场景

-   新构建Trace系统

    如果您的系统刚开始接入日志服务Trace，则推荐使用OpenTelemetry直接将Trace数据上传到日志服务。如果对应的语言没有OpenTelemetry接入方案或方案不完善，则使用Jaeger、Zipkin等支持OpenTracing或OpenCensus的接入方案。

-   现有Trace系统升级

    如果您当前的系统已经使用了Trace功能，则根据实际情况选择接入方案。

    -   Trace系统已稳定运行的场景
        -   如果Trace系统支持上传Trace数据到OpenTelemetry Collector，则使用OpenTelemetry Collector转发Trace数据到日志服务。
        -   如果Trace系统使用的是自定义协议或其他非OpenTelemetry、OpenTracing协议，则可以将Trace数据打印到文件并通过Logtail上传到日志服务，并使用日志服务数据加工功能将Trace数据格式转化为OpenTelemetry格式。
    -   Trace系统不完善或需要升级的场景
        -   如果Trace系统使用的是OpenTracing或OpenCensus协议，则可以使用平滑迁移方式。在原系统中，将Trace数据上传至OpenTelemetry Collector并转发到日志服务。期间，逐步替换其中的协议为OpenTelemetry，使用OpenTelemetry直接发送Trace数据到日志服务。
        -   如果Trace系统使用的是其他协议，则需要进行整体替换，否则两者共存期间Trace数据可能不完整。
-   线下Trace系统

    如果您的业务部署在线下IDC中，只有部分网关机有公网、专线，则可以在网关机上部署OpenTelemetry Collector，将其他机器上的Trace数据发送到网关机中，并通过网关机上的OpenTelemetry Collector将Trace数据转发到日志服务。


## Trace Demo

日志服务提供各种语言的接入Demo。更多信息，请参见[Trace Demos](https://github.com/aliyun-sls/sls-mis/tree/main/demos)。

