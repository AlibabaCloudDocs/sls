# 开启SQL独享实例

SQL独享实例是日志服务提供的计费资源，用于SQL分析。当您对大规模（百亿到千亿级）数据有分析需求时，可使用SQL独享实例。

当您在使用SQL分析时，如果数据量较大，日志服务无法在一次查询中完整扫描这个时间段内的所有日志。为了快速返回结果，日志服务限制了每个Shard扫描的数据量，先返回部分不精确的结果。针对该问题，推荐您增加Shard数量，以增加计算资源。但该方法存在如下问题：增加Shard后只对新写入的数据生效，不能解决旧数据的读取问题。另外，增加Shard会影响数据消费，导致消费的客户端过多。

现在，日志服务推出SQL独享实例，用于SQL分析。SQL独享实例为计费资源，支持更强大的SQL分析；SQL普通实例为免费资源，存在更多的资源限制。更多信息，请参见[使用限制](/cn.zh-CN/查询与分析/分析简介.md)。

**说明：**

-   目前，SQL独享实例功能在公测阶段，仅支持西南1（成都）和华南2（河源）。如果您所在地域未支持SQL独享实例，可提交[工单](https://selfservice.console.aliyun.com/ticket/category/sls/today)申请。
-   SQL独享实例按照实际使用的CPU时间付费，公测阶段免费使用。

## 开启SQL独享实例

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在Project列表区域，单击目标Project。

3.  在**日志存储** \> **日志库**页签中，单击目标Logstore。

4.  单击![独享实例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8514051261/p275403.png)图标。

    开启SQL独享实例后，您可以使用SQL独享实例执行查询和分析操作。具体操作，请参见[查询和分析日志](/cn.zh-CN/查询与分析/查询和分析日志.md)。


## 常见问题

-   如何通过API开启SQL独享实例？

    您可以在GetLogs接口中，通过powerSql参数或query参数开启SQL独享实例。更多信息，请参见[GetLogs](/cn.zh-CN/开发指南/API 参考/日志库相关接口/GetLogs.md)。

-   如何获取CPU时间？

    您可以在执行查询和分析操作后，获取CPU时间，如下图所示。

    ![计费](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5701051261/p275450.png)

-   SQL独享实例的费用是否可控？

    日志服务通过SQL独享实例的CPU核数来控制SQL独享实例的费用。您可以在目标Project的概览页面中，配置**SQL独享实例CU数**，如下图所示。

    ![核数](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5701051261/p275481.png)


