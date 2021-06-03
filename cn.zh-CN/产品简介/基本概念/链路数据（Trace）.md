# 链路数据（Trace）

链路数据（Trace）用于记录单次请求范围内的处理信息，其中包括服务调用和处理时长等数据。

一条链路数据对应一条调用链。在广义上，一个调用链代表一个事务或者流程在（分布式）系统中的执行过程。在OpenTracing标准中，调用链是多个Span组成的一个有向无环图（Directed Acyclic Graph，简称DAG），每一个Span代表调用链中被命名并计时的连续性执行片段。

下图是一个分布式调用的例子：客户端发起请求，请求首先到达负载均衡器，接着经过认证服务、计费服务，然后请求资源，最后返回结果。

![分布式调用示例](http://aliware-images.oss-cn-hangzhou.aliyuncs.com/arms/xtrace_dg_distributed_call.png)

