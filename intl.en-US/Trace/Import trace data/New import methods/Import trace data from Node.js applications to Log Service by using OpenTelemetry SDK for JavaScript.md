# Import trace data from Node.js applications to Log Service by using OpenTelemetry SDK for JavaScript

This topic describes how to import trace data from Node.js applications to Log Service by using OpenTelemetry SDK for JavaScript.

-   A trace instance is created. For more information, see [Create a trace instance]().
-   A Node.js v8.5.0 or later development environment is installed.

## Method 1 \(recommended\): semi-automatic import

You can add dependencies in Node.js that support HTTP, HTTPS, gRPC, Express, MySQL, MongoDB, and Redis frameworks. This way, trace data can be automatically uploaded to Log Service. For a list of frameworks, see [opentelemetry-node-js-contrib](https://github.com/open-telemetry/opentelemetry-js-contrib). In this example, Express is used to describe the semi-automatic import method. For more information, see [examples](https://github.com/open-telemetry/opentelemetry-js-contrib/tree/main/examples).

1.  Install dependencies.

    ```
    npm install --save @opentelemetry/api
    npm install --save @opentelemetry/node
    npm install --save @opentelemetry/tracing
    npm install --save @opentelemetry/exporter-collector-grpc
    npm install --save @opentelemetry/instrumentation
    npm install --save @opentelemetry/plugin-http
    npm install --save @opentelemetry/plugin-express
    ```

2.  Initialize the tracer and start Express.

    Replace the variables in the following code with the actual values. For more information about the variables, see [Table 1](#table_fmo_w3n_cou).

    ```
    const opentelemetry = require('@opentelemetry/api');
    const {BatchSpanProcessor, ConsoleSpanExporter} = require('@opentelemetry/tracing');
    const { NodeTracerProvider } = require('@opentelemetry/node');
    const { CollectorTraceExporter } =  require('@opentelemetry/exporter-collector-grpc');
    const ExpressLayerType = require('@opentelemetry/plugin-express')
    const grpc = require('grpc');
    
    var os = require("os");
    var hostname = os.hostname();
    
    class AttributeSpanProcessor {
      onStart(span, _context) {
        span.setAttribute("host.name", hostname);
        span.setAttribute("service.version", "$\{version\}");
      }
      onEnd(_span) {}
      shutdown() {
        return Promise.resolve();
      }
      forceFlush() {
        return Promise.resolve();
      }
    }
    
    var url = "$\{endpoint\}"
    
    var logStdout = false
    if (url == "stdout") {
      logStdout = true
    }
    var meta = new grpc.Metadata();
    meta.add('x-sls-otel-project', '$\{project\}');
    meta.add('x-sls-otel-instance-id', '$\{instance\}');
    meta.add('x-sls-otel-ak-id', '$\{access-key-id\}');
    meta.add('x-sls-otel-ak-secret', '$\{access-key-secret\}');
    const collectorOptions = {
      serviceName: "$\{service\}",
      url: url,
      credentials: grpc.credentials.createSsl(),
      metadata: meta
    };
    
    const exporter = new CollectorTraceExporter(collectorOptions);
    const provider = new NodeTracerProvider({
      plugins: {
        http: {
          enabled: true,
          path: '@opentelemetry/plugin-http',
          ignoreIncomingPaths: [new RegExp('.*(png|html)')],
        },
        express: {
          enabled: true,
          // You may use a package name or absolute path to the file.
          path: '@opentelemetry/plugin-express',
          ignoreLayers: [new RegExp('middleware.*')],
          ignoreLayersType: [ExpressLayerType.MIDDLEWARE],
        },
      }
    });
    
    if (!logStdout) {
      provider.addSpanProcessor(new AttributeSpanProcessor())
      provider.addSpanProcessor(new BatchSpanProcessor(exporter));
    } else {
      provider.addSpanProcessor(new AttributeSpanProcessor())
      var stdexporter = new ConsoleSpanExporter();
      provider.addSpanProcessor(new BatchSpanProcessor(stdexporter));
    }
    provider.register();
    var tracer = opentelemetry.trace.getTracer("front-end");
    
    
    var express = require('express');
    var app = express()
    
    app.get('/hello', function (req, res, next) {
      res.send("success");
    });
    
    var server = app.listen(8079, function () {
      var port = server.address().port;
      console.log("App now running in %s mode on port %d", app.get("env"), port);
    });
    ```

    |Variable|Description|Example|
    |--------|-----------|-------|
    |$\{endpoint\}|The endpoint. The format is $\{project\}.$\{region-endpoint\}, where:    -   $\{project\}: the name of the Log Service project.
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

3.  Access the service and then send the trace data to Log Service.

    ```
    127.0.0.1:8079/hello
    ```


## Method 2: Manually construct and send trace data

If you use a self-managed framework or have other requirements, you can manually construct and send trace data to Log Service. For more information, see [opentelemetry-js](https://github.com/open-telemetry/opentelemetry-js).

1.  Install dependencies.

    ```
    npm install --save @opentelemetry/api
    npm install --save @opentelemetry/node
    npm install --save @opentelemetry/tracing
    npm install --save @opentelemetry/exporter-collector-grpc
    ```

2.  Initialize the tracer and start Express.

    Replace the variables in the following code with the actual values. For more information about the variables, see [Table 1](#table_fmo_w3n_cou).

    ```
    const opentelemetry = require('@opentelemetry/api');
    const {BasicTracerProvider, BatchSpanProcessor, ConsoleSpanExporter} = require('@opentelemetry/tracing');
    const { CollectorTraceExporter } =  require('@opentelemetry/exporter-collector-grpc');
    const grpc = require('grpc');
    
    var os = require("os");
    var hostname = os.hostname();
    
    class AttributeSpanProcessor {
      onStart(span, _context) {
        span.setAttribute("host.name", hostname);
        span.setAttribute("service.version", "$\{version\}");
      }
      onEnd(_span) {}
      shutdown() {
        return Promise.resolve();
      }
      forceFlush() {
        return Promise.resolve();
      }
    }
    
    var url = "$\{endpoint\}"
    
    var logStdout = false
    if (url == "stdout") {
      logStdout = true
    }
    var meta = new grpc.Metadata();
    meta.add('x-sls-otel-project', '$\{project\}');
    meta.add('x-sls-otel-instance-id', '$\{instance\}');
    meta.add('x-sls-otel-ak-id', '$\{access-key-id\}');
    meta.add('x-sls-otel-ak-secret', '$\{access-key-secret\}');
    const collectorOptions = {
      serviceName: "$\{service\}",
      url: url, // url is optional and can be omitted - default is localhost:4317
      credentials: grpc.credentials.createSsl(),
      metadata: meta
    };
    
    const exporter = new CollectorTraceExporter(collectorOptions);
    const provider = new BasicTracerProvider();
    
    if (!logStdout) {
      provider.addSpanProcessor(new AttributeSpanProcessor())
      provider.addSpanProcessor(new BatchSpanProcessor(exporter));
    } else {
      provider.addSpanProcessor(new AttributeSpanProcessor())
      var stdexporter = new ConsoleSpanExporter();
      provider.addSpanProcessor(new BatchSpanProcessor(stdexporter));
    }
    provider.register();
    var tracer = opentelemetry.trace.getTracer("front-end");
    
    var express = require('express');
    
    var app = express()
    
    app.get('/hello', function (req, res, next) {
        const span = tracer.startSpan('hello');
        span.setAttribute('name', 'toma');
        span.setAttribute('age', '26');
        span.addEvent('invoking doWork');
    
        res.send("success");
    
        span.end();
    });
    
    var server = app.listen(8079, function () {
      var port = server.address().port;
      console.log("App now running in %s mode on port %d", app.get("env"), port);
    });
    ```

3.  Access the service and then send the trace data to Log Service.

    ```
    127.0.0.1:8079/hello
    ```


-   [View the details of a trace instance]()
-   [Query and analyze trace data]()

