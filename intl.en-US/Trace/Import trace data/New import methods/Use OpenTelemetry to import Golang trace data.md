# Use OpenTelemetry to import Golang trace data

This topic describes how to use OpenTelemetry SDK for Golang to import trace data of Golang applications to Log Service.

-   A trace instance is created. For more information, see [Create a trace instance]().
-   The development environment for Golang 1.13 and later is installed.

## Procedure

1.  Initialize OpenTelemetry Provider.
2.  Check whether the conditions for semi-automatic import are met.
    -   If so, you can use the semi-automatic method to import trace data.

        If the semi-automatic method does not apply in some of your business scenarios, you can manually import trace data.

    -   If not, you can manually import trace data.

## Step 1: Initialize OpenTelemetry Provider

Log Service provides [SLS Provider](https://github.com/aliyun-sls/opentelemetry-go-provider-sls) to simplify the use of OpenTelemetry Provider. You can use SLS Provider to build dependencies and upload them to Log Service.

**Note:** You must initialize OpenTelemetry Provider before you create traces and register metrics.

You can run code or configure environment variables to initialize OpenTelemetry Provider.

-   Run code to initialize OpenTelemetry Provider.
    1.  Add dependencies.

        ```
        module opentelemetry-golang-sample
        
        go 1.13
        
        require (
            github.com/aliyun-sls/opentelemetry-go-provider-sls v0.1.0
            go.opentelemetry.io/contrib/instrumentation/host v0.16.0
            go.opentelemetry.io/contrib/instrumentation/runtime v0.16.0
            go.opentelemetry.io/otel v0.16.0
            go.opentelemetry.io/otel/exporters/otlp v0.16.0
            go.opentelemetry.io/otel/exporters/stdout v0.16.0
            go.opentelemetry.io/otel/sdk v0.16.0
        )
        ```

    2.  Write the initialization code.

        Replace the variables in the following code with the actual values. For more information about the variables, see [Table 1](#table_6dg_ce0_eji).

        ```
        package main
        
        import (
            "github.com/aliyun-sls/opentelemetry-go-provider-sls/provider"
        )
        
        func main() {
        
            slsConfig, err := provider.NewConfig(provider.WithServiceName("$\{service\}"),
                provider.WithServiceVersion("$\{version\}"),
                provider.WithTraceExporterEndpoint("$\{endpoint\}"),
                provider.WithMetricExporterEndpoint("$\{endpoint\}"),
                provider.WithSLSConfig("$\{project\}", "$\{instance\}", "$\{access-key-id\}", "$\{access-key-secret\}"))
            // Invoke the panic() function. If the initialization fails, OpenTelemetry Provider exits. You can also use other error handling methods. 
            if err != nil {
                panic(err)
            }
            if err := provider.Start(slsConfig); err != nil {
                panic(err)
            }
            defer provider.Shutdown(slsConfig)
            
            // Add the business logic code. 
            ...
        }$\{project\}
        ```

        |Variable|Description|Example|
        |--------|-----------|-------|
        |$\{service\}|The name of the service. Specify the name based on the actual scenario.|payment|
        |$\{version\}|The version of the service. We recommend that you specify the version in the format of va.b.c.|v0.1.2|
        |$\{endpoint\}|The endpoint that is used to access Log Service. The format is $\{project\}.$\{region-endpoint\}, where:        -   $\{project\}: the name of the Log Service project.
        -   $\{region-endpoint\}: the endpoint that is used to access the project. You can access the project by using the Internet, the classic network, or a virtual private cloud \(VPC\). For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).
        -   Port: the port number, which is set to 10010.
**Note:**

        -   If you set the variable to stdout, the code line is provider.WithTraceExporterEndpoint\("stdout"\). It indicates that trace data is printed as standard outputs.
        -   If you set the variable to an empty value, no trace data is uploaded to Log Service.
|test-project.cn-hangzhou.log.aliyuncs.com:10010|
        |$\{project\}|The name of the project.|test-project|
        |$\{instance\}|The name of the trace instance.|test-traces|
        |$\{access-key-id\}|The AccessKey ID of your Alibaba Cloud account. We recommend that you use an AccessKey pair of a Resource Access Management \(RAM\) user who has only the write permissions on the Log Service project. An AccessKey pair consists of an AccessKey ID and an AccessKey secret. For information about how to grant the write permissions on a specific project to a RAM user, see [Use custom policies to grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md). For more information about how to obtain an AccessKey pair, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).

|None|
        |$\{access-key-secret\}|The AccessKey secret of your Alibaba Cloud account. We recommend that you use an AccessKey pair of a Resource Access Management \(RAM\) user who has only the write permissions on the Log Service project. An AccessKey pair consists of an AccessKey ID and an AccessKey secret.

|None|

-   Configure environment variables to initialize OpenTelemetry Provider.

    |Method|Environment variable|Required|Description|Default value|
    |------|--------------------|--------|-----------|-------------|
    |WithServiceName|SLS\_OTEL\_SERVICE\_NAME|Yes|The name of the service. Specify the name based on the actual scenario.|None|
    |WithServiceVersion|SLS\_OTEL\_SERVICE\_VERSION|Yes|The version of the service. We recommend that you specify the version in the va.b.c format.|v0.1.0|
    |WithSLSConfig|SLS\_OTEL\_PROJECT, SLS\_OTEL\_INSTANCE\_ID, SLS\_OTEL\_ACCESS\_KEY\_ID, SLS\_OTEL\_ACCESS\_KEY\_SECRET|No|The information of the Log Service resource. The information includes the project name, trace instance name, AccessKey ID that has the write-only permissions on the project, and AccessKey secret. For information about how to grant the write permissions on a specific project to a RAM user, see [Use custom policies to grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md). For information about how to obtain an AccessKey pair, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).|None|
    |WithTraceExporterEndpoint|SLS\_OTEL\_TRACE\_ENDPOINT|No|The endpoint that is used to access Log Service. The format is $\{project\}.$\{region-endpoint\}, where:    -   $\{project\}: the name of the Log Service project.
    -   $\{region-endpoint\}: the domain name used to access the project. Internet and Alibaba Cloud internal network are supported. Alibaba Cloud internal network includes the classic network and virtual private network \(VPC\). For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).
    -   Port: the port number, which is set to 10010.
**Note:**

    -   If you set the variable to stdout, trace data is printed as standard outputs.
    -   If you set the variable to an empty value, no trace data is uploaded to Log Service.
|stdout|
    |WithTraceExporterInsecure|SLS\_OTEL\_TRACE\_INSECURE|No|Specifies whether to transfer data by using a method that is not secure.     -   true: uses a method that is not secure.
    -   false: uses a method that is secure.
**Note:** If you want to directly transfer data to Log Service, you must set this variable to false.

|false|
    |WithMetricExporterEndpoint|SLS\_OTEL\_METRIC\_ENDPOINT|No|The endpoint that is used to access Log Service. The format is $\{project\}.$\{region-endpoint\}, where:    -   $\{project\}: the name of the Log Service project.
    -   $\{region-endpoint\}: the endpoint that is used to access the project. You can access the project by using the Internet, the classic network, or a VPC. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).
    -   Port: the port number, which is set to 10010.
**Note:**

    -   If you set the variable to stdout, metric data is printed as standard outputs.
    -   If you set the variable to an empty value, no metric data is uploaded to Log Service.
|stdout|
    |WithMetricExporterInsecure|SLS\_OTEL\_METRIC\_INSECURE|No|Specifies whether to transfer data by using a method that is not secure.     -   true: uses a method that is not secure.
    -   false: uses a method that is secure.
**Note:** If you want to directly transfer data to Log Service, you must set this variable to false.

|false|
    |WithResourceAttributes|None|No|Specify additional tag information, such as the environment and zone.|None|
    |WithResource|OTEL\_RESOURCE\_ATTRIBUTES|No|Specify additional tag information, such as the environment and zone. The format is key1=value1,key2=value2.|None|
    |WithMetricReportingPeriod|SLS\_OTEL\_METRIC\_EXPORT\_PERIOD|No|The interval of the metric output. We recommend that you set the interval to a value from 15s to 60s.|30s|
    |WithErrorHandler|None|No|The function that is invoked to fix errors.|None|
    |WithErrorHandlerFunc|None|None|The function that is invoked to fix errors.|None|
    |None|SLS\_OTEL\_ATTRIBUTES\_ENV\_KEYS|No|Specify additional tag information, such as the environment and zone. This variable is similar to OTEL\_RESOURCE\_ATTRIBUTES. The difference is that the value of Attribute Key defined in the SLS\_OTEL\_ATTRIBUTES\_ENV\_KEYS variable is read from the related environment variables. This variable is commonly used in Kubernetes clusters to pad some template values to environment variables. The format is env-key-1\|env-key-2\|env-key-3.

|None|


## Step 2: Import data

-   Recommended. Use the semi-automatic method to import data.

    OpenTelemetry provides automatic instrumentation solutions for various libraries. If your business depends on these libraries, you can use automatic instrumentation solutions to import data. For more information about instrumentation libraries, see [Golang instrumentation solutions](https://github.com/open-telemetry/opentelemetry-go-contrib/tree/master/instrumentation).

    -   Use the .NET and HTTP frameworks to import data.

        The following code example is created based on go.opentelemetry.io/contrib/instrumentation/net/http/otelhttp v0.16.0. For more information, see [otel-http-example](https://github.com/open-telemetry/opentelemetry-go-contrib/tree/master/instrumentation/net/http/otelhttp/example).

        Replace the variables in the following code with the actual values. For more information about the variables, see [Table 1](#table_6dg_ce0_eji).

        ```
        package main
        
        import (
            "fmt"
            "io"
            "net/http"
            "time"
        
            "github.com/aliyun-sls/opentelemetry-go-provider-sls/provider"
        
            "go.opentelemetry.io/contrib/instrumentation/net/http/otelhttp"
            "go.opentelemetry.io/otel"
            "go.opentelemetry.io/otel/label"
            "go.opentelemetry.io/otel/metric"
            "go.opentelemetry.io/otel/trace"
        )
        
        func main() {
        
            slsConfig, err := provider.NewConfig(provider.WithServiceName("$\{service\}"),
                provider.WithServiceVersion("$\{version\}"),
                provider.WithTraceExporterEndpoint("$\{endpoint\}"),
                provider.WithMetricExporterEndpoint("$\{endpoint\}"),
                provider.WithSLSConfig("$\{project\}", "$\{instance\}", "$\{access-key-id\}", "$\{access-key-secret\}"))
            // Invoke the panic() function. If the initialization fails, OpenTelemetry Provider exits. You can also use other error handling methods. 
            if err != nil {
                panic(err)
            }
            if err := provider.Start(slsConfig); err != nil {
                panic(err)
            }
            defer provider.Shutdown(slsConfig)
        
            // If you want to analyze metric data in the application, you can register the metrics. 
            labels := []label.KeyValue{
                label.String("label1", "value1"),
            }
            meter := otel.Meter("aliyun.sls")
            sayDavidCount := metric.Must(meter).NewInt64Counter("say_david_count")
        
            helloHandler := func(w http.ResponseWriter, req *http.Request) {
                if time.Now().Unix()%10 == 0 {
                    _, _ = io.WriteString(w, "Hello, world!\n")
                } else {
                    // If you want to record some events, you can obtain the span in the context and add events. 
                    ctx := req.Context()
                    span := trace.SpanFromContext(ctx)
                    span.AddEvent("say : Hello, I am david", trace.WithAttributes(label.KeyValue{
                        Key:   "label-key-1",
                        Value: label.StringValue("label-value-1"),
                    }))
        
                    _, _ = io.WriteString(w, "Hello, I am david!\n")
                    sayDavidCount.Add(req.Context(), 1, labels...)
                }
            }
        
            // To use otel net/http, you only need to enclose http.Handler with otelhttp.NewHandler. 
            otelHandler := otelhttp.NewHandler(http.HandlerFunc(helloHandler), "Hello")
        
            http.Handle("/hello", otelHandler)
            fmt.Println("Now listen port 8080, you can visit 127.0.0.1:8080/hello .")
            err = http.ListenAndServe(":8080", nil)
            if err != nil {
                panic(err)
            }
        }
        ```

    -   Use the Gorilla Mux framework to import data.

        The following code example is created based on go.opentelemetry.io/contrib/instrumentation/github.com/gorilla/mux/otelmux v0.16.0. The interface may change in later versions. For information about the latest examples, see [otel-mux-example](https://github.com/open-telemetry/opentelemetry-go-contrib/tree/master/instrumentation/github.com/gorilla/mux/otelmux/example).

        Replace the variables in the following code with the actual values. For more information about the variables, see [Table 1](#table_6dg_ce0_eji).

        ```
        package main
        
        import (
            "context"
            "fmt"
            "net/http"
        
            "github.com/aliyun-sls/opentelemetry-go-provider-sls/provider"
        
            "github.com/gorilla/mux"
            "go.opentelemetry.io/contrib/instrumentation/github.com/gorilla/mux/otelmux"
            "go.opentelemetry.io/otel"
            "go.opentelemetry.io/otel/label"
            "go.opentelemetry.io/otel/metric"
            "go.opentelemetry.io/otel/trace"
        )
        
        func main() {
        
            slsConfig, err := provider.NewConfig(provider.WithServiceName("$\{service\}"),
                provider.WithServiceVersion("$\{version\}"),
                provider.WithTraceExporterEndpoint("$\{endpoint\}"),
                provider.WithMetricExporterEndpoint("$\{endpoint\}"),
                provider.WithSLSConfig("$\{project\}", "$\{instance\}", "$\{access-key-id\}", "$\{access-key-secret\}"))
            // Invoke the panic() function. If the initialization fails, OpenTelemetry Provider exits. You can also use other error handling methods. 
            if err != nil {
                panic(err)
            }
            if err := provider.Start(slsConfig); err != nil {
                panic(err)
            }
            defer provider.Shutdown(slsConfig)
        
            // If you want to analyze metric data in the application, you can register the metrics. 
            labels := []label.KeyValue{
                label.String("label1", "value1"),
            }
            meter := otel.Meter("aliyun.sls")
            callUsersCount := metric.Must(meter).NewInt64Counter("call_users_count")
        
            r := mux.NewRouter()
            r.Use(otelmux.Middleware("my-server"))
            r.HandleFunc("/users/{id:[0-9]+}", http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
                vars := mux.Vars(r)
                id := vars["id"]
                callUsersCount.Add(r.Context(), 1, labels...)
                name := getUser(r.Context(), id)
                reply := fmt.Sprintf("user %s (id %s)\n", name, id)
                _, _ = w.Write(([]byte)(reply))
            }))
            http.Handle("/", r)
            fmt.Println("Now listen port 8080, you can visit 127.0.0.1:8080/users/xxx .")
            _ = http.ListenAndServe(":8080", nil)
        }
        
        func getUser(ctx context.Context, id string) string {
            if id == "123" {
                return "otelmux tester"
            }
            // If you want to record some events, you can obtain the span in the context and add events. 
            span := trace.SpanFromContext(ctx)
            span.AddEvent("unknown user id : "+id, trace.WithAttributes(label.KeyValue{
                Key:   "label-key-1",
                Value: label.StringValue("label-value-1"),
            }))
            return "unknown"
        }
        ```

-   Manually import data

    Replace the variables in the following code with the actual values. For more information about the variables, see [Table 1](#table_6dg_ce0_eji).

    ```
    // Copyright The AliyunSLS Authors
    //
    // Licensed under the Apache License, Version 2.0 (the "License");
    // you may not use this file except in compliance with the License.
    // You may obtain a copy of the License at
    //
    //     http://www.apache.org/licenses/LICENSE-2.0
    //
    // Unless required by applicable law or agreed to in writing, software
    // distributed under the License is distributed on an "AS IS" BASIS,
    // WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    // See the License for the specific language governing permissions and
    // limitations under the License.
    
    package main
    
    import (
        "context"
        "errors"
        "fmt"
        "math/rand"
        "time"
    
        "github.com/aliyun-sls/opentelemetry-go-provider-sls/provider"
    
        "go.opentelemetry.io/otel"
        "go.opentelemetry.io/otel/codes"
        "go.opentelemetry.io/otel/label"
        "go.opentelemetry.io/otel/metric"
        "go.opentelemetry.io/otel/trace"
    )
    
    func main() {
        slsConfig, err := provider.NewConfig(provider.WithServiceName("$\{service\}"),
            provider.WithServiceVersion("$\{version\}"),
            provider.WithTraceExporterEndpoint("$\{endpoint\}"),
            provider.WithMetricExporterEndpoint("$\{endpoint\}"),
            provider.WithSLSConfig("$\{project\}", "$\{instance\}", "$\{access-key-id\}", "$\{access-key-secret\}"))
        // Invoke the panic() function. If the initialization fails, OpenTelemetry Provider exits. You can also use other error handling methods. 
        if err != nil {
            panic(err)
        }
        if err := provider.Start(slsConfig); err != nil {
            panic(err)
        }
        defer provider.Shutdown(slsConfig)
    
        mockTrace()
        mockMetrics()
    }
    
    func mockMetrics() {
        // Add labels. 
        labels := []label.KeyValue{
            label.String("label1", "value1"),
        }
    
        meter := otel.Meter("ex.com/basic")
        // The observed value, which is used to obtain a measured value on a regular basis. The callback function is invoked once per reporting cycle. 
        _ = metric.Must(meter).NewFloat64ValueObserver(
            "randval",
            func(_ context.Context, result metric.Float64ObserverResult) {
                result.Observe(
                    rand.Float64(),
                    labels...,
                )
            },
            metric.WithDescription("A random value"),
        )
    
        temperature := metric.Must(meter).NewFloat64ValueRecorder("temperature")
        interrupts := metric.Must(meter).NewInt64Counter("interrupts")
    
        ctx := context.Background()
    
        for {
            temperature.Record(ctx, 100+10*rand.NormFloat64(), labels...)
            interrupts.Add(ctx, int64(rand.Intn(100)), labels...)
    
            time.Sleep(time.Second * time.Duration(rand.Intn(10)))
        }
    }
    
    func mockTrace() {
    
        tracer := otel.Tracer("ex.com/basic")
    
        ctx0 := context.Background()
    
        ctx1, finish1 := tracer.Start(ctx0, "foo")
        defer finish1.End()
    
        ctx2, finish2 := tracer.Start(ctx1, "bar")
        defer finish2.End()
    
        ctx3, finish3 := tracer.Start(ctx2, "baz")
        defer finish3.End()
    
        ctx := ctx3
        getSpan(ctx)
        addAttribute(ctx)
        addEvent(ctx)
        recordException(ctx)
        createChild(ctx, tracer)
    }
    
    // example of getting the current span
    // Obtain the current span. 
    func getSpan(ctx context.Context) {
        span := trace.SpanFromContext(ctx)
        fmt.Printf("current span: %v\n", span)
    }
    
    // example of adding an attribute to a span
    // Add attribute values to the span. 
    func addAttribute(ctx context.Context) {
        span := trace.SpanFromContext(ctx)
        span.SetAttributes(label.KeyValue{
            Key:   "label-key-1",
            Value: label.StringValue("label-value-1")})
    }
    
    // example of adding an event to a span
    // Add an event to the span. 
    func addEvent(ctx context.Context) {
        span := trace.SpanFromContext(ctx)
        span.AddEvent("event1", trace.WithAttributes(
            label.String("event-attr1", "event-string1"),
            label.Int64("event-attr2", 10)))
    }
    
    // example of recording an exception
    // Record the result of the span and error information. 
    func recordException(ctx context.Context) {
        span := trace.SpanFromContext(ctx)
        span.RecordError(errors.New("exception has occurred"))
        span.SetStatus(codes.Error, "internal error")
    }
    
    // example of creating a child span
    // Create a child span. 
    func createChild(ctx context.Context, tracer trace.Tracer) {
        // span := trace.SpanFromContext(ctx)
        _, childSpan := tracer.Start(ctx, "child")
        defer childSpan.End()
        fmt.Printf("child span: %v\n", childSpan)
    }
    ```


-   [View the details of a trace instance]()
-   [Query and analyze trace data]()

