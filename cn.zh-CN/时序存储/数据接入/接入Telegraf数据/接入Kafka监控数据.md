# 接入Kafka监控数据

您可使用Telegraf采集Kafka监控数据，再通过日志服务Logtail将Telegraf数据上传到MetricStore中，搭建Kafka可视化监控方案。本文介绍如何通过日志服务来完成Kafka监控数据的采集和可视化。

-   已在服务器上安装Linux Logtail 0.16.48或以上版本。更多信息，请参见[安装Logtail（Linux系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)。
-   已在服务器上安装Java 1.6或以上版本 。

## 步骤1：配置JavaAgent

在采集Kafka监控数据前，您需要先将JMX协议转换为HTTP协议。日志服务支持使用[Jolokia](https://jolokia.org/)将JMX协议转换为HTTP协议。您可以按照Jolokia官方文档下载及加载Jolokia，也可以使用日志服务Logtail自带的Jolokia JavaAgent。Logtail自带的Jolokia JavaAgent位于`/etc/logtail/telegraf/javaagent/jolokia-jvm.jar`中。

您需要在kafka所在机器上设置`KAFKA_JVM_PERFORMANCE_OPTS`环境变量，例如`export KAFKA_JVM_PERFORMANCE_OPTS=-javaagent:/etc/logtail/telegraf/javaagent/jolokia-jvm.jar=port=7777`，其中7777为指定的端口号，用于Logtail配置。

**说明：** 默认Jolokia JavaAgent只在127.0.0.1上监听，即只允许本机请求。如果您的Logtail和被监控的应用不在相同的机器上，您可以在添加的脚本中补充host=字段，使其可监听其他IP地址。如果设置为host=0.0.0.0，则表示监听所有IP地址。相关命令如下所示：

```
-javaagent:/tmp/jolokia-jvm.jar=port=7777,host=0.0.0.0
```

设置完成后，需重启应用。如果您暂时无法重启应用，可使用如下命令将Jolokia JavaAgent连接到指定的Java进程，实现实时生效。其中进程PID请根据实际值替换。

**说明：** 该操作仅用于测试，请确保按照上述操作完成配置，否则重启后将失效。

```
java -jar /etc/ilogtail/telegraf/javaagent/jolokia-jvm.jar --port 7777 start 进程PID
```

如果返回如下信息则表示连接成功。

```
Jolokia is already attached to PID 752
http://127.0.0.1:7777/jolokia/
```

连接成功后，您可以访问该URL，验证连接是否正常。

```
curl http://127.0.0.1:7777/jolokia/
# 返回参考
{"request":{"type":"version"},"value":{"agent":"1.6.2","protocol":"7.2","config":{"listenForHttpService":"true","maxCollectionSize":"0","authIgnoreCerts":"false","agentId":"30.43.124.186-752-5b091b5d-jvm","debug":"false","agentType":"jvm","policyLocation":"classpath:\/jolokia-access.xml","agentContext":"\/jolokia","serializeException":"false","mimeType":"text\/plain","maxDepth":"15","authMode":"basic","authMatch":"any","discoveryEnabled":"true","streaming":"true","canonicalNaming":"true","historyMaxEntries":"10","allowErrorDetails":"true","allowDnsReverseLookup":"true","realm":"jolokia","includeStackTrace":"true","maxObjects":"0","useRestrictorService":"false","debugMaxEntries":"100"},"info":{"product":"tomcat","vendor":"Apache","version":"8.5.57"}},"timestamp":1602663330,"status":200}⏎
```

## 步骤2：创建Logtail采集配置

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在接入数据区域，选择**Kafka监控**。

3.  在选择日志空间页签中，选择目标Project和MetricStore，单击**下一步**。

    您也可以单击**立即创建**，重新创建Project和MetricStore。更多信息，请参见[创建Project](/cn.zh-CN/数据采集/准备工作/管理Project.md)和[创建MetricStore](/cn.zh-CN/时序存储/管理MetricStore.md)。

4.  在创建机器组页签中，创建机器组。

    -   如果您已有可用的机器组，请单击**使用现有机器组**。
    -   如果您还没有可用的机器组，请执行以下操作（以ECS为例）：
        1.  选择ECS实例安装Logtail。更多信息，请参见[安装Logtail（ECS实例）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（ECS实例）.md)。

            如果已在ECS上安装Logtail，请单击**确认安装完毕**。

            **说明：** 如果是自建集群、其他云厂商服务器，需要手动安装Logtail。更多信息，请参见[安装Logtail（Linux系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md#)。

        2.  安装完成后，单击**确认安装完毕**。
        3.  创建机器组。

            如何创建机器组，请参见[创建IP地址机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建IP地址机器组.md)或[创建用户自定义标识机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建用户自定义标识机器组.md)。

5.  在机器组配置页签中，应用机器组。

    选择一个机器组，将该机器组从**源机器组**移动到**应用机器组**。

6.  在数据源设置页签中，配置如下参数。

    |参数名称|说明|
    |----|--|
    |配置名称|Logtail配置名称。|
    |集群名称|Kafka集群名称。配置该参数后，日志服务会为您的数据添加cluster=集群名称的标签。**说明：** 请确保该集群名称唯一，否则可能出现数据冲突。 |
    |服务器列表|单击**+**，添加Kafka服务器信息。    -   **地址**：Kafka服务器的连接地址。
    -   **端口**：配置为您在[步骤1：配置JavaAgent](#section_uj1_fpu_0rx)中配置的端口号。
您可以根据业务需求，添加多台Kafka服务器信息。 |
    |自定义标签|一个MetricStore下可创建多个Logtail配置，您可以使用**自定义标签**为通过该Logtail配置采集到的数据添加标签。单击**+**，添加自定义标签，支持添加多个标签。添加的标签将加入到每一条数据中。 |


## 常见问题

如何查看Telegraf采集是否正常？

您可以在服务器上查看/etc/ilogtail/telegraf/telegraf.log文件中记录的日志进行判断，还可以将该日志采集到日志服务中进行查询。

-   查询分析

    配置完成后，Telegraf将采集到的监控数据通过Logtail上传到日志服务MetricStore中。您可以在MetricStore查询分析页面进行查询分析操作，详情请参见[查询分析时序数据](/cn.zh-CN/时序存储/查询与分析/查询分析时序数据.md)。

-   可视化

    配置完成后，日志服务自动在对应Project中生成名为kafka监控\_集群名称的仪表盘，您可以直接使用该仪表盘，还可以进行告警设置等操作。


