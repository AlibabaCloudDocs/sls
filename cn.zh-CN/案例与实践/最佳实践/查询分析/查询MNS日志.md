# 查询MNS日志

阿里云消息服务（MNS）日志推送到日志服务后，可进行实时查询，本文介绍实时查询的常用场景及操作步骤，您也可以通过多个关键字组合方式实现更加复杂的查询。

-   已采集到MNS日志，详情请参见[MNS日志](/cn.zh-CN/数据采集/云产品日志采集/MNS操作日志/使用前须知.md)。
-   已开启并配置索引，详情请参见[开启并配置索引](/cn.zh-CN/查询与分析/开启并配置索引.md)。

MNS日志包括队列消息操作日志和主题消息操作日志，日志内容包含消息生命周期的所有信息，例如时间、客户端、操作等。您可以通过实时查询、实时计算和离线计算三种方式对日志进行分析计算。

-   实时查询：在日志服务控制台上进行实时查询，例如：查询消息轨迹、写入量、删除量等。
-   实时计算：使用Spark、Storm、StreamCompute、Consumer Library等方式对MNS日志进行实时计算。例如：计算某个队列中，Top 10消息的产生者和消费者；计算生产和消费的速度，确认是否均衡；计算某些消费者的处理延时，确认是否存在瓶颈等。
-   离线计算：使用MaxCompute、E-MapReduce、Hive进行长时间跨度的计算，例如：计算最近一周内消息从发布到被消费的平均延迟。

## 查询队列消息的消息轨迹

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在Project列表区域，单击目标Project。

3.  在**日志存储** \> **日志库**页签中，单击目标Logstore。

4.  输入查询语句。

    本案例要查询队列消息的消息轨迹，即输入队列名称和消息ID，格式为$QueueName and $MessageId，例如log and FF973C9C6572630D7F963C527CC5A82C。

5.  在页面右上角，单击**15分钟（相对）**，设置查询的时间范围。

    您可以选择相对时间、整点时间和自定义时间范围。

    **说明：** 查询结果相对于指定的时间范围来说，有1min以内的误差。

6.  单击**查询/分析**。

    查询结果如下所示，记录了某条消息从发送到接收的过程。

    ![查看队列消息的消息轨迹](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0592723061/p32448.jpg)


## 查询队列消息发送量

1.  在目标Logstore的查询分析页面，输入查询语句。

    本案例要查询队列消息发送量，即输入队列名称和发送操作，查询语句格式为$QueueName and \(SendMessage or BatchSendMessage\)，例如log and \(SendMessage or BatchSendMessage\)。

2.  在页面右上角，单击**15分钟（相对）**，设置查询的时间范围。

    您可以选择相对时间、整点时间和自定义时间范围。

    **说明：** 查询结果相对于指定的时间范围来说，有1min以内的误差。

3.  单击**查询/分析**。

    查询结果如下所示，当前查询时段内，生产者向log队列发送了3条队列消息。

    ![查看队列消息写入量](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4652723061/p32449.jpg)


## 查询队列消息消费量

1.  在目标Logstore的查询分析页面，输入查询语句。

    本案例要查询队列消息消费量，即输入队列名称和消费操作，查询语句格式为$QueueName and \(ReceiveMessage or BatchReceiveMessage\)，例如log and \(ReceiveMessage or BatchReceiveMessage\)。

2.  在页面右上角，单击**15分钟（相对）**，设置查询的时间范围。

    您可以选择相对时间、整点时间和自定义时间范围。

    **说明：** 查询结果相对于指定的时间范围来说，有1min以内的误差。

3.  单击**查询/分析**。

    查询结果如下所示，当前查询时段内，log队列中有12条消息被消费。

    ![查看队列消息消费量](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4652723061/p32450.jpg)


## 查询队列消息删除量

1.  在目标Logstore的查询分析页面，输入查询语句。

    本案例要查询队列消息删除量，即输入队列名称和删除操作，查询语句格式为$QueueName and \(DeleteMessage or BatchDeleteMessage\)，例如log and \(DeleteMessage or BatchDeleteMessage\)。

2.  在页面右上角，单击**15分钟（相对）**，设置查询的时间范围。

    您可以选择相对时间、整点时间和自定义时间范围。

    **说明：** 查询结果相对于指定的时间范围来说，有1min以内的误差。

3.  单击**查询/分析**。

    查询结果如下所示，当前查询时段内，61条log队列消息被删除。

    ![查看队列消息删除量](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4652723061/p32451.jpg)


## 查询主题消息的消息轨迹

1.  在目标Logstore的查询分析页面，输入查询语句。

    本案例要查询主题消息的消息轨迹，即输入主题名称和MessageId，查询语句格式为$TopicName and $MessageId，例如logtest and 979628CD657261357FCB3C8A68BFA0E3。

2.  在页面右上角，单击**15分钟（相对）**，设置查询的时间范围。

    您可以选择相对时间、整点时间和自定义时间范围。

    **说明：** 查询结果相对于指定的时间范围来说，有1min以内的误差。

3.  单击**查询/分析**。

    查询结果如下图所示，记录了某条消息从发送到通知的过程。

    ![查看主题消息的消息轨迹](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0592723061/p32452.jpg)


## 查询主题消息发布量

1.  在目标Logstore的查询分析页面，输入查询语句。

    本案例要查询主题消息发布量，即输入主题名称和发布操作，查询语句格式为$TopicName and PublishMessage，例如logtest and PublishMessage。

2.  在页面右上角，单击**15分钟（相对）**，设置查询的时间范围。

    您可以选择相对时间、整点时间和自定义时间范围。

    **说明：** 查询结果相对于指定的时间范围来说，有1min以内的误差。

3.  单击**查询/分析**。

    查询结果如下图所示，当前查询时段内，生产者向logtest主题发布了3条消息。

    ![查看主题消息发布量](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4652723061/p32453.jpg)


## 查询某个客户端消息处理量

1.  在目标Logstore的查询分析页面，输入查询语句。

    本案例要查询某个客户端消息处理量，即输入客户端IP地址，查询语句格式为$ClientIP，例如10.10.10.0。

    如果您要查询某个客户端的某类操作日志，可使用多个关键字组合方式，例如$ClientIP and \(SendMessage or BatchSendMessage\)。

2.  在页面右上角，单击**15分钟（相对）**，设置查询的时间范围。

    您可以选择相对时间、整点时间和自定义时间范围。

    **说明：** 查询结果相对于指定的时间范围来说，有1min以内的误差。

3.  单击**查询/分析**。

    查询结果如下图所示，当前查询时段内，该客户端处理了66条消息。

    ![查看某个客户端消息处理量](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0592723061/p32454.jpg)


