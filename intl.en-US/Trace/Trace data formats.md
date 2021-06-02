# Trace data formats

This topic describes the trace data formats that are supported by Log Service.

The trace data formats supported by Log Service are compatible with the data formats defined in [OpenTelemetry Trace 1.0](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/trace/api.md#span) format. The trace data that is written over the OpenTelemetry, Jaeger, Zipkin, OpenCensus, and SkyWalking protocols can be automatically mapped to the trace data formats of OpenTelemetry. Other types of trace data can be transformed to the Log Service Trace format.

|Field|Type|Required|Description|Example|
|-----|----|--------|-----------|-------|
|host|String|No|The hostname of the host where the resources reside. The host field is extracted from the host.name field in the resource field.|test-host|
|service|String|Yes|The service name of the resource. The service field is extracted from the service.name field in theresource field.|test-service|
|resource|JSON Object|No|Resource fields other than host and service, such as process.pid, process.runtime.name, and pod.name. For more information, see [Resource Semantic Conventions](https://github.com/open-telemetry/opentelemetry-specification/tree/main/specification/resource/semantic_conventions).|\{"k8s.pod.name":"xxxx", "k8s.pod.namespace":"kube-system"\}|
|otlp.name|String|No|The name of the Trace SDK.|go-sdk|
|otlp.version|String|No|The version of the Trace SDK.|v1.0.0|
|name|String|Yes|The name of the span.|/get/314159|
|kind|String|No|The span type, for example, CLIENT and SERVER. For more information, see [SpanKind](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/trace/api.md#spankind).|SERVER|
|traceID|String|Yes|The ID of the trace, in hexadecimal.|0123456789abcde0123456789abcde|
|spanID|String|Yes|The ID of the span, in hexadecimal.|0123456789abcde|
|parentSpanID|String|Yes|The ID of the parent span, in hexadecimal.|0123456789abcde|
|links|JSON Array|No|Other associated spans. For more information, see [Specifying links](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/trace/api.md#specifying-links).|\[\{"TraceID" : "abc", "SpanId" : "abc", "TraceState" : "", "Attributes" : \{ "k" : "v" \} \}\]|
|logs|JSON Array|No|The associated logs and events. For more information, see [Add Events](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/trace/api.md#add-events).|None|
|traceState|String|No|The tracestate header defined by W3C. For more information, see [W3C Trace Context Specification](https://www.w3.org/TR/trace-context/#tracestate-header).|None|
|start|INT|Yes|The start time. The value is a UNIX timestamp. Unit: microseconds.|1615882567123456|
|end|INT|No|The end time. The time is a UNIX timestamp. Unit: microseconds.|1615882567234567|
|duration|INT|Yes|The latency, which is the difference between the valueof the start parameter and the value of the end parameter. Unit: microseconds.|1020|
|attribute|JSON Object|Yes|The attribute information of the span, such as the URL and status code of HTTP requests. For more information, see [Attribute and Label Naming](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/common/attribute-and-label-naming.md).|\{"custom":"custom","host.hostname":"myhost","my-label":"myapp-type","null-value":"","service.name":"myapp"\}|
|statusCode|String|Yes|The status code. Valid values: OK, ERROR, and UNSET. UNSET and OK are equivalent.|ERROR|
|statusMessage|String|No|The status message.|stack overflow|

