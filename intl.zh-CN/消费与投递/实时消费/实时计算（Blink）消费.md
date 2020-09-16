# 实时计算（Blink）消费

阿里云实时计算通过创建日志服务源表的方式，可以直接消费日志服务中的数据。本文介绍了如何为实时计算创建日志服务源表以及创建过程涉及到的属性字段提取和类型映射。

## 创建日志服务源表

日志服务是实时数据存储，实时计算能将其作为流式数据输入。假设有如下日志内容：

```
__source__:  11.85.123.199
__tag__:__receive_time__:  1562125591
__topic__:  test-topic
a:  1234
b:  0
c:  hello
```

实时计算日志服务源表DDL示例如下：

```
create table sls_stream(
  a int,
  b int,
  c varchar
) with (
  type ='sls',
  endPoint ='<your endpoint>',
  accessId ='<your AccessKey ID>',
  accessKey ='<your AccessKey Secret>',
  startTime = '2017-07-05 00:00:00',
  project ='ali-cloud-streamtest',
  logStore ='stream-test',
  consumerGroup ='consumerGroupTest1'
);
```

WITH参数说明：

|参数|是否必须|说明|
|--|----|--|
|endPoint|是|日志服务Endpoint，详情请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。|
|accessId|是|访问日志服务的AccessKey ID。|
|accessKey|是|访问日志服务的密钥AccessKey Secret。|
|project|是|日志服务Project名称。|
|logStore|是|日志服务Logstore名称。|
|consumerGroup|否|消费组名称。|
|startTime|否|消费日志开始的时间点。|
|heartBeatIntervalMills|否|消费客户端的心跳间隔时间，默认为10秒。|
|maxRetryTimes|否|读取最大尝试次数，默认5次。|
|batchGetSize|否|单次读取logGroup条数，默认为10条。如果Blink的版本是1.4.2版本及以上版本，则默认为100条，最大1000条。**说明：** 如果单条日志的大小和batchGetSize都很大，可能会导致Java系统频繁的对内存数据进行垃圾回收。 |
|columnErrorDebug|否|是否打开调试开关，如果打开，会把解析异常的日志打印出来。默认为false。|

## 属性字段提取

除日志字段外，支持提取如下三个属性字段，也支持提取其它自定义字段。

|属性字段|说明|
|:---|:-|
|`__source__`|日志来源。|
|`__topic__`|日志主题。|
|`__timestamp__`|日志时间。|

属性字段的提取需要添加HEADER声明，示例如下：

```
create table sls_stream(
  __timestamp__ bigint HEADER,
  __receive_time__ bigint HEADER
  b int,
  c varchar
) with (
  type ='sls',
  endPoint ='<your endpoint>',
  accessId ='<your AccessKey ID>',
  accessKey ='<your AccessKey Secret>',
  startTime = '2017-07-05 00:00:00',
  project ='ali-cloud-streamtest',
  logStore ='stream-test',
  consumerGroup ='consumerGroupTest1'
);
```

## 类型映射

日志服务字段类型和实时计算字段类型对应关系如下，建议您使用该对应关系进行DDL声明。

|日志服务字段类型|实时计算字段类型|
|--------|--------|
|STRING|VARCHAR|

如果使用其他类型也会尝试自动转换，例如`1000`或者`2018-01-12 12:00:00`也可以定义为bigint和timestamp类型。

## 注意事项

-   Blink 2.2.0版本及之前的版本不支持Shard的扩容和缩容，如果分裂或者缩容了某个正在实时计算读取的Logstore，会导致任务持续出错且不可恢复，这种情况下需要重新启动任务来使任务恢复正常。
-   所有blink版本均不支持对正在消费的LogStore进行删除重建。
-   Blink 1.6.0版本及之前版本，在Shard数目很多的情况下指定消费组可能会影响读取性能。
-   日志服务暂不支持Map类型的数据。
-   不存在的字段默认为null。
-   字段顺序支持无序，建议您在设置时字段顺序和表中参数顺序一致。
-   如果一个Shard没有新数据写入，会导致任务的整体延迟增加。在这种情况下，只需要把并发数调整为读写的Shard数量即可。
-   针对`__tag__:__hostname__`、`__tag__:__path__`等字段，去掉前面的tag，按照获取属性字段的方式获取即可。

    **说明：** 调试时无法抽取到该类型数据，建议您用本地DEBUG方法，上线运行后打印到日志中查看。


