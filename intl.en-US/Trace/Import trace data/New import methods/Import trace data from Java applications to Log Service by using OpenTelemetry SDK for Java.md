# Import trace data from Java applications to Log Service by using OpenTelemetry SDK for Java

This topic describes how to import trace data from Java applications to Log Service by using OpenTelemetry SDK for Java.

-   A trace instance is created. For more information, see [Create a trace instance]().
-   A Java 8 development environment is installed. We recommend that you use Java Development Kit \(JDK\) V8u252 or later.

## Method 1 \(recommended\): Use a Java agent to automatically upload trace data

A Java agent can be used to automatically upload trace data to Log Service in dozens of Java frameworks. For a list of Java frameworks, see [Supported libraries, frameworks, application servers, and JVMs](https://github.com/open-telemetry/opentelemetry-java-instrumentation/blob/main/docs/supported-libraries.md).

**Note:** A Java agent cannot be used together with a SkyWalking agent or Zipkin agent. Otherwise, undefined behavior may occur.

1.  Click [here](https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/latest/download/opentelemetry-javaagent-all.jar) to download the latest version of the Java agent.

2.  Configure the Java agent.

    In this example, the environment variables in the -javaagent parameter of Java Virtual Machine \(JVM\) are specified. For more information, see [opentelemetry-java-instrumentation](https://github.com/open-telemetry/opentelemetry-java-instrumentation). You must replace the variables such as $\{endpoint\} and $\{project\} in the following code with the actual values.

    ```
    export OTEL_EXPORTER_OTLP_PROTOCOL=grpc
    export OTEL_EXPORTER_OTLP_ENDPOINT=https://$\{endpoint\}
    export OTEL_EXPORTER_OTLP_COMPRESSION=gzip
    export OTEL_EXPORTER_OTLP_HEADERS=x-sls-otel-project=$\{project\},x-sls-otel-instance-id=$\{instance\},x-sls-otel-ak-id=$\{access-key-id\},x-sls-otel-ak-secret=$\{access-key-secret\}
    java -javaagent:/path/to/opentelemetry-javaagent-all.jar  -Dotel.resource.attributes=service.name=$\{service\},service.version=$\{version\},host.name=$\{host\} -jar /path/to/your/app.jar
    ```

    |Variable|Description|Example|
    |--------|-----------|-------|
    |$\{endpoint\}|The endpoint of Log Service. The format is $\{project\}.$\{region-endpoint\}, where:    -   $\{project\}: the name of the Log Service project.
    -   $\{region-endpoint\}: the endpoint of the project. You can access Log Service by using an endpoint of the Internet, the classic network, or a virtual private cloud \(VPC\). For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).
    -   Port: the port number, which is set to 10010.
|test-project.cn-hangzhou.log.aliyuncs.com:10010|
    |$\{project\}|The name of the Log Service project.|test-project|
    |$\{instance\}|The name of the trace instance.|test-traces|
    |$\{access-key-id\}|The AccessKey ID of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a Resource Access Management \(RAM\) user who has only the write permissions on the Log Service project. An AccessKey pair consists of an AccessKey ID and an AccessKey secret. For information about how to grant the write permissions on a specific project to a RAM user, see [Use custom policies to grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md). For information about how to obtain an AccessKey pair, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).

|None|
    |$\{access-key-secret\}|The AccessKey secret of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a RAM user who has only the write permissions on the Log Service project.

|None|
    |$\{service\}|The name of the service. Enter a name based on the actual scenario.|payment|
    |$\{version\}|The version of the service. We recommend that you specify the version in the va.b.c format.|v0.1.2|
    |$\{host\}|The hostname of the server.|localhost|


## Method 2: Manually construct and upload trace data

If you use a self-managed framework or have other requirements, you can manually construct and upload trace data to Log Service. In this example, Maven is used to construct trace data. For more information, see [OpenTelemetry QuickStart](https://github.com/open-telemetry/opentelemetry-java/blob/main/QUICKSTART.md).

1.  Add Maven dependencies.

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

2.  Add initialization code.

    You must replace the variables such as $\{endpoint\} and $\{project\} in the following code with the actual values. For more information about the variables, see [Table 1](#table_dnh_f2x_mb3).

    ```
            OtlpGrpcSpanExporter grpcSpanExporter = OtlpGrpcSpanExporter.builder()
                    .setEndpoint("https://$\{endpoint\}")   // You must add https:// to the beginning of the .setEndpoint parameter, for example, https://test-project.cn-hangzhou.log.aliyuncs.com:10010. 
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


## FAQ

What can I do if the "Could not find TLS ALPN provider" error message is returned by the Java agent on a JDK whose version is earlier than 8u252?

![Could not find TLS ALPN provider](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6346262261/p249755.png)

To resolve this issue, perform the following steps:

1.  Click [here](https://repo1.maven.org/maven2/io/netty/netty-tcnative-boringssl-static/2.0.25.Final/netty-tcnative-boringssl-static-2.0.25.Final.jar) to download the boot package.

2.  Run the following command to add the related JAR files.

    $\{youpath\} is the path where each JAR file resides. Replace each $\{youpath\} variable with the actual value.

    ```
    java -Xbootclasspath/p:${youpath}/netty-tcnative-boringssl-static-2.0.25.Final.jar -javaagent:$\{youpath\}/opentelemetry-javaagent-all.jar -jar $\{youpath\}/demo2-0.0.1-SNAPSHOT.jar
    ```


-   [View the details of a trace instance]()
-   [Query and analyze trace data]()

