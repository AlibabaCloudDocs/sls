# Use open source Flink to consume log data

Log Service provides the flink-log-connector agent to connect with Flink. This topic describes how to connect Log Service with Flink to consume logs.

-   An AccessKey pair is created. For more information, see [Obtain an AccessKey pair](/intl.en-US/Data Collection/Preparation/Preparations.md).
-   A project and a Logstore are created. For more information, see [Step 1: Create a project and a Logstore](/intl.en-US/.md).
-   A RAM user is authorized to access the Logstore from which data is consumed. For more information, see [Grant a RAM user the permissions to consume data from a specified Logstore](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md).

The flink-log-connector agent consists of the flink-log-consumer and flink-log-producer agents. The two agents have the following differences:

-   The flink-log-consumer agent reads data from Log Service. This agent supports the exactly-once semantics and load balancing among shards.
-   The flink-log-producer agent writes data to Log Service.

You must add Maven dependencies before you use the flink-log-producer agent to write data to Log Service. The following example shows sample Maven dependencies:

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

For more information about the flink-log-producer agent, visit [GitHub](https://github.com/aliyun/aliyun-log-flink-connector).

## Flink Log Consumer

The flink-log-consumer agent can consume log data from a Logstore. The exactly-once semantics is applied during log consumption. The flink-log-consumer agent detects the change in the number of shards in a Logstore.

Each Flink subtask consumes data from some shards in a Logstore. If the shards in a Logstore are split or merged, the shards consumed by the subtask also change.

When you use the flink-log-consumer agent to consume data from Log Service, you can call the following API operations:

-   GetCursorOrData

    You can call this operation to pull log data from a shard. If you frequently call this operation, data transfer may exceed the capacities of shards. You can use the ConfigConstants.LOG\_FETCH\_DATA\_INTERVAL\_MILLIS parameter to control the interval of API calls. You can use the ConfigConstants.LOG\_MAX\_NUMBER\_PER\_FETCH parameter to control the number of log entries pulled by each call. For more information about the shard quota, see [Shards](/intl.en-US/Product Introduction/Basic concepts/Shards.md).

    Example:

    ```
    configProps.put(ConfigConstants.LOG_FETCH_DATA_INTERVAL_MILLIS, "100");
    configProps.put(ConfigConstants.LOG_MAX_NUMBER_PER_FETCH, "100");
    ```

-   ListShards

    You can call this operation to view all shards in a Logstore and the status of each shard. If the shards are frequently split and merged, you can adjust the call interval to detect the changes in the number of shards in a timely manner. Example:

    ```
    // Call the ListShards operation once every 30 seconds.
    configProps.put(ConfigConstants.LOG_SHARDS_DISCOVERY_INTERVAL_MILLIS, "30000");
    ```

-   CreateConsumerGroup

    You can call this operation to create a consumer group to synchronize checkpoints.

-   ConsumerGroupUpdateCheckPoint

    You can call this operation to synchronize snapshots of Flink to a consumer group.


1.  Set startup parameters.

    The following example shows how to consume log data. The java.util.Properties class is used as a configuration tool. You can find all constants to be configured in the ConfigConstants class.

    ```
    Properties configProps = new Properties();
    // Specify the endpoint of Log Service.
    configProps.put(ConfigConstants.LOG_ENDPOINT, "cn-hangzhou.log.aliyuncs.com");
    // Specify the AccessKey ID and AccessKey secret.
    configProps.put(ConfigConstants.LOG_ACCESSKEYID, "");
    configProps.put(ConfigConstants.LOG_ACCESSKEY, "");
    // Specify the project.
    configProps.put(ConfigConstants.LOG_PROJECT, "ali-cn-hangzhou-sls-admin");
    // Specify the Logstore.
    configProps.put(ConfigConstants.LOG_LOGSTORE, "sls_consumergroup_log");
    // Specify the start position of log consumption.
    configProps.put(ConfigConstants.LOG_CONSUMER_BEGIN_POSITION, Consts.LOG_END_CURSOR);
    // Specify the data deserialization method.
    RawLogGroupListDeserializer deserializer = new RawLogGroupListDeserializer();
    final StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
    DataStream<RawLogGroupList> logTestStream = env.addSource(
            new FlinkLogConsumer<RawLogGroupList>(deserializer, configProps));
    ```

    **Note:** The number of Flink subtasks is independent of the number of shards in a Logstore. If the number of shards is greater than the number of subtasks, each subtask consumes one or more shards. If the number of shards is less than the number of subtasks, some subtasks are idle until new shards are generated. Each shard is consumed by only one subtask.

2.  Specify the start position of log consumption.

    When you use the flink-log-consumer agent to consume data from a Logstore, you can use the ConfigConstants.LOG\_CONSUMER\_BEGIN\_POSITION parameter to specify the start position of log consumption. You can start to consume data from the earliest entry, the latest entry, or from a specific time. In addition, the flink-log-consumer agent also allows you to resume consumption from a specific consumer group. You can set the parameter to one of the following values:

    -   Consts.LOG\_BEGIN\_CURSOR: starts to consume data from the earliest entry.
    -   Consts.LOG\_END\_CURSOR: starts to consume data from the latest entry.
    -   Consts.LOG\_FROM\_CHECKPOINT: starts to consume data from a checkpoint that is stored in a specified consumer group. You can use the ConfigConstants.LOG\_CONSUMERGROUP parameter to specify the consumer group.
    -   UnixTimestamp: a string of the INTEGER data type. The timestamp is the number of seconds that have elapsed since 00:00:00 Thursday January 1, 1970. The value indicates that the data in a shard is consumed from this point in time.
    Example:

    ```
    configProps.put(ConfigConstants.LOG_CONSUMER_BEGIN_POSITION, Consts.LOG_BEGIN_CURSOR);
    configProps.put(ConfigConstants.LOG_CONSUMER_BEGIN_POSITION, Consts.LOG_END_CURSOR);
    configProps.put(ConfigConstants.LOG_CONSUMER_BEGIN_POSITION, "1512439000");
    configProps.put(ConfigConstants.LOG_CONSUMER_BEGIN_POSITION, Consts.LOG_FROM_CHECKPOINT);
    ```

    **Note:** If you have configured consumption resumption from a state backend of Flink when you start a Flink job, the flink-log-connector agent uses checkpoints stored in the state backend.

3.  Configure consumption progress monitoring.

    The flink-log-connector agent allows you to monitor consumption progress. You can use the monitoring feature to obtain the real-time consumption position of each shard. The consumption position is indicated by a timestamp. For more information, see [View consumer group status](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Consumption by consumer groups/Use consumer groups to consume log data.md).

    Example:

    ```
    configProps.put(ConfigConstants.LOG_CONSUMERGROUP, "your consumer group name");
    ```

    **Note:** This setting is optional. If you configure consumption progress monitoring and no consumer group exists, the flink-log-connector agent creates a consumer group. If a consumer group is available, the agent synchronizes snapshots to the consumer group. You can view the consumption progress of the agent in the Log Service console.

4.  Configure consumption resumption and the exactly-once semantics.

    If the checkpointing feature of Flink is enabled, the flink-log-consumer agent periodically stores the consumption progress of each shard. If a subtask fails, Flink restores the subtask and starts to consume data from the latest checkpoint.

    When you configure the checkpoint period, you can define the maximum amount of data to be re-consumed if a failure occurs. You can use the following code to configure the checkpoint period:

    ```
    final StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
    // Configure the exactly-once semantics.
    env.getCheckpointConfig().setCheckpointingMode(CheckpointingMode.EXACTLY_ONCE);
    // Save checkpoints every 5 seconds.
    env.enableCheckpointing(5000);
    ```

    For more information about Flink checkpoints, see [Checkpoints](https://ci.apache.org/projects/flink/flink-docs-release-1.3/setup/checkpoints.html).


## Flink Log Producer

The flink-log-producer agent writes data to Log Service.

**Note:** The flink-log-producer agent supports only the Flink at-least-once semantics. If a task fails, some data that is written to Log Service may be duplicated. However, no data will be lost.

When you use the flink-log-producer agent to write data to Log Service, you can call the following API operations:

-   PostLogStoreLogs
-   ListShards

1.  Initialize the flink-log-producer agent.

    1.  Set the initialization parameters of the flink-log-producer agent.

        The flink-log-producer agent is initialized the same way as the flink-log-consumer agent. The following example shows how to configure the initialization parameters of the flink-log-producer agent. In most cases, you can use the default values of the parameters. Example:

        ```
        // The number of I/O threads that are used to send data. Default value: 8.
        ConfigConstants.LOG_SENDER_IO_THREAD_COUNT
        // The time that is required to send cached logs. Default value: 3000.
        ConfigConstants.LOG_PACKAGE_TIMEOUT_MILLIS
        // The number of logs in the cached packet. Default value: 4096.
        ConfigConstants.LOG_LOGS_COUNT_PER_PACKAGE
        // The size of the cached packet. Default value: 3 MB.
        ConfigConstants.LOG_LOGS_BYTES_PER_PACKAGE
        // The total memory size that the job can use. Default value: 100 MB.
        ConfigConstants.LOG_MEM_POOL_BYTES
        ```

    2.  Reload LogSerializationSchema and define the method that is used to serialize data into raw log groups.

        A raw log group is a collection of log entries. For information about log fields, see [Log entry](/intl.en-US/Developer Guide/API Reference/Common resources/Data model.md).

        If you need to write data to a specific shard, you can use the LogPartitioner parameter to generate hash keys for log data. LogPartitioner is an optional parameter. If you do not specify this parameter, data is written to a random shard.

        Example:

        ```
        FlinkLogProducer<String> logProducer = new FlinkLogProducer<String>(new SimpleLogSerializer(), configProps);
        logProducer.setCustomPartitioner(new LogPartitioner<String>() {
              // Generate a 32-bit hash value.
              public String getHashKey(String element) {
                  try {
                      MessageDigest md = MessageDigest.getInstance("MD5");
                      md.update(element.getBytes());
                      String hash = new BigInteger(1, md.digest()).toString(16);
                      while(hash.length() < 32) hash = "0" + hash;
                      return hash;
                  } catch (NoSuchAlgorithmException e) {
                  }
                  return  "0000000000000000000000000000000000000000000000000000000000000000";
              }
          });
        ```

2.  Write simulated data to Log Service, as shown in the following example:

    ```
    // Serialize data into the format of raw log groups.
    class SimpleLogSerializer implements LogSerializationSchema<String> {
        public RawLogGroup serialize(String element) {
            RawLogGroup rlg = new RawLogGroup();
            RawLog rl = new RawLog();
            rl.setTime((int)(System.currentTimeMillis() / 1000));
            rl.addContent("message", element);
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
            // Specify the endpoint of Log Service.
            configProps.put(ConfigConstants.LOG_ENDPOINT, sEndpoint);
            // Specify the AccessKey ID and AccessKey secret.
            configProps.put(ConfigConstants.LOG_ACCESSKEYID, sAccessKeyId);
            configProps.put(ConfigConstants.LOG_ACCESSKEY, sAccessKey);
            // Specify the project to which logs are written.
            configProps.put(ConfigConstants.LOG_PROJECT, sProject);
            // Specify the Logstore to which logs are written.
            configProps.put(ConfigConstants.LOG_LOGSTORE, sLogstore);
            FlinkLogProducer<String> logProducer = new FlinkLogProducer<String>(new SimpleLogSerializer(), configProps);
            simpleStringStream.addSink(logProducer);
            env.execute("flink log producer");
        }
        // Simulate log generation.
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


