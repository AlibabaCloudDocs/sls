# Import trace data from Python applications to Log Service by using OpenTelemetry SDK for Python

This topic describes how to import trace data from Python applications to Log Service by using OpenTelemetry SDK for Python.

-   A trace instance is created. For more information, see [Create a trace instance]().
-   Python 3.7 or later is installed.
-   OpenTelemetry SDK for Python is installed in a Python development environment.

    If you have not installed OpenTelemetry SDK for Python, run the following commands to install the SDK:

    ```
    pip install opentelemetry-api==1.1.0
    pip install opentelemetry-sdk==1.1.0
    pip install opentelemetry-exporter-otlp==1.1.0
    ```


## Procedure

1.  Initialize an OpenTelemetry provider.
2.  Check whether the required conditions are met to import data by using the semi-automatic import method.
    -   If the required conditions are met, you can import trace data by using the semi-automatic import method.

        If the semi-automatic import method does not meet your requirements, you must manually import the trace data.

    -   If the required conditions are not met, you can import trace data by using the manual instrumentation method.

## Step 1: Initialize an OpenTelemetry provider

You can initialize the OpenTelemetry provider by using the following code. Replace the variables in the code with the actual values. For more information about the variables, see [Table 1](#table_1lj_o0g_tnd).

**Note:** You must initialize the OpenTelemetry provider before you create traces and register metrics.

```
# For Opentelemetry
import socket
from opentelemetry import metrics
from opentelemetry import trace
from opentelemetry.exporter.otlp.metrics_exporter import OTLPMetricsExporter
from opentelemetry.exporter.otlp.trace_exporter import OTLPSpanExporter
from opentelemetry.sdk.metrics import MeterProvider
from opentelemetry.sdk.metrics.export import ConsoleMetricsExporter
from opentelemetry.sdk.metrics.export.controller import PushController
from opentelemetry.sdk.resources import Resource
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor
from opentelemetry.sdk.trace.export import ConsoleSpanExporter
from opentelemetry.sdk.trace.export import SimpleSpanProcessor


class OpenTelemetrySLSProvider(object):

    def __init__(self, service="", version="", endpoint='stdout',
                 project=None, instance=None, access_key_id=None, access_key_secret=None):
        '''
        :param service: Your Application Service Name
        :param version: Your Application Version
        :param endpoint: console or https://sls_endpoint:10010
        :param project: SLS project
        :param instance: SLS OTEL InstanceId
        :param access_key_id: Aliyun AccesskeyId
        :param access_key_secret:  Aliyun AccesskeySecret
        '''
        self.sls_otel_endpoint = endpoint
        self.sls_otel_project = project
        self.sls_otel_akid = access_key_id
        self.sls_otel_aksecret = access_key_secret
        self.sls_otel_instanceid = instance
        self.local_mode = False
        if endpoint == "stdout":
            self.local_mode = True
            self.resource = Resource(attributes={
                "host.name": socket.gethostname(),
                "service.name": service,
                "service.version": version})
        else:
            self.resource = Resource(attributes={
                "host.name": socket.gethostname(),
                "service.name": service,
                "service.version": version,
                "sls.otel.project": self.sls_otel_project,
                "sls.otel.akid": self.sls_otel_akid,
                "sls.otel.aksecret": self.sls_otel_aksecret,
                "sls.otel.instanceid": self.sls_otel_instanceid
            })

    def initTracer(self):
        trace.set_tracer_provider(TracerProvider(resource=self.resource))
        if self.local_mode:
            trace.get_tracer_provider().add_span_processor(SimpleSpanProcessor(ConsoleSpanExporter()))
        else:
            otlp_exporter = OTLPSpanExporter(endpoint=self.sls_otel_endpoint)
            trace.get_tracer_provider().add_span_processor(BatchSpanProcessor(otlp_exporter))

    def initMetric(self, interval=30):
        metrics.set_meter_provider(MeterProvider(resource=self.resource))
        if self.local_mode:
            metric_exporter = ConsoleMetricsExporter()
        else:
            metric_exporter = OTLPMetricsExporter(endpoint=self.sls_otel_endpoint)

        meter = metrics.get_meter(__name__)
        PushController(meter, metric_exporter, interval)
        return meter
        
# debug mode
#sls_ot_provider = OpenTelemetrySLSProvider(service="example", version="v0.1.0")
 
# write to sls
sls_ot_provider = OpenTelemetrySLSProvider(service="$\{service\}", version="$\{version\}",
                                         endpoint='$\{endpoint\}',
                                         project="$\{project\}",
                                         instance="$\{instance\}",
                                         access_key_id="$\{access-key-id\}",
                                         access_key_secret="$\{access-key-secret\}"
                                         )
```

|Variable|Description|Example|
|--------|-----------|-------|
|$\{service\}|The name of the service. Enter a name based on the actual scenario.|payment|
|$\{version\}|The version of the service. We recommend that you specify the version in the va.b.c format.|v0.1.2|
|$\{endpoint\}|The endpoint. The format is $\{project\}.$\{region-endpoint\}, where:-   $\{project\}: the name of the Log Service project.
-   $\{region-endpoint\}: the endpoint of the project. You can access Log Service by using an endpoint of the Internet, the classic network, or a virtual private cloud \(VPC\). For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).
-   Port: the port number, which is set to 10010.

**Note:**

-   If you set the variable to stdout in the endpoint='stdout' format, data is printed as standard outputs.
-   If you set the variable to an empty value, trace data is not uploaded to Log Service.

|https://test-project.cn-hangzhou.log.aliyuncs.com:10010|
|$\{project\}|The name of the Log Service project.|test-project|
|$\{instance\}|The name of the trace instance.|test-traces|
|$\{access-key-id\}|The AccessKey ID of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a Resource Access Management \(RAM\) user who has only the write permissions on the Log Service project. An AccessKey pair consists of an AccessKey ID and an AccessKey secret. For information about how to grant the write permissions on a specific project to a RAM user, see [Use custom policies to grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md). For information about how to obtain an AccessKey pair, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).

|None|
|$\{access-key-secret\}|The AccessKey secret of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a RAM user who has only the write permissions on the Log Service project.

|None|

## Step 2: Import data

You can import trace data to Log Service in a semi-automatic or manual manner. OpenTelemetry SDK for Python provides various types of instrumentation packages, and supports the semi-automatic instrumentation of common frameworks. If you use one of the these [instrumentation packages](https://github.com/open-telemetry/opentelemetry-python-contrib/tree/main/instrumentation), you can import data in a semi-automatic manner.

-   Semi-automatic instrumentation

    In this example, the flask and requests instrumentation packages are used.

    1.  Install the instrumentation packages.

        ```
        pip install opentelemetry-instrumentation-flask
        pip install opentelemetry-instrumentation-requests
        ```

    2.  Run the following code.

        Replace the variables in the following code with the actual values. For more information about the variables, see [Table 1](#table_1lj_o0g_tnd).

        ```
        # flask-example-1.py
        
        # for flask
        import flask
        import requests
        
        # for Opentelemetry instrumentation
        import socket
        from opentelemetry.instrumentation.flask import FlaskInstrumentor
        from opentelemetry.instrumentation.requests import RequestsInstrumentor
        
        # For Opentelemetry
        from opentelemetry import metrics
        from opentelemetry import trace
        from opentelemetry.exporter.otlp.metrics_exporter import OTLPMetricsExporter
        from opentelemetry.exporter.otlp.trace_exporter import OTLPSpanExporter
        from opentelemetry.sdk.metrics import MeterProvider
        from opentelemetry.sdk.metrics.export import ConsoleMetricsExporter
        from opentelemetry.sdk.metrics.export.controller import PushController
        from opentelemetry.sdk.resources import Resource
        from opentelemetry.sdk.trace import TracerProvider
        from opentelemetry.sdk.trace.export import BatchSpanProcessor
        from opentelemetry.sdk.trace.export import ConsoleSpanExporter
        from opentelemetry.sdk.trace.export import SimpleSpanProcessor
        
        
        class OpenTelemetrySLSProvider(object):
        
            def __init__(self, service="", version="", endpoint='stdout',
                         project=None, instance=None, access_key_id=None, access_key_secret=None):
                '''
                :param service: Your Application Service Name
                :param version: Your Application Version
                :param endpoint: console or https://sls_endpoint:10010
                :param project: SLS project
                :param instance: SLS OTEL InstanceId
                :param access_key_id: Aliyun AccesskeyId
                :param access_key_secret:  Aliyun AccesskeySecret
                '''
                self.sls_otel_endpoint = endpoint
                self.sls_otel_project = project
                self.sls_otel_akid = access_key_id
                self.sls_otel_aksecret = access_key_secret
                self.sls_otel_instanceid = instance
                self.local_mode = False
                if endpoint == "stdout":
                    self.local_mode = True
                    self.resource = Resource(attributes={
                        "host.name": socket.gethostname(),
                        "service.name": service,
                        "service.version": version})
                else:
                    self.resource = Resource(attributes={
                        "host.name": socket.gethostname(),
                        "service.name": service,
                        "service.version": version,
                        "sls.otel.project": self.sls_otel_project,
                        "sls.otel.akid": self.sls_otel_akid,
                        "sls.otel.aksecret": self.sls_otel_aksecret,
                        "sls.otel.instanceid": self.sls_otel_instanceid
                    })
        
            def initTracer(self):
                trace.set_tracer_provider(TracerProvider(resource=self.resource))
                if self.local_mode:
                    trace.get_tracer_provider().add_span_processor(SimpleSpanProcessor(ConsoleSpanExporter()))
                else:
                    otlp_exporter = OTLPSpanExporter(endpoint=self.sls_otel_endpoint)
                    trace.get_tracer_provider().add_span_processor(BatchSpanProcessor(otlp_exporter))
        
            def initMetric(self, interval=30):
                metrics.set_meter_provider(MeterProvider(resource=self.resource))
                if self.local_mode:
                    metric_exporter = ConsoleMetricsExporter()
                else:
                    metric_exporter = OTLPMetricsExporter(endpoint=self.sls_otel_endpoint)
        
                meter = metrics.get_meter(__name__)
                PushController(meter, metric_exporter, interval)
                return meter
        
        
        # write to sls
        sls_ot_provider = OpenTelemetrySLSProvider(service="$\{service\}", version="$\{version\}",
                                                 endpoint='$\{endpoint\}',
                                                 project="$\{project\}",
                                                 instance="$\{instance\}",
                                                 access_key_id="$\{access-key-id\}",
                                                 access_key_secret="$\{access-key-secret\}"
                                                 )
        # for console debug
        #sls_ot_provider = OpenTelemetrySLSProvider(service="example", version="v0.1.0")
        
        sls_ot_provider.initTracer()
        sls_ot_provider.initMetric()
        
        # flask init
        app = flask.Flask(__name__)
        
        # instrumentation init
        FlaskInstrumentor().instrument_app(app)
        RequestsInstrumentor().instrument()
        
        @app.route("/")
        def hello():
            tracer = trace.get_tracer(__name__)
            with tracer.start_as_current_span("request_server"):
                requests.get("http://www.taobao.com")
            return "hello"
        
        
        app.run(debug=True, port=5000)
                            
        ```

-   Manual instrumentation

    Run the following code. Replace the variables in the code with the actual values. For more information about the code, see [Table 1](#table_1lj_o0g_tnd).

    ```
    # ot-manual-example.py
    import time
    
    # For Opentelemetry
    import socket
    from opentelemetry import metrics
    from opentelemetry import trace
    from opentelemetry.exporter.otlp.metrics_exporter import OTLPMetricsExporter
    from opentelemetry.exporter.otlp.trace_exporter import OTLPSpanExporter
    from opentelemetry.sdk.metrics import MeterProvider
    from opentelemetry.sdk.metrics.export import ConsoleMetricsExporter
    from opentelemetry.sdk.metrics.export.controller import PushController
    from opentelemetry.sdk.resources import Resource
    from opentelemetry.sdk.trace import TracerProvider
    from opentelemetry.sdk.trace.export import BatchSpanProcessor
    from opentelemetry.sdk.trace.export import ConsoleSpanExporter
    from opentelemetry.sdk.trace.export import SimpleSpanProcessor
    
    
    class OpenTelemetrySLSProvider(object):
    
        def __init__(self, service="", version="", endpoint='stdout',
                     project=None, instance=None, access_key_id=None, access_key_secret=None):
            '''
            :param service: Your Application Service Name
            :param version: Your Application Version
            :param endpoint: console or https://sls_endpoint:10010
            :param project: SLS project
            :param instance: SLS OTEL InstanceId
            :param access_key_id: Aliyun AccesskeyId
            :param access_key_secret:  Aliyun AccesskeySecret
            '''
            self.sls_otel_endpoint = endpoint
            self.sls_otel_project = project
            self.sls_otel_akid = access_key_id
            self.sls_otel_aksecret = access_key_secret
            self.sls_otel_instanceid = instance
            self.local_mode = False
            if endpoint == "stdout":
                self.local_mode = True
                self.resource = Resource(attributes={
                    "host.name": socket.gethostname(),
                    "service.name": service,
                    "service.version": version})
            else:
                self.resource = Resource(attributes={
                    "host.name": socket.gethostname(),
                    "service.name": service,
                    "service.version": version,
                    "sls.otel.project": self.sls_otel_project,
                    "sls.otel.akid": self.sls_otel_akid,
                    "sls.otel.aksecret": self.sls_otel_aksecret,
                    "sls.otel.instanceid": self.sls_otel_instanceid
                })
                
        def initTracer(self):
            trace.set_tracer_provider(TracerProvider(resource=self.resource))
            if self.local_mode:
                trace.get_tracer_provider().add_span_processor(SimpleSpanProcessor(ConsoleSpanExporter()))
            else:
                otlp_exporter = OTLPSpanExporter(endpoint=self.sls_otel_endpoint)
                trace.get_tracer_provider().add_span_processor(BatchSpanProcessor(otlp_exporter))
    
        def initMetric(self, interval=30):
            metrics.set_meter_provider(MeterProvider(resource=self.resource))
            if self.local_mode:
                metric_exporter = ConsoleMetricsExporter()
            else:
                metric_exporter = OTLPMetricsExporter(endpoint=self.sls_otel_endpoint)
    
            meter = metrics.get_meter(__name__)
            PushController(meter, metric_exporter, interval)
            return meter
    
    
    # write to sls
    sls_ot_provider = OpenTelemetrySLSProvider(service="$\{service\}", version="$\{version\}",
                                               endpoint='$\{endpoint\}',
                                               project="$\{project\}",
                                               instance="$\{instance\}",
                                               access_key_id="$\{access-key-id\}",
                                               access_key_secret="$\{access-key-secret\}"
                                               )
    
    # for console debug
    #sls_ot_provider = OpenTelemetrySLSProvider(service="example", version="v0.1.0")
    
    # Trace Example
    sls_ot_provider.initTracer()
    tracer = trace.get_tracer(__name__)
    
    with tracer.start_as_current_span("foo"):
        print("Hello world!")
    
    # Metric Example
    meter = sls_ot_provider.initMetric(1)
    requests_counter = meter.create_counter(
        name="requests",
        description="number of requests",
        unit="1",
        value_type=int,
    )
    
    labels = {"environment": "staging"}
    requests_counter.add(25, labels)
    
    time.sleep(60)
    ```


## FAQ

How do I check whether OpenTelemetry SDK for Python is installed as expected?

The following example is used to describe how to check whether the SDK is installed as expected. Save the code as a tracing.py file. Then, run the tracing.py command. If the command output is returned, the related dependencies of OpenTelemetry SDK for Python are installed.

```
# tracing-example-1.py
from opentelemetry import trace
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import (
    ConsoleSpanExporter,
    SimpleSpanProcessor,
)

trace.set_tracer_provider(TracerProvider())
trace.get_tracer_provider().add_span_processor(
    SimpleSpanProcessor(ConsoleSpanExporter())
)

tracer = trace.get_tracer(__name__)

with tracer.start_as_current_span("foo"):
    print("Hello world!")
```

![python trace](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7546262261/p249923.png)

-   [View the details of a trace instance]()
-   [Query and analyze trace data]()

