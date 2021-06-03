# 日志主题（Topic）

日志主题（Topic）是日志服务的基础管理单元。您可在采集日志时指定Topic，用于区分日志。

Topic可用于区分不同服务、用户、实例等产生的日志。例如系统A由前端HTTP请求处理模块、缓存模块、逻辑处理模块和存储模块组成，您可设置前端HTTP请求处理模块日志的Topic为http\_module，缓存模块日志的Topic为cache\_module、逻辑处理模块日志的Topic为logic\_module和存储模块日志的Topic为store\_module。当日志被采集到的同一个Logstore中后，您可通过Topic进行区分。

如果不需要区分Logstore中的日志，则在采集日志时设置Topic为空-不生成Topic即可。空字符串是一个有效的Topic，即Topic的值为空字符串。

Logstore、Topic、Shard之间的关系如下：

![日志主题](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8632565851/p2389.png)

