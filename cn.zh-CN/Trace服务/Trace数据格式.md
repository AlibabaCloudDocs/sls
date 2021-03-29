# Trace数据格式

本文介绍日志服务Trace数据的格式。

日志服务Trace数据格式完全兼容[OpenTelemetry Trace 1.0](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/trace/api.md#span)格式，通过OpenTelemetry、Jaeger、Zipkin、OpenCensus、SkyWalking等协议写入的Trace数据可自动映射成OpenTelemetry的Trace数据格式。其他类型的Trace数据可通过数据加工转换为日志服务Trace格式。

|字段|类型|是否必选|说明|示例|
|--|--|----|--|--|
|host|String|否|资源所在主机的主机名。提取自resouce字段中的host.name字段。|test-host|
|service|String|是|资源的服务名。提取自resouce字段中的service.name字段。|test-service|
|resource|JSON Object|否|除host、service之外的其他资源字段，例如进程ID、进程名、Pod名等。更多信息，请参见[Resource Semantic Conventions](https://github.com/open-telemetry/opentelemetry-specification/tree/main/specification/resource/semantic_conventions)。|\{"k8s.pod.name":"xxxx", "k8s.pod.namespace":"kube-system"\}|
|otlp.name|String|否|Trace SDK名称。|go-sdk|
|otlp.version|String|否|Trace SDK版本号。|v1.0.0|
|name|String|是|Span名称。|/get/314159|
|kind|String|否|Span类型，例如CLIENT、SERVER等。更多信息，请参见[SpanKind](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/trace/api.md#spankind)。|SERVER|
|traceID|String|是|Trace ID。使用十六进制表示。|0123456789abcde0123456789abcde|
|spanID|String|是|Span ID。使用十六进制表示。|0123456789abcde|
|parentSpanID|String|是|ParentSpan ID。使用十六进制表示。|0123456789abcde|
|links|JSON Array|否|相关联的其他的Span。更多信息，请参见[Specifying links](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/trace/api.md#specifying-links)。|\[\{"TraceID" : "abc", "SpanId" : "abc", "TraceState" : "", "Attributes" : \{ "k" : "v" \} \}\]|
|logs|JSON Array|否|相关联的日志、事件信息。更多信息，请参见[Add Events](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/trace/api.md#add-events)。|无|
|traceState|String|否|W3C定义的Trace State信息。更多信息，请参见[W3C Trace Context specification](https://www.w3.org/TR/trace-context/#tracestate-header)。|无|
|start|INT|是|开始时间。Unix时间戳类型，单位：微秒。|1615882567123456|
|end|INT|否|结束时间。Unix时间戳类型，单位：微秒。|1615882567234567|
|duration|INT|是|延迟时间，start参数与end参数之间的差值。单位：微秒。|1020|
|attribute|JSON Object|否|Span相关的属性信息，例如HTTP请求的URL、状态码等。更多信息，请参见[Attribute and Label Naming](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/common/attribute-and-label-naming.md)。|\{"custom":"custom","host.hostname":"myhost","my-label":"myapp-type","null-value":"","service.name":"myapp"\}|
|statusCode|String|是|状态码。取值为OK、ERROR、UNSET。其中，UNSET与OK同义。|ERROR|
|statusMessage|String|否|状态信息。|stack overflow|

