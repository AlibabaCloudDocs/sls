# 通过OpenTelemetry接入Java Trace数据

本文介绍通过OpenTelemetry Java SDK将Java应用的Trace数据接入到日志服务的操作步骤。

-   已创建Trace实例。更多信息，请参见[创建Trace实例](/cn.zh-CN/Trace服务/创建Trace实例.md)。
-   安装Java 8及以上版本的开发环境（推荐使用JDK 8u252及以上版本）。

## （推荐）方案一：通过Java Agent自动上传Trace数据

目前有数十种Jave框架支持通过Java Agent自动上传Trace数据到日志服务，详细的Jave框架列表请参见[SupportedLibraries](https://github.com/open-telemetry/opentelemetry-java-instrumentation/blob/main/docs/supported-libraries.md)。

**说明：** Java Agent上传方式不支持和其他类似的方案（例如SkyWalking Agent、Zipkin Agent等）同时使用，可能会产生未定义行为。

1.  [下载最新版本Java Agent](https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/latest/download/opentelemetry-javaagent-all.jar)。

2.  集成Java Agent。

    此处以JVM的-javaagent为例，配置相关的环境变量。更多信息，请参见[opentelemetry-java-instrumentation](https://github.com/open-telemetry/opentelemetry-java-instrumentation)。其中，代码中的变量（例如$\{endpoint\}、$\{project\}等）需根据实际情况替换。

    ```
    export OTEL_EXPORTER_OTLP_PROTOCOL=grpc
    export OTEL_EXPORTER_OTLP_ENDPOINT=https://$\{endpoint\}
    export OTEL_EXPORTER_OTLP_COMPRESSION=gzip
    export OTEL_EXPORTER_OTLP_HEADERS=x-sls-otel-project=$\{project\},x-sls-otel-instance-id=$\{instance\},x-sls-otel-ak-id=$\{access-key-id\},x-sls-otel-ak-secret=$\{access-key-secret\}
    java -javaagent:/path/to/opentelemetry-javaagent-all.jar  -Dotel.resource.attributes=service.name=$\{service\},service.version=$\{version\},host.name=$\{host\} -jar /path/to/your/app.jar
    ```

    |变量|说明|示例|
    |--|--|--|
    |$\{endpoint\}|日志服务的接入地址，格式为$\{project\}.$\{region-endpoint\}:Port，其中：    -   $\{project\}：日志服务Project名称。
    -   $\{region-endpoint\}：Project访问域名，支持公网和阿里云内网（经典网络、VPC）。更多信息，请参见[服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)。
    -   Port：网络端口，固定为10010。
|test-project.cn-hangzhou.log.aliyuncs.com:10010|
    |$\{project\}|日志服务Project名称。|test-project|
    |$\{instance\}|Trace服务实例名称。|test-traces|
    |$\{access-key-id\}|阿里云账号AccessKey ID。建议您使用只具备日志服务Project写入权限的RAM用户的AccessKey（包括AccessKey ID和AccessKey Secret）。授予RAM用户向指定Project写入数据权限的具体操作，请参见[授权](/cn.zh-CN/开发指南/访问控制RAM/RAM自定义授权场景.md)。如何获取AccessKey的具体操作，请参见[访问密钥](/cn.zh-CN/开发指南/API 参考/访问密钥.md)。

|无|
    |$\{access-key-secret\}|阿里云账号AccessKey Secret。建议您使用只具备日志服务Project写入权限的RAM用户的AccessKey。

|无|
    |$\{service\}|服务名。根据您的实际场景取值即可。|payment|
    |$\{version\}|服务版本号。建议按照va.b.c格式定义。|v0.1.2|
    |$\{host\}|主机名。|localhost|


## 方案二：手动构造Trace数据并上传

当您使用自建框架或有其他需求时，可以手动构造Trace数据并上传到日志服务。此处以maven方式为例，更多信息，请参见[OpenTelemetry QuickStart](https://github.com/open-telemetry/opentelemetry-java/blob/main/QUICKSTART.md)。

1.  添加maven依赖。

    ```
           <dependency>
                <groupId>io.opentelemetry</groupId>
                <artifactId>opentelemetry-sdk</artifactId>
                <version>0.15.0</version>
            </dependency>
            <dependency>
                <groupId>io.opentelemetry</groupId>
                <artifactId>opentelemetry-exporter-otlp</artifactId>
                <version>0.15.0</version>
            </dependency>
            <dependency>
                <groupId>io.grpc</groupId>
                <artifactId>grpc-netty</artifactId>
                <version>1.31.1</version>
            </dependency>
            <dependency>
                <groupId>io.netty</groupId>
                <artifactId>netty-tcnative-boringssl-static</artifactId>
                <version>2.0.25.Final</version>
            </dependency>
    ```

2.  添加初始化代码。

    代码中的变量（例如$\{endpoint\}、$\{project\}等）需根据实际情况替换。关于变量的详细说明，请参见[表 1](#table_dnh_f2x_mb3)。

    ```
            OtlpGrpcSpanExporter grpcSpanExporter = OtlpGrpcSpanExporter.builder()
                    .setEndpoint("https://$\{endpoint\}")   //配置.setEndpoint参数时，必须添加https://，例如https://test-project.cn-hangzhou.log.aliyuncs.com:10010。
                    .addHeader("x-sls-otel-project", "$\{project\}")
                    .addHeader("x-sls-otel-instance-id", "$\{instance\}")
                    .addHeader("x-sls-otel-ak-id", "$\{access-key-id\}")
                    .addHeader("x-sls-otel-ak-secret", "$\{access-key-secret\}")
                    .build();
    
            SdkTracerProvider tracerProvider = SdkTracerProvider.builder()
                    .addSpanProcessor(BatchSpanProcessor.builder(grpcSpanExporter).build())
                    .setResource(Resource.create(Attributes.builder()
                    .put("service.name", "$\{service\}")
                    .put("service.version", "$\{version\}")
                    .put("host.name", "$\{host\}")
                    .build()))
                    .build();
    
            OpenTelemetry openTelemetry = OpenTelemetrySdk.builder()
                    .setTracerProvider(tracerProvider)
                    .setPropagators(ContextPropagators.create(W3CTraceContextPropagator.getInstance()))
                    .build();
    
            Tracer tracer =
                    openTelemetry.getTracer("instrumentation-library-name", "1.0.0");
            Span parentSpan = tracer.spanBuilder("parent").startSpan();
    
            try {
                Span childSpan = tracer.spanBuilder("child")
                        .setParent(Context.current().with(parentSpan))
                        .startSpan();
                childSpan.setAttribute("test","vllelel");
                // do stuff
                childSpan.end();
            } finally {
               parentSpan.end();
            }
    ```


## 常见问题

当您使用JDK 8u252之前版本时，Java Agent中可能出现Counld not find TLS ALPN provider错误，如何处理？

![Counld not find TLS ALPN provider](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5125685161/p249755.png)

您可以参见如下步骤解决：

1.  [下载引导包](https://repo1.maven.org/maven2/io/netty/netty-tcnative-boringssl-static/2.0.25.Final/netty-tcnative-boringssl-static-2.0.25.Final.jar)。

2.  执行如下命令添加相关的.jar包。

    $\{youpath\}为各个.jar包所在路径，请根据实际情况替换。

    ```
    java -Xbootclasspath/p:${youpath}/netty-tcnative-boringssl-static-2.0.25.Final.jar -javaagent:$\{youpath\}/opentelemetry-javaagent-all.jar -jar $\{youpath\}/demo2-0.0.1-SNAPSHOT.jar
    ```


-   [查看Trace实例详情](/cn.zh-CN/Trace服务/查看Trace实例详情.md)
-   [查询和分析Trace数据](/cn.zh-CN/Trace服务/查询和分析Trace数据.md)

