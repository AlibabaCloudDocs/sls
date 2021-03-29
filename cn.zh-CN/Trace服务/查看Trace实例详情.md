# 查看Trace实例详情

接入Trace数据后，您可以在Trace实例详情页面查看服务详情、Trace数据详情、Trace数据仪表盘等信息。

已接入Trace数据。具体操作，请参见[接入trace数据](/cn.zh-CN/Trace服务/接入Trace数据/概览.md)。

## 查看服务详情

当您接入目标应用的Trace数据后，您可以在服务列表中查看该应用相关的所有服务以及服务的调用信息，包括QPS、错误率、平均延迟、P50延迟等。支持按照时间范围进行过滤。

例如您要监控一个商城系统的运行情况，您可以接入该系统的Trace数据。服务列表中将展示该系统相关的所有服务（user、order、front-end、queue-master、shipping、job-server等）及服务的调用信息。

![SLS Trace-服务列表](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3238746161/p253265.png)

## 查看Trace数据详情

您可以在Trace详情页面中，查看目标Trace数据的详细信息，包括Trace轨迹图、Span数据详情等。更多信息，请参见[查看Trace数据详情]()。

## 查询和分析Trace数据

您可以在Trace分析页面中，设置查询条件过滤Trace数据，还可以根据service等维度进行分组统计。具体操作，请参见[查询和分析Trace数据](/cn.zh-CN/Trace服务/查询和分析Trace数据.md)。

## 查看服务的拓扑关系

服务的拓扑图主要展示各个服务间的依赖关系。您可以在**Trace分析** \> **依赖分析**页签或拓扑查询页面中，查看服务间的拓扑关系。

-   选中目标服务后，单击![聚合](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2004946161/p254203.png)图标，系统将只展示与该服务相依赖的服务。
-   单击目标服务，系统将显示该服务相关的平均延迟、错误率、Span个数等信息。

![拓扑关系](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2004946161/p254141.png)

## 查询日志

日志服务将记录您在接入Trace数据过程中所产生的日志，您可以在日志查询页面中进行查询和分析操作。具体操作，请参见[查询和分析日志](/cn.zh-CN/查询与分析/查询和分析日志.md)。

## 查看仪表盘

日志服务默认提供两个仪表盘，详细说明如下表所示。

|名称|说明|
|--|--|
|接入概览|展示Trace数据的接入信息，包括Trace数量、Span数量、服务数量、方法数据、Agent数量、平均延迟等。|
|数据统计|展示Trace数据的分析结果，包括Span数量、平均延迟、TOP10延迟服务、TOP10请求服务、TOP延迟方法等。|

## 查看Store配置详情

当您创建Trace实例及接入Trace数据后，日志服务默认生成对应的Logstore和Metricstore，用于存储相关数据，例如\{instance\}-traces Logstore、\{instance\}-metrics Metricstore等。更多信息，请参见[资产详情](/cn.zh-CN/Trace服务/使用前须知.md)。

您可以在Store配置区域查看[Logstore](/cn.zh-CN/数据采集/准备工作/管理Logstore.md)或[Metricstore](/cn.zh-CN/时序存储/管理MetricStore.md)的属性信息。

