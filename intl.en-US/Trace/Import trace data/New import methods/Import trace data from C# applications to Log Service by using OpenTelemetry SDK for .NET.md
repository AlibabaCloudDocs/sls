# Import trace data from C\# applications to Log Service by using OpenTelemetry SDK for .NET

This topic describes how to import trace data from C\# applications to Log Service by using OpenTelemetry SDK for .NET.

-   A trace instance is created. For more information, see [Create a trace instance]().
-   .NET Framework development environment is installed.

    **Note:** The access method in this topic supports all official versions, except .NET Framework 3.5 SP1. For more information, see [.NET Core](https://dotnet.microsoft.com/download/dotnet-core) and [.NET Framework](https://dotnet.microsoft.com/download/dotnet-framework).


## Procedure

1.  Add dependencies.

    ```
    dotnet add package OpenTelemetry --version 1.1.0-beta2
    dotnet add package OpenTelemetry.Exporter.Console --version 1.1.0-beta2
    dotnet add package OpenTelemetry.Exporter.OpenTelemetryProtocol --version 1.1.0-beta2
    dotnet add package OpenTelemetry.Extensions.Hosting --version 1.0.0-rc4
    dotnet add package OpenTelemetry.Instrumentation.AspNetCore --version 1.0.0-rc4
    dotnet add package Grpc.Core --version 2.36.4
    ```

2.  Run the code.

    You must replace the variables in the following code based on your actual scenario. For more information about the variables, see [Table 1](#table_h57_p2p_lra).

    ```
    using System;
    using OpenTelemetry;
    using OpenTelemetry.Trace;
    using System.Diagnostics;
    using System.Collections.Generic;
    using OpenTelemetry.Resources;
    using Grpc.Core;
    
    namespace mydemo
    {
        class Program
        {
             private static readonly ActivitySource MyActivitySource = new ActivitySource(
            "MyCompany.MyProduct.MyLibrary");
    
            static void Main(string[] args) {
                using var tracerProvider = Sdk.CreateTracerProviderBuilder()
               .SetSampler(new AlwaysOnSampler())
               .AddSource("MyCompany.MyProduct.MyLibrary")
               .AddOtlpExporter(opt => {
                    opt.Endpoint = new Uri("$\{endpoint\}");
                    opt.Headers = "x-sls-otel-project=$\{project\},x-sls-otel-instance-id=$\{instance\},x-sls-otel-ak-id=$\{access-key-id\},x-sls-otel-ak-secre=$\{access-key-secret\}";
               })
                .SetResourceBuilder(OpenTelemetry.Resources.ResourceBuilder.CreateDefault()
               .AddAttributes(new Dictionary<string, object> { { "service.name", "$\{service\}" },
               {"service.version","$\{version\}"},
               {"service.host","$\{host\}"} }))
               .Build();
    
                using (var activity = MyActivitySource.StartActivity("SayHello"))
                {
                    activity?.SetTag("foo", 1);
                    activity?.SetTag("bar", "Hello, World!");
                    activity?.SetTag("baz", new int[] { 1, 2, 3 });
                }
                Console.WriteLine("Hello World!");
    
            } 
        }
    }
    ```

    |Variable|Description|Example|
    |--------|-----------|-------|
    |$\{service\}|The name of the service. You can set a suitable value based on your actual scenario.|payment|
    |$\{version\}|The version of the service. We recommend that you use the va.b.cformat.|v0.1.2|
    |$\{host\}|The hostname of the server.|localhost|
    |$\{endpoint\}|The endpoint, in the format of https://$\{project\}.$\{region-endpoint\}:Port, where:    -   $\{project\}:the name of the Log Service project.
    -   $\{region-endpoint\}:the domain name used to access the project. Internet and Alibaba Cloud internal network are supported. Alibaba Cloud internal network includes the classic network and virtual private network \(VPC\). For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).
    -   Port:the port number, which is set to 10010.
|https://test-project.cn-hangzhou.log.aliyuncs.com:10010|
    |$\{project\}|The name of the project in Log Service.|test-project|
    |$\{instance\}|The name of the trace instance.|instance-traces|
    |$\{access-key-id\}|The AccessKey ID of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a Resource Access Management \(RAM\) user who has only the write permissions on the Log Service project. An AccessKey pair consists of an AccessKey ID and an AccessKey secret. For more information about how to authorize the write permission to a RAM user for a specific Project, see [Authorize](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md). For more information about how to obtain the AccessKey, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).

|LTAI4FvyvBaF3DY\*\*\*|
    |$\{access-key-secret\}|The AccessKey secret of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a RAM user that has only the write permissions on the Log Service project.

|HfJEw25sYldO0fx2iu\*\*\*|


-   [View the details of a trace instance]()
-   [Query and analyze trace data]()

