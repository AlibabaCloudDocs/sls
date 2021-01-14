# Use LogHub Storm to consume log data

Log Service provides efficient and reliable log collection and consumption channels. You can use Logtail or an SDK to collect log data in real time. After logs are collected to LogHub, you can use LogHub Storm to consume the log data in real time. To reduce the consumption cost of LogHub data, Log Service provides LogHub Storm spouts for Storm users to read data from LogHub in real time.

## Architecture and implementation

![Architecture and implementation](../images/p5809.png "Architecture and implementation")

-   In the preceding figure, LogHub Storm spouts are in red dashed-line boxes. Each Storm topology has a group of spouts that jointly work to read data from a Logstore. Spouts in different topologies are independent of each other.
-   Each topology is identified based on the name of the consumer group that is included in the topology. Spouts in a topology use a consumer group to consume data. This ensures automatic loading and failover. For more information, see [Use consumer groups to consume log data](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Consumption by consumer groups/Use consumer groups to consume log data.md).
-   Spouts in a topology read data from a Logstore in real time and send the data to bolts in the topology. Consumption checkpoints are saved to the LogHub server on a regular basis.

**Note:**

-   You can specify a maximum of 10 consumer groups to consume data from a Logstore. If you no longer need a consumer group, you can call the DeleteConsumerGroup operation to delete the consumer group.
-   We recommend that you configure the same number of spouts for a Logstore as the number of shards in the Logstore. This is because a single spout may not be able to process a large amount of data from multiple shards.
-   If the data volume in a shard exceeds the processing capacity of a single spout, you can split the shard to multiple shards. This way, the data volume in each new shard is reduced.
-   LogHub spouts and bolts must use the ack method to check whether log data is sent from spouts to bolts and whether the data is processed by the bolts.

## Examples

-   The following script shows how to configure a LogHub Storm topology:

    ```
         public static void main( String[] args )
        {     
            String mode = "Local"; // Use the local test mode.
            String consumer_group_name = ""; // The unique consumer group name of a topology.
            String project = ""; // The project in Log Service. 
            String logstore = ""; // The Logstore in Log Service.
            String endpoint = ""; // The endpoint of Log Service.
            String access_id = ""; // The AccessKey ID.
            String access_key = "";
            // Configure a LogHub Storm spout.
            LogHubSpoutConfig config = new LogHubSpoutConfig(consumer_group_name,
                    endpoint, project, logstore, access_id,
                    access_key, LogHubCursorPosition.END_CURSOR);
            TopologyBuilder builder = new TopologyBuilder();
            // Create a LogHub Storm spout.
            LogHubSpout spout = new LogHubSpout(config);
            // You can create the same number of spouts for a Logstore as the number of shards in the Logstore in actual business scenarios.
            builder.setSpout("spout", spout, 1);
            builder.setBolt("exclaim", new SampleBolt()).shuffleGrouping("spout");
            Config conf = new Config();
            conf.setDebug(false);
            conf.setMaxSpoutPending(1); 
            // If you use Kryo to serialize and deserialize data, you must set the serialization method of log groups by using the LogGroupDataSerializSerializer class.
            Config.registerSerialization(conf, LogGroupData.class, LogGroupDataSerializSerializer.class);
            if (mode.equals("Local")) {
                logger.info("Local mode...") ;
                LocalCluster cluster  = new LocalCluster();
                cluster.submitTopology("test-jstorm-spout", conf, builder.createTopology());
                try {
                    Thread.sleep(6000 * 1000);   
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }  
                cluster.killTopology("test-jstorm-spout");
                cluster.shutdown();  
            } else if (mode.equals("Remote")) {
                logger.info("Remote mode...");
                conf.setNumWorkers(2);
                try {
                    StormSubmitter.submitTopology("stt-jstorm-spout-4", conf, builder.createTopology());
                } catch (AlreadyAliveException e) {
                    e.printStackTrace();
                } catch (InvalidTopologyException e) {
                    e.printStackTrace();
                }
            } else {
                logger.error("invalid mode: " + mode);
            }
        }
    }
    ```

-   Use the following example code of bolts to consume log data and display the content of each log entry:

    ```
    public class SampleBolt extends BaseRichBolt {
        private static final long serialVersionUID = 4752656887774402264L;
        private static final Logger logger = Logger.getLogger(BaseBasicBolt.class);
        private OutputCollector mCollector;
        @Override
        public void prepare(@SuppressWarnings("rawtypes") Map stormConf, TopologyContext context,
                OutputCollector collector) {
            mCollector = collector;
        }
        @Override
        public void execute(Tuple tuple) {
            String shardId = (String) tuple
                    .getValueByField(LogHubSpout.FIELD_SHARD_ID);
            @SuppressWarnings("unchecked")
            List<LogGroupData> logGroupDatas = (ArrayList<LogGroupData>) tuple.getValueByField(LogHubSpout.FIELD_LOGGROUPS);
            for (LogGroupData groupData : logGroupDatas) {
                // Each log group consists of one or more log entries.
                LogGroup logGroup = groupData.GetLogGroup();
                for (Log log : logGroup.getLogsList()) {
                    StringBuilder sb = new StringBuilder();
                    // Each log entry has a time field and other fields formatted in key-value pairs.
                    int log_time = log.getTime();
                    sb.append("LogTime:").append(log_time);
                    for (Content content : log.getContentsList()) {
                        sb.append("\t").append(content.getKey()).append(":")
                                .append(content.getValue());
                    }
                    logger.info(sb.toString());
                }
            }
            LogHub spouts and bolts must use the ack method to check whether log data is sent from spouts to bolts and whether the data is processed by the bolts.
            mCollector.ack(tuple);
        }
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            //do nothing
        }
    }
    ```


## Add Maven dependencies

Use the following code to add Maven dependencies for LogHub Storm 1.0 or earlier \(for example, Storm 0.9.6\):

```
<dependency>
  <groupId>com.aliyun.openservices</groupId>
  <artifactId>loghub-storm-spout</artifactId>
  <version>0.6.6</version>
</dependency>
```

Use the following code to add Maven dependencies for LogHub Storm 1.0 or later:

```
<dependency>
  <groupId>com.aliyun.openservices</groupId>
  <artifactId>loghub-storm-1.0-spout</artifactId>
  <version>0.1.3</version>
</dependency>
```

