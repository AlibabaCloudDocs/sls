# Storm消费

日志服务LogHub提供了高效、可靠的日志通道功能，您可以通过Logtail、SDK等多种方式来实时采集日志数据。采集日志数据之后，可以通过Storm实时消费写入到日志服务中的数据。为了降低Storm消费的代价，日志服务提供了LogHub Storm Spout来实时读取日志数据。

## 基本结构和流程

![LogService_user_guide_0175.png ](../images/p5809.png "基本结构和流程")

-   上图中红色虚线框中是LogHub Storm Spout，每个Storm Topology会有一组Spout，同组内的Spout共同负责读取Logstore中全部数据。不同Topology中的Spout相互不干扰。
-   每个Storm Topology需要选择唯一的消费组名称来相互标识，同一Topology内的Spout通过消费组消费日志数据来完成负载均衡和自动Failover，详情请参见[通过消费组消费日志数据](/cn.zh-CN/消费与投递/实时消费/消费组消费/通过消费组消费日志数据.md)。
-   Spout从LogHub中实时读取数据之后，发送至Storm Topology中的Bolt节点，定期将消费完成位置作为Checkpoint保存到LogHub服务端。

**说明：**

-   为了防止滥用，每个Logstore最多支持10个消费组，对于不再使用的消费组，可以调用DeleteConsumerGroup接口删除。
-   建议Spout的个数和Shard个数相同，否则可能会导致单个Spout处理数据量过多而影响性能。
-   如果单个Shard的数据量太大，超过一个Spout处理极限，则可以使用Shard split接口分裂Shard，来降低每个Shard的数据量。
-   在Loghub Spout中，强制依赖Storm的ACK机制，用于确认Spout将消息正确发送至Bolt，所以在Bolt中一定要调用ACK进行确认。

## 示例

-   Spout使用示例，用于构建Storm Topology。

    ```
         public static void main( String[] args )
        {     
            String mode = "Local";  //使用本地测试模式。
               String consumer_group_name = "";   //每个Topology需要设定唯一的消费组名称。
            String project = "";    // 日志服务的Project。 
            String logstore = "";   // 日志服务的Logstore。
            String endpoint = "";   // 日志服务访问域名。
            String access_id = "";  // 用户AK信息。
            String access_key = "";
            // 构建一个Loghub Storm Spout需要使用的配置。
            LogHubSpoutConfig config = new LogHubSpoutConfig(consumer_group_name,
                    endpoint, project, logstore, access_id,
                    access_key, LogHubCursorPosition.END_CURSOR);
            TopologyBuilder builder = new TopologyBuilder();
            // 构建Loghub Storm Spout。
            LogHubSpout spout = new LogHubSpout(config);
            // 在实际场景中，Spout的个数可以和Logstore Shard个数相同。
            builder.setSpout("spout", spout, 1);
            builder.setBolt("exclaim", new SampleBolt()).shuffleGrouping("spout");
            Config conf = new Config();
            conf.setDebug(false);
            conf.setMaxSpoutPending(1); 
            // 如果使用Kryo进行数据的序列化和反序列化，则需要显示设置LogGroupData的序列化方法LogGroupDataSerializSerializer。
            Config.registerSerialization(conf, LogGroupData.class, LogGroupDataSerializSerializer.class);
            if (mode.equals("Local")) {
                logger.info("Local mode...");
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

-   消费数据的bolt代码示例，打印每条日志的内容。

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
                // 每个logGroup由一条或多条日志组成。
                LogGroup logGroup = groupData.GetLogGroup();
                for (Log log : logGroup.getLogsList()) {
                    StringBuilder sb = new StringBuilder();
                    // 每条日志，有一个时间字段，以及多个Key，Value对。
                    int log_time = log.getTime();
                    sb.append("LogTime:").append(log_time);
                    for (Content content : log.getContentsList()) {
                        sb.append("\t").append(content.getKey()).append(":")
                                .append(content.getValue());
                    }
                    logger.info(sb.toString());
                }
            }
            // 在LogHub Spout中，强制依赖Storm的ACK机制，用于确认Spout将消息正确发送至bolt，所以在bolt中一定要调用ACK。
            mCollector.ack(tuple);
        }
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            //do nothing
        }
    }
    ```


## 添加Maven依赖

-   Storm 1.0版本及之前版本（如0.9.6版本），请使用：

    ```
    <dependency>
      <groupId>com.aliyun.openservices</groupId>
      <artifactId>loghub-storm-spout</artifactId>
      <version>0.6.6</version>
    </dependency>
    ```

-   storm 1.0版本及之后版本，请使用：

    ```
    <dependency>
      <groupId>com.aliyun.openservices</groupId>
      <artifactId>loghub-storm-1.0-spout</artifactId>
      <version>0.1.3</version>
    </dependency>
    ```


