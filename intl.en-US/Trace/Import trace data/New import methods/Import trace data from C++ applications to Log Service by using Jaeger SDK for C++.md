# Import trace data from C++ applications to Log Service by using Jaeger SDK for C++

This topic describes how to import trace data from C++ applications to Log Service by using Jaeger SDK for C++.

-   A trace instance is created. For more information, see [Create a trace instance]().
-   A development environment in which Jaeger SDK for C++ can be compiled and run is prepared.
    -   If you use the CMake Editor, the version must be 3.0 or later.
    -   If you use the GCC or g++ compiler, the version must be 4.9.0 or later.

1.  Download and compile the SDK.

    1.  Click [here](https://github.com/jaegertracing/jaeger-client-cpp/archive/v0.7.0.tar.gz) to download Jaeger SDK for C++.

    2.  Decompress the package to the specified path.

    3.  Go to the specified path after the package is decompressed, and run the following commands:

        ```
        mkdir build
        cd build
        cmake ..
        make
        ```

2.  Compile and run the code.

    1.  Modify the App.cpp file of the examples directory.

        Replace the content of the App.cpp file with the following content. The following content indicates that Jaeger is initialized by using environment variables. For more information, see [jaeger-client-cpp](https://github.com/jaegertracing/jaeger-client-cpp).

        ```
        #include <iostream>
        #include <jaegertracing/Tracer.h>
        #include <jaegertracing/utils/EnvVariable.h>
        
        namespace {
        
        void setUpTracer()
        {
            const auto serviceName = jaegertracing::utils::EnvVariable::getStringVariable("JAEGER_SERVICE_NAME");
            auto config = jaegertracing::Config();
            config.fromEnv();
            auto tracer = jaegertracing::Tracer::make(
                serviceName, config, jaegertracing::logging::consoleLogger());
            opentracing::Tracer::InitGlobal(
                std::static_pointer_cast<opentracing::Tracer>(tracer));
        }
        
        void tracedSubroutine(const std::unique_ptr<opentracing::Span>& parentSpan)
        {
            auto span = opentracing::Tracer::Global()->StartSpan(
                "tracedSubroutine", { opentracing::ChildOf(&parentSpan->context()) });
        }
        
        void tracedFunction()
        {
            auto span = opentracing::Tracer::Global()->StartSpan("tracedFunction");
            tracedSubroutine(span);
        }
        
        }  // anonymous namespace
        
        int main(int argc, char* argv[])
        {
            setUpTracer();
            tracedFunction();
            // Not stricly necessary to close tracer, but might flush any buffered
            // spans. See more details in opentracing::Tracer::Close() documentation.
            opentracing::Tracer::Global()->Close();
            return 0;
        }
        ```

    2.  Go to the build directory.

    3.  Run the `make` command to build an application.

    4.  Run the following code.

        You must replace the variables in the following code with the actual values. The following table describes the variables.

        ```
        # If you want to print spans, set the required environment variable in the format of export JAEGER_REPORTER_LOG_SPANS=true. 
        export JAEGER_SAMPLER_TYPE=const
        export JAEGER_SAMPLER_PARAM=1
        export JAEGER_SERVICE_NAME=${service}
        export JAEGER_PROPAGATION=w3c
        export JAEGER_ENDPOINT="https://$\{endpoint\}/api/traces"
        export JAEGER_TAGS=sls.otel.project=$\{project\},sls.otel.instanceid=$\{instance\},sls.otel.akid=$\{access-key-id\},sls.otel.aksecret=$\{access-key-secret\},service.version=$\{version\}
        ./app
        ```

        |Variable|Description|Example|
        |--------|-----------|-------|
        |$\{service\}|The name of the service. Enter a name based on the actual scenario.|payment|
        |$\{version\}|The version of the service. We recommend that you specify the version in the va.b.c format.|v0.1.2|
        |$\{endpoint\}|The endpoint. The format is $\{project\}.$\{region-endpoint\}, where:        -   $\{project\}: the name of the Log Service project.
        -   $\{region-endpoint\}: the endpoint of the project. You can access Log Service by using an endpoint of the Internet, the classic network, or a virtual private cloud \(VPC\). For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).
        -   Port: the port number, which is set to 10010.
**Note:**

        -   If you set the variable to stdout in the provider.WithTraceExporterEndpoint\("stdout"\), format, data is printed as standard outputs.
        -   If you set the variable to an empty value, trace data is not uploaded to Log Service.
|test-project.cn-hangzhou.log.aliyuncs.com:10010|
        |$\{project\}|The name of the Log Service project.|test-project|
        |$\{instance\}|The name of the trace instance.|test-traces|
        |$\{access-key-id\}|The AccessKey ID of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a Resource Access Management \(RAM\) user who has only the write permissions on the Log Service project. An AccessKey pair consists of an AccessKey ID and an AccessKey secret. For information about how to grant the write permissions on a specific project to a RAM user, see [Use custom policies to grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md). For information about how to obtain an AccessKey pair, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).

|None|
        |$\{access-key-secret\}|The AccessKey secret of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a RAM user who has only the write permissions on the Log Service project.

|None|


-   [View the details of a trace instance]()
-   [Query and analyze trace data]()

