# Trace

You can use a trace to record the processing information of a request, such as service calls and processing duration.

A trace corresponds to a call chain. Generally, a call chain represents the execution process of an incident or the process in a distributed system. In the OpenTracing standard, a call chain is a directed acyclic graph \(DAG\) that consists of multiple spans. Each span represents a named and timed segment that is continuously run in the trace.

The following figure shows an example of a distributed call. When the client initiates a request, the request is sent to the load balancer, processed by the authentication service and billing service, and then sent to the requested resources. Then, a result is returned to the client.

![Example of a distributed call](http://aliware-images.oss-cn-hangzhou.aliyuncs.com/arms/xtrace_dg_distributed_call.png)

