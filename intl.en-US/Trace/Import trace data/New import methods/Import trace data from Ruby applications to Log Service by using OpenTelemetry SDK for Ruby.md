# Import trace data from Ruby applications to Log Service by using OpenTelemetry SDK for Ruby

This topic describes how to use OpenTelemetry SDK for Ruby to import trace data of Ruby applications to Log Service.

-   A trace instance is created. For more information, see [Create a trace instance]().
-   Ruby 2.0 or later is installed.
-   OpenTelemetry SDK for Ruby is installed.

    You can run the following commands to install the SDK.

    ```
    gem install opentelemetry-api
    gem install opentelemetry-sdk
    gem install opentelemetry-exporter-otlp
    ```


1.  Configure environment variables.

    You must replace the variables in the following code based on your actual scenario. For more information about variables, see [Table 1](#table_7p0_h32_tru).

    `export OTEL_RESOURCE_ATTRIBUTES=sls.otel.project=$\{project\},sls.otel.instanceid=$\{instance\},sls.otel.akid=$\{akid\},sls.otel.aksecret=$\{aksecret\},service.name=$\{service\},service.version=$\{version\},host.name=$\{host\}`

    |Variable|Description|Example|
    |--------|-----------|-------|
    |$\{service\}|The name of the service. You can set the value based on your actual scenario.|payment|
    |$\{version\}|The version of the service. We recommend that you use the va.b.c format.|v0.1.2|
    |$\{project\}|The name of the Log Service project.|test-project|
    |$\{instance\}|The name of the trace instance.|test-traces|
    |$\{akid\}|The AccessKey ID of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a RAM user that has only the write permissions on the Log Service project. An AccessKey pair consists of an AccessKey ID and an AccessKey secret. For more information about how to grant the write permissions on a specific project to a RAM user, see [Authorize](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md). For more information about how to obtain the AccessKey, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).

|None|
    |$\{aksecret\}|The AccessKey secret of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a RAM user who has only the write permissions on the Log Service project.

|None|
    |$\{host\}|The hostname of the server.|localhost|

2.  Set the tracking point.

    You must replace the $\{endpoint\} variable in the following code based on your actual scenario. For more information about variables, see [Table 2](#table_yh4_jdq_a7b). For more import examples, see [opentelemetry-ruby](https://github.com/open-telemetry/opentelemetry-ruby).

    ```
    require 'opentelemetry/sdk'
    require 'opentelemetry-exporter-otlp'
    
    # Configure the sdk with default export and context propagation formats
    # see SDK#configure for customizing the setup
    OpenTelemetry::SDK.configure do |c|
      c.add_span_processor(
        OpenTelemetry::SDK::Trace::Export::BatchSpanProcessor.new(
          OpenTelemetry::Exporter::OTLP::Exporter.new(
            endpoint: 'https://$\{endpoint\}/opentelemetry/v1/traces'
          )
        )
      )
    end
    
    # To start a trace you need to get a Tracer from the TracerProvider
    tracer = OpenTelemetry.tracer_provider.tracer('my_app_or_gem', '0.1.0')
    
    tracer.in_span('foo') do |span|
      # set an attribute
      span.set_attribute('tform', 'osx')
      # add an event
      span.add_event('event in bar')
      # create bar as child of foo
      tracer.in_span('bar') do |child_span|
        # inspect the span
        pp child_span
      end
    end
    
    sleep 10
    ```

    |Variable|Description|Example|
    |--------|-----------|-------|
    |$\{endpoint\}|The endpoint, in the $\{project\}.$\{region-endpoint\} format, where:    -   $\{project\}: the name of the Log Service project.
    -   $\{region-endpoint\}: the domain name used to access the project. Internet and Alibaba Cloud internal network are supported. Alibaba Cloud internal network includes the classic network and virtual private network \(VPC\). For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).
|https://test-project.cn-hangzhou.log.aliyuncs.com|


-   [View the details of a trace instance]()
-   [Query and analyze trace data]()

