# 时序库（MetricStore）

时序库（MetricStore）是日志服务中时序数据的采集、存储和查询单元。

每个MetricStore隶属于一个Project，每个Project中可创建多个MetricStore。您可以根据实际需求为某个项目创建多个MetricStore，一般是为不同类型的时序数据创建不同的MetricStore。例如您需要采集基础主机监控数据、云服务监控数据、业务应用监控数据，您可以创建一个名为demo-monitor的Project，然后在该Project下创建名为host-metrics、cloud-service-metrics和app-metrics的MetricStore，用于分类存储基础主机监控数据、云服务监控数据和业务应用监控数据。

您在执行写入、查询和分析、消费时序数据时，都需要指定MetricStore。具体说明如下：

-   以MetricStore为采集单元，采集时序数据。
-   以MetricStore为存储单元，存储时序数据以及执行消费操作。
-   查询和分析时序数据，支持SQL92语法和PromQL语法。

