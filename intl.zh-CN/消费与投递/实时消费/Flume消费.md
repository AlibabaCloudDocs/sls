# Flume消费

日志服务支持通过aliyun-log-flume插件与Flume进行对接，实现日志数据的写入和消费。

aliyun-log-flume是一个实现日志服务与Flume对接的插件，与Flume对接后，日志服务可以通过Flume与其它数据系统如HDFS、Kafka等对接。aliyun-log-flume提供Sink和Source实现日志服务与Flume的对接。

-   Sink：Flume读取其他数据源的数据然后写入日志服务。
-   Source：Flume消费日志服务的日志数据然后写入其他系统。

更多关于aliyun-log-flume插件的信息请参见[Github](https://github.com/aliyun/aliyun-log-flume)。

## 操作步骤

1.  下载并安装Flume，详情请参见[Flume](http://flume.apache.org/download.html)。

2.  下载aliyun-log-flume插件，并将插件存放于cd/\*\*\*/flume/lib目录下。下载地址请参见[aliyun-log-flume-1.3.jar](https://github.com/aliyun/aliyun-log-flume/releases/download/1.3/aliyun-log-flume-1.3.jar)。

3.  在cd/\*\*\*/flume/conf目录下，创建配置文件flumejob.conf。

    -   Sink配置及示例请参见[Sink](#section_wsy_7qg_7zx)。
    -   Source配置及示例请参见[Source](#section_wcf_ixl_8zq)。
4.  启动Flume。


## Sink

通过Sink将其他数据源的数据通过Flume写入日志服务。目前支持两种解析格式：

-   SIMPLE：将整个Flume Event作为一个字段写入日志服务。
-   DELIMITED：将整个Flume Event作为被分隔符分隔的数据，根据配置的列名解析成对应的字段写入日志服务。

Sink的配置如下：

|参数|是否必须|说明|
|--|----|--|
|type|是|默认配置为com.aliyun.Loghub.flume.sink.LoghubSink。|
|endpoint|是|Project所属日志服务地域的服务入口，详情请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。|
|project|是|Project名称。|
|logstore|是|Logstore名称。|
|accessKeyId|是|阿里云访问密钥AccessKeyId。|
|accessKey|是|阿里云访问密钥AccessKey。|
|batchSize|否|每批次写入日志服务的数据条数。默认为1000条。|
|maxBufferSize|否|缓存队列的大小。默认为1000条。|
|serializer|否|Event序列化格式。支持的模式如下： -   **DELIMITED**：设置解析格式为分隔符模式。
-   **SIMPLE**：设置解析格式为单行模式。默认为该模式。
-   自定义**serializer**：设置解析格式为自定义的序列化模式，设置为该模式时需要填写完整列名称。 |
|columns|否|当serializer为**DELIMITED**时，必须指定该字段列表，用英文逗号（,）分隔，顺序与实际数据中的字段顺序一致。|
|separatorChar|否|当serializer为**DELIMITED**时，用于指定数据的分隔符，必须为单个字符。默认为英文逗号（,）。|
|quoteChar|否|当serializer为**DELIMITED**时，用于指定Quote字符。默认为英文引号（"）。|
|escapeChar|否|当serializer为**DELIMITED**时，用于指定转义字符。默认为英文引号（"）。|
|useRecordTime|否|用于设置是否使用数据中的timestamp字段作为日志时间。默认为false表示使用当前时间。|

Sink配置示例请参见[GitHub](https://github.com/aliyun/aliyun-log-flume/blob/master/src/test/resources/sink-example.conf)。

## Source

通过Source将日志服务的日志数据通过Flume投递到其他的数据源。目前支持两种输出格式。

-   DELIMITED：数据以分隔符日志的形式写入Flume。
-   JSON：数据以JSON日志的形式写入Flume。

Source配置如下：

|参数|是否必须|说明|
|--|----|--|
|type|是|默认配置为com.aliyun.Loghub.flume.source.LoghubSource。|
|endpoint|是|Project所属日志服务Region的服务入口，详情请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)|
|project|是|Project名称。|
|logstore|是|Logstore名称。|
|accessKeyId|是|阿里云访问密钥AccessKeyId。|
|accessKey|是|阿里云访问密钥AccessKey。|
|heartbeatIntervalMs|否|客户端和日志服务的心跳间隔，默认为30000毫秒。|
|fetchIntervalMs|否|数据拉取间隔，默认为100毫秒。|
|fetchInOrder|否|是否按顺序消费。默认为false。|
|batchSize|否|每批次读取的数据条数，默认为100条。|
|consumerGroup|否|读取的消费组名称。|
|initialPosition|否|读取数据的起点位置，支持**begin**，**end**，**timestamp**。默认为**begin**。 **说明：** 如果服务端已经存在Checkpoint，会优先使用服务端的Checkpoint。 |
|timestamp|否|当initialPosition为**timestamp**时，必须指定时间戳，为Unix时间戳格式。|
|deserializer|是|Event反序列化格式，支持的模式如下： -   **DELIMITED**：设置解析格式为分隔符模式。默认为该模式。
-   **JSON**：设置解析格式为JSON模式。
-   自定义**deserializer**：设置解析格式为自定义的反序列化模式，设置为该模式时需要填写完整列名称。 |
|columns|否|当deserializer为**DELIMITED**时，必须指定字段列表，用英文逗号（,）分隔，顺序与实际数据中的字段顺序一致。|
|separatorChar|否|当deserializer为**DELIMITED**时，用于指定数据的分隔符，必须为单个字符。默认为英文逗号（,）。|
|quoteChar|否|当deserializer为**DELIMITED**时，用于指定Quote字符。默认为英文引号（"）。|
|escapeChar|否|当deserializer为**DELIMITED**时，用于指定转义字符。默认为英文引号（"）。|
|appendTimestamp|否|当deserializer为**DELIMITED**时，用于设置是否将时间戳作为一个字段自动添加到每行末尾。默认为false。|
|sourceAsField|否|当deserializer为**JSON**时，用于设置是否将日志Source作为一个字段，字段名称为\_\_source\_\_。默认为false。|
|tagAsField|否|当deserializer为**JSON**时，用于设置是否将日志Tag作为字段，字段名称为\_\_tag\_\_：\{tag名称\}。默认为false。|
|timeAsField|否|当deserializer为**JSON**时，用于设置是否将日志时间作为一个字段，字段名称为\_\_time\_\_。默认为false。|
|useRecordTime|否|用于设置是否使用日志的时间，如果为false则使用当前时间。默认为false。|

Source配置示例请参见[GitHub](https://github.com/aliyun/aliyun-log-flume/blob/master/src/test/resources/source-example.conf)。

