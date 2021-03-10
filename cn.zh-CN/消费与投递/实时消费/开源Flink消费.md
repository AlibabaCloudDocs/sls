# 开源Flink消费

Flink Log Connector是日志服务提供的用于对接Flink的工具。本文介绍如何对接Flink消费日志数据。

-   已开启Access Key，详情请参见[开启Access Key](/cn.zh-CN/数据采集/准备工作/准备工作.md)。
-   已创建Project和Logstore，详情请参见[步骤二：创建Project和Logstore](/cn.zh-CN/.md)。
-   已为RAM用户授权，详情请参见[指定Logstore的消费权限](/cn.zh-CN/开发指南/访问控制RAM/RAM自定义授权场景.md)。

Flink Log Connector包括两部分，消费者（Flink Log Consumer）和生产者（Flink Log Producer），两者用途区别如下：

-   消费者用于从日志服务中读取数据，支持exactly once语义，支持Shard负载均衡。
-   生产者用于将数据写入日志服务。

使用Flink Log Connector时，需要在项目中添加maven依赖，示例如下：

```
<dependency>
   <groupId>com.aliyun.openservices</groupId>
   <artifactId>flink-log-connector</artifactId>
   <version>0.1.13</version>
</dependency>
<dependency>
   <groupId>com.google.protobuf</groupId>
   <artifactId>protobuf-java</artifactId>
   <version>2.5.0</version>
</dependency>
```

更多关于Flink Log Connector的信息请参见[Github](https://github.com/aliyun/aliyun-log-flink-connector)。

## Flink Log Consumer

在Flink Log Connector中，Flink Log Consumer提供了订阅日志服务中某一个Logstore的能力，实现了exactly once语义，在使用时您无需关心Logstore中Shard数量的变化，Flink Log Consumer会自动感知。

Flink中每一个子任务负责消费Logstore中的部分Shard，如果Logstore中Shard发生分裂或合并，子任务消费的Shard也会随之改变。

Flink Log Consumer用到的日志服务API接口如下：

-   GetCursorOrData

    用于从Shard中获取数据，注意频繁的调用该接口可能会导致数据超过日志服务的Shard限额，可以通过ConfigConstants.LOG\_FETCH\_DATA\_INTERVAL\_MILLIS和ConfigConstants.LOG\_MAX\_NUMBER\_PER\_FETCH控制接口调用的时间间隔和每次调用获取的日志数量，Shard的限额请参见[分区](/cn.zh-CN/产品简介/基本概念/分区.md)。

    示例如下：

    ```
    configProps.put(ConfigConstants.LOG_FETCH_DATA_INTERVAL_MILLIS， "100");
    configProps.put(ConfigConstants.LOG_MAX_NUMBER_PER_FETCH， "100");
    ```

-   ListShards

    用于获取Logstore中所有的Shard列表及Shard状态等。如果您的Shard经常发生分裂合并，可以通过调整接口的调用周期来及时发现Shard的变化。示例如下：

    ```
    // 设置每30s调用一次ListShards接口。
    configProps.put(ConfigConstants.LOG_SHARDS_DISCOVERY_INTERVAL_MILLIS， "30000");
    ```

-   CreateConsumerGroup

    当设置消费进度监控时调用该接口创建ConsumerGroup，用于同步Checkpoint。

-   ConsumerGroupUpdateCheckPoint

    该接口将Flink的snapshot同步到日志服务的ConsumerGroup中。


1.  配置启动参数。

    以下是一个简单的消费示例，使用java.util.Properties作为配置工具，所有Flink Log Consumer的配置均在ConfigConstants中。

    ```
    Properties configProps = new Properties();
    // 设置访问日志服务的域名。
    configProps.put(ConfigConstants.LOG_ENDPOINT， "cn-hangzhou.log.aliyuncs.com");
    // 设置用户AccessKey ID及AccessKey Secret。
    configProps.put(ConfigConstants.LOG_ACCESSKEYID， "");
    configProps.put(ConfigConstants.LOG_ACCESSKEY， "");
    // 设置日志服务的project。
    configProps.put(ConfigConstants.LOG_PROJECT， "ali-cn-hangzhou-sls-admin");
    // 设置日志服务的Logstore。
    configProps.put(ConfigConstants.LOG_LOGSTORE， "sls_consumergroup_log");
    // 设置消费日志服务起始位置。
    configProps.put(ConfigConstants.LOG_CONSUMER_BEGIN_POSITION， Consts.LOG_END_CURSOR);
    // 设置日志服务的消息反序列化方法。
    RawLogGroupListDeserializer deserializer = new RawLogGroupListDeserializer();
    final StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
    DataStream<RawLogGroupList> logTestStream = env.addSource(
            new FlinkLogConsumer<RawLogGroupList>(deserializer， configProps));
    ```

    **说明：** Flink的子任务数量和日志服务Logstore中的Shard数量是独立的，如果Shard数量多于子任务数量，每个子任务不重复的消费Shard，如果少于子任务数量，那么部分子任务就会空闲，直到新的Shard产生。

2.  设置消费起始位置。

    Flink Log Consumer支持设置Shard的消费起始位置，通过设置属性ConfigConstants.LOG\_CONSUMER\_BEGIN\_POSITION，就可以定制从Shard的头、尾或者某个特定时间开始消费。另外，Flink Log Connector也支持从某个具体的消费组中恢复消费。属性的具体取值如下：

    -   Consts.LOG\_BEGIN\_CURSOR：表示从Shard的头开始消费，也就是从Shard中最旧的数据开始消费。
    -   Consts.LOG\_END\_CURSOR：表示从Shard的尾开始，也就是从Shard中最新的数据开始消费。
    -   Consts.LOG\_FROM\_CHECKPOINT：表示从某个特定的消费组中保存的Checkpoint开始消费，通过ConfigConstants.LOG\_CONSUMERGROUP指定具体的消费组。
    -   UnixTimestamp：一个整型数值的字符串，用1970-01-01到现在的秒数表示，含义是消费Shard中这个时间点之后的数据。
    示例如下：

    ```
    configProps.put(ConfigConstants.LOG_CONSUMER_BEGIN_POSITION, Consts.LOG_BEGIN_CURSOR);
    configProps.put(ConfigConstants.LOG_CONSUMER_BEGIN_POSITION, Consts.LOG_END_CURSOR);
    configProps.put(ConfigConstants.LOG_CONSUMER_BEGIN_POSITION, "1512439000");
    configProps.put(ConfigConstants.LOG_CONSUMER_BEGIN_POSITION, Consts.LOG_FROM_CHECKPOINT);
    ```

    **说明：** 如果在启动Flink任务时，设置了从Flink自身的StateBackend中恢复，那么Flink Log Connector会忽略上面的配置，使用StateBackend中保存的Checkpoint。

3.  设置消费进度监控。

    Flink Log Consumer支持设置消费进度监控，获取每一个Shard的实时消费位置，使用时间戳表示，详情请参见[查看消费组状态](/cn.zh-CN/消费与投递/实时消费/消费组消费/通过消费组消费日志数据.md)和[消费组监控与告警](/cn.zh-CN/消费与投递/实时消费/消费组消费/消费组监控与告警.md)。

    示例如下：

    ```
    configProps.put(ConfigConstants.LOG_CONSUMERGROUP, "your consumer group name");
    ```

    **说明：** 该项为可选配置项，设置后Flink Log Consumer会首先创建消费组，如果消费组已经存在，则不执行任何操作，Flink Log Consumer中的snapshot会自动同步到日志服务的消费组中，您可以通过日志服务的控制台查看Flink Log Consumer的消费进度。

4.  设置容灾和exactly once语义支持。

    当打开Flink的Checkpointing功能时，Flink Log Consumer会周期性的将每个Shard的消费进度保存，当任务失败时，Flink会恢复消费任务，并从保存的最新的Checkpoint开始消费。

    Checkpoint的周期定义了当任务失败时，最多多少的数据会被回溯，即重新消费，使用代码如下：

    ```
    final StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
    // 开启Flink exactly once语义。
    env.getCheckpointConfig().setCheckpointingMode(CheckpointingMode.EXACTLY_ONCE);
    // 每5s保存一次Checkpoint。
    env.enableCheckpointing(5000);
    ```

    更多Flink Checkpoint的信息请参见[Checkpoints](https://ci.apache.org/projects/flink/flink-docs-release-1.3/setup/checkpoints.html)。


## Flink Log Producer

Flink Log Producer用于将数据写入日志服务。

**说明：** Flink Log Producer只支持Flink at least once语义，在任务失败时，写入日志服务中的数据有可能会重复，但不会丢失。

Flink Log Producer用到的日志服务API接口如下：

-   PostLogStoreLogs
-   ListShards

1.  初始化Flink Log Producer。

    1.  初始化配置参数Properties。

        Flink Log Producer初始化步骤与Flink Log Consumer类似。Flink Log Producer初始化配置包含以下参数，一般情况下使用默认值即可，如有需要可以自定义配置。示例如下：

        ```
        // 用于发送数据的I/O线程的数量，默认是8。
        ConfigConstants.LOG_SENDER_IO_THREAD_COUNT
        // 该值定义日志数据被缓存发送的时间，默认是3000。
        ConfigConstants.LOG_PACKAGE_TIMEOUT_MILLIS
        // 缓存发送的包中日志的数量，默认是4096
        ConfigConstants.LOG_LOGS_COUNT_PER_PACKAGE
        // 缓存发送的包的大小，默认是3 MB。
        ConfigConstants.LOG_LOGS_BYTES_PER_PACKAGE
        // 作业可以使用的内存总的大小，默认是100 MB。
        ConfigConstants.LOG_MEM_POOL_BYTES
        ```

    2.  重载LogSerializationSchema，定义将数据序列化成RawLogGroup的方法。

        RawLogGroup是日志的集合，各字段含义请参见[日志（Log）](/cn.zh-CN/开发指南/API 参考/公共资源说明/数据模型.md)。

        如果您需要指定数据写到某一个Shard中，可以使用LogPartitioner产生数据的HashKey，LogPartitioner为可选项，如果您没有配置，数据会随机写入某一个Shard。

        示例如下：

        ```
        FlinkLogProducer<String> logProducer = new FlinkLogProducer<String>(new SimpleLogSerializer()， configProps);
        logProducer.setCustomPartitioner(new LogPartitioner<String>() {
              // 生成32位Hash值。
              public String getHashKey(String element) {
                  try {
                      MessageDigest md = MessageDigest.getInstance("MD5");
                      md.update(element.getBytes());
                      String hash = new BigInteger(1， md.digest()).toString(16);
                      while(hash.length() < 32) hash = "0" + hash;
                      return hash;
                  } catch (NoSuchAlgorithmException e) {
                  }
                  return  "0000000000000000000000000000000000000000000000000000000000000000";
              }
          });
        ```

2.  将模拟产生的字符串写入日志服务，示例如下：

    ```
    // 将数据序列化成日志服务的数据格式。
    class SimpleLogSerializer implements LogSerializationSchema<String> {
        public RawLogGroup serialize(String element) {
            RawLogGroup rlg = new RawLogGroup();
            RawLog rl = new RawLog();
            rl.setTime((int)(System.currentTimeMillis() / 1000));
            rl.addContent("message"， element);
            rlg.addLog(rl);
            return rlg;
        }
    }
    public class ProducerSample {
        public static String sEndpoint = "cn-hangzhou.log.aliyuncs.com";
        public static String sAccessKeyId = "";
        public static String sAccessKey = "";
        public static String sProject = "ali-cn-hangzhou-sls-admin";
        public static String sLogstore = "test-flink-producer";
        private static final Logger LOG = LoggerFactory.getLogger(ConsumerSample.class);
        public static void main(String[] args) throws Exception {
            final ParameterTool params = ParameterTool.fromArgs(args);
            final StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
            env.getConfig().setGlobalJobParameters(params);
            env.setParallelism(3);
            DataStream<String> simpleStringStream = env.addSource(new EventsGenerator());
            Properties configProps = new Properties();
            // 设置访问日志服务的域名。
            configProps.put(ConfigConstants.LOG_ENDPOINT， sEndpoint);
            // 设置用户AK。
            configProps.put(ConfigConstants.LOG_ACCESSKEYID， sAccessKeyId);
            configProps.put(ConfigConstants.LOG_ACCESSKEY， sAccessKey);
            // 设置日志写入的日志服务project。
            configProps.put(ConfigConstants.LOG_PROJECT， sProject);
            // 设置日志写入的日志服务Logstore。
            configProps.put(ConfigConstants.LOG_LOGSTORE， sLogstore);
            FlinkLogProducer<String> logProducer = new FlinkLogProducer<String>(new SimpleLogSerializer()， configProps);
            simpleStringStream.addSink(logProducer);
            env.execute("flink log producer");
        }
        // 模拟产生日志。
        public static class EventsGenerator implements SourceFunction<String> {
            private boolean running = true;
            @Override
            public void run(SourceContext<String> ctx) throws Exception {
                long seq = 0;
                while (running) {
                    Thread.sleep(10);
                    ctx.collect((seq++) + "-" + RandomStringUtils.randomAlphabetic(12));
                }
            }
            @Override
            public void cancel() {
                running = false;
            }
        }
    }
    ```


