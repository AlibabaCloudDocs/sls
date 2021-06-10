# 通过OpenTelemetry接入Python Trace数据

本文介绍通过OpenTelemetry Python SDK将Python应用的Trace数据接入到日志服务的操作步骤。

-   已创建Trace实例。更多信息，请参见[创建Trace实例](/intl.zh-CN/Trace服务/创建Trace实例.md)。
-   已安装Python 3.7及以上版本。
-   已在Python上安装OpenTelemetry Python SDK。

    如果未安装，可使用以下命令进行安装。

    ```
    pip install opentelemetry-api==1.1.0
    pip install opentelemetry-sdk==1.1.0
    pip install opentelemetry-exporter-otlp==1.1.0
    ```


## 接入流程

1.  初始化OpenTelemetry Provider。
2.  判断是否符合半自动接入条件。
    -   如果符合，则您可以使用半自动方式接入Trace数据。

        当半自动方式无法覆盖您的所有场景时，余下场景您需要使用手动方式接入Trace数据。

    -   如果不符合，则您可以使用手动方式接入Trace数据。

## 步骤1：初始化OpenTelemetry Provider

您可以通过如下代码完成OpenTelemetry Provider的初始化。其中，代码中的变量需根据实际情况替换。关于变量的详细说明，请参见[表 1](#table_1lj_o0g_tnd)。

**说明：** 您需要在创建Traces之前，完成OpenTelemetry Provider的初始化。

```
# For Opentelemetry
import socket
from opentelemetry import trace
from opentelemetry.exporter.otlp.trace_exporter import OTLPSpanExporter
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

|变量|说明|示例|
|--|--|--|
|$\{service\}|服务名。根据您的实际场景取值即可。|payment|
|$\{version\}|服务版本号。建议按照va.b.c格式定义。|v0.1.2|
|$\{endpoint\}|接入地址，格式为https://$\{project\}.$\{region-endpoint\}:Port，其中：-   $\{project\}：日志服务Project名称。
-   $\{region-endpoint\}：Project访问域名，支持公网和阿里云内网（经典网络、VPC）。更多信息，请参见[服务入口](/intl.zh-CN/开发指南/API参考/服务入口.md)。
-   Port：网络端口，固定为10010。

**说明：**

-   如果配置为stdout，即endpoint='stdout'，表示将数据打印到标准输出。
-   如果配置为空值，表示不上传Trace数据到日志服务。

|https://test-project.cn-hangzhou.log.aliyuncs.com:10010|
|$\{project\}|日志服务Project名称。|test-project|
|$\{instance\}|Trace服务实例名称。|test-traces|
|$\{access-key-id\}|阿里云账号AccessKey ID。建议您使用只具备日志服务Project写入权限的RAM用户的AccessKey（包括AccessKey ID和AccessKey Secret）。授予RAM用户向指定Project写入数据权限的具体操作，请参见[授权](/intl.zh-CN/开发指南/访问控制RAM/RAM自定义授权场景.md)。如何获取AccessKey的具体操作，请参见[访问密钥](/intl.zh-CN/开发指南/API参考/访问密钥.md)。

|无|
|$\{access-key-secret\}|阿里云账号AccessKey Secret。建议您使用只具备日志服务Project写入权限的RAM用户的AccessKey。

|无|

## 步骤2：接入数据

您可以通过半自动或手动方式接入Trace数据到日志服务。OpenTelemetry Python SDK提供多种类型的instrumentation包，支持常用框架的半自动埋点，如果您使用的是指定的[instrumentation包](https://github.com/open-telemetry/opentelemetry-python-contrib/tree/main/instrumentation)，则支持通过半自动方式接入数据。

-   半自动埋点

    此处以flask和requests为例。

    1.  安装instrumentation包。

        ```
        pip install opentelemetry-instrumentation-flask
        pip install opentelemetry-instrumentation-requests
        ```

    2.  运行代码。

        如下代码中的变量需根据实际情况替换。更多信息，请参见[表 1](#table_1lj_o0g_tnd)。

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
        from opentelemetry import trace
        from opentelemetry.exporter.otlp.trace_exporter import OTLPSpanExporter
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

    3.  访问服务，触发Trace数据生成并发送。

        ```
        127.0.0.1:5000/hello
        ```

-   手动埋点

    运行如下代码，其中代码中的变量需根据实际情况替换。关于代码的详细说明，请参见[表 1](#table_1lj_o0g_tnd)。

    ```
    # ot-manual-example.py
    import time
    
    # For Opentelemetry
    import socket
    from opentelemetry import trace
    from opentelemetry.exporter.otlp.trace_exporter import OTLPSpanExporter
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
    
    
    labels = {"environment": "staging"}
    requests_counter.add(25, labels)
    
    time.sleep(60)
    ```


## 常见问题

如何检查是否正确安装OpenTelemetry SDK？

以如下代码为例，将如下代码保存为tracing.py，执行tracing.py命令，如果正常输出则表示Python OpenTelemetry相关的基础依赖安装正确。

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

![python trace](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2788046161/p249923.png)

-   [查看Trace实例详情](/intl.zh-CN/Trace服务/查看Trace实例详情.md)
-   [查询和分析Trace数据](/intl.zh-CN/Trace服务/查询和分析Trace数据.md)

