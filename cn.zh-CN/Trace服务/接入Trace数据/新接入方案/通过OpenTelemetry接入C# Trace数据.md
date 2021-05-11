# 通过OpenTelemetry接入C\# Trace数据

本文介绍通过OpenTelemetry .NET SDK将C\#应用的Trace数据接入到日志服务的操作步骤。

-   已创建Trace实例。更多信息，请参见[创建Trace实例](/cn.zh-CN/Trace服务/创建Trace实例.md)。
-   已安装.NET Framework开发环境。

    **说明：** 本文中的接入方式支持除.NET Framework 3.5 SP1外的所有官方所支持的版本。更多信息，请参见[.NET Core](https://dotnet.microsoft.com/download/dotnet-core)和[.NET Framework](https://dotnet.microsoft.com/download/dotnet-framework)。


## 操作步骤

1.  添加依赖包。

    ```
    dotnet add package OpenTelemetry --version 1.1.0-beta2
    dotnet add package OpenTelemetry.Exporter.Console --version 1.1.0-beta2
    dotnet add package OpenTelemetry.Exporter.OpenTelemetryProtocol --version 1.1.0-beta2
    dotnet add package OpenTelemetry.Extensions.Hosting --version 1.0.0-rc4
    dotnet add package OpenTelemetry.Instrumentation.AspNetCore --version 1.0.0-rc4
    dotnet add package Grpc.Core --version 2.36.4
    ```

2.  运行代码。

    如下代码中的变量需根据实际情况替换。关于变量的详细说明，请参见[表 1](#table_h57_p2p_lra)。

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

    |变量|说明|示例|
    |--|--|--|
    |$\{service\}|服务名。根据您的实际场景取值即可。|payment|
    |$\{version\}|服务版本号。建议按照va.b.c格式定义。|v0.1.2|
    |$\{host\}|主机名。|localhost|
    |$\{endpoint\}|接入地址，格式为https://$\{project\}.$\{region-endpoint\}:Port，其中：    -   $\{project\}：日志服务Project名称。
    -   $\{region-endpoint\}：Project访问域名，支持公网和阿里云内网（经典网络、VPC）。更多信息，请参见[服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)。
    -   Port：网络端口，固定为10010。
|https://test-project.cn-hangzhou.log.aliyuncs.com:10010|
    |$\{project\}|日志服务Project名称。|test-project|
    |$\{instance\}|Trace服务实例名称。|instance-traces|
    |$\{access-key-id\}|阿里云账号AccessKey ID。建议您使用只具备日志服务Project写入权限的RAM用户的AccessKey（包括AccessKey ID和AccessKey Secret）。授予RAM用户向指定Project写入数据权限的具体操作，请参见[授权](/cn.zh-CN/开发指南/访问控制RAM/RAM自定义授权场景.md)。如何获取AccessKey的具体操作，请参见[访问密钥](/cn.zh-CN/开发指南/API 参考/访问密钥.md)。

|LTAI4FvyvBaF3DY\*\*\*|
    |$\{access-key-secret\}|阿里云账号AccessKey Secret。建议您使用只具备日志服务Project写入权限的RAM用户的AccessKey。

|HfJEw25sYldO0fx2iu\*\*\*|


-   [查看Trace实例详情](/cn.zh-CN/Trace服务/查看Trace实例详情.md)
-   [查询和分析Trace数据](/cn.zh-CN/Trace服务/查询和分析Trace数据.md)

