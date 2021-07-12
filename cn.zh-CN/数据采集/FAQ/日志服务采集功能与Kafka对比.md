# 日志服务采集功能与Kafka对比

本文展示了日志服务LogHub与Kafka的功能对比。

日志服务是基于飞天Pangu构建的针对日志平台化服务。日志服务提供各种类型日志的实时采集、存储、分发及实时查询能力。通过标准化的RESTful API提供服务。日志服务LogHub提供公共的日志采集、分发通道，您如果不想搭建、运维Kafka集群，可以使用日志服务LogHub功能。

Kafka是分布式消息系统，由于其高吞吐和水平扩展，被广泛使用于消息的发布和订阅。以开源软件的方式提供，您可以根据需要搭建Kafka集群。

|概念|Kafka|LogHub|
|:-|:----|:-----|
|存储对象|topic|logstore|
|水平分区|partition|shard|
|数据消费位置|offset|cursor|

## LogHub与Kafka功能比较

|功能|Kafka|LogHub|
|:-|:----|:-----|
|使用依赖|自建或共享Kafka集群|阿里云日志服务|
|通信协议|TCP网络|Http（RESTful API），且为80端口|
|访问控制|无|基于云账号的签名认证与访问控制|
|动态扩容|暂无|支持动态Shard个数弹性伸缩（Merge/Split）|
|多租户QoS|暂无|基于Shard的标准化流控|
|数据拷贝数|用户自定义|默认3份拷贝|
|failover/replication|调用工具完成|自动|
|扩容/升级|调用工具完成，影响服务|自动|
|写入模式|round robin/key hash|暂只支持round robin/key hash|
|当前消费位置|保存在Kafka集群的zookeeper|服务端维护|
|保存时间|配置确定|根据需求动态调整|

## 成本对比

请参见[成本优势](/cn.zh-CN/产品简介/竞品对比/成本优势.md)中LogHub部分。

