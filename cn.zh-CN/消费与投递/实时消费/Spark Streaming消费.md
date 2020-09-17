# Spark Streaming消费

日志服务采集到日志数据后，可以通过运行Spark Streaming任务消费日志数据。

日志服务提供的Spark SDK实现了Receiver模式和Direct模式两种消费模式。Maven依赖如下：

```
<dependency>
  <groupId>com.aliyun.emr</groupId>
  <artifactId>emr-logservice_2.11</artifactId>
  <version>1.7.2</version>
</dependency>
```

## Receiver模式

Receiver模式通过消费组消费日志数据并暂存在Spark Executor，Spark Streaming任务启动之后从Executor读取并处理数据。每条数据以JSON字符串的形式返回。消费组自动定时保存Checkpoint到服务端，无需手动更新Checkpoint。 详情请参见[通过消费组消费日志数据](/cn.zh-CN/消费与投递/实时消费/消费组消费/通过消费组消费日志数据.md)。

-   参数说明

    |参数|类型|说明|
    |--|--|--|
    |project|String|日志服务Project名称。|
    |logstore|String|日志服务Logstore名称。|
    |consumerGroup|String|消费组名称。|
    |endpoint|String|日志服务Project所在地域的Endpoint，详情请参见[服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)。|
    |accessKeyId|String|访问日志服务的AccessKey ID。|
    |accessKeySecret|String|访问日志服务的AccessKey Secret。|

-   示例

    **说明：** 默认配置下，Receiver模式在异常情况下可能导致数据丢失。为了避免此类情况发生，建议开启Write-Ahead Logs开关（Spark 1.2以上版本支持）。更多关于Write-Ahead Logs的细节请参见[Spark](https://spark.apache.org/docs/latest/streaming-programming-guide.html#deploying-applications)。

    ```
    import org.apache.spark.storage.StorageLevel
    import org.apache.spark.streaming.aliyun.logservice.LoghubUtils
    import org.apache.spark.streaming.{Milliseconds, StreamingContext}
    import org.apache.spark.SparkConf
    
    object TestLoghub {
      def main(args: Array[String]): Unit = {
        if (args.length < 7) {
          System.err.println(
            """Usage: TestLoghub <project> <logstore> <loghub group name> <endpoint>
              |         <access key id> <access key secret> <batch interval seconds>
            """.stripMargin)
          System.exit(1)
        }
    
        val project = args(0)
        val logstore = args(1)
        val consumerGroup = args(2)
        val endpoint = args(3)
        val accessKeyId = args(4)
        val accessKeySecret = args(5)
        val batchInterval = Milliseconds(args(6).toInt * 1000)
    
        def functionToCreateContext(): StreamingContext = {
          val conf = new SparkConf().setAppName("Test Loghub")
          val ssc = new StreamingContext(conf, batchInterval)
          val loghubStream = LoghubUtils.createStream(
            ssc,
            project,
            logstore,
            consumerGroup,
            endpoint,
            accessKeyId,
            accessKeySecret,
            StorageLevel.MEMORY_AND_DISK)
    
          loghubStream.checkpoint(batchInterval * 2).foreachRDD(rdd =>
            rdd.map(bytes => new String(bytes)).top(10).foreach(println)
          )
          ssc.checkpoint("hdfs:///tmp/spark/streaming") // set checkpoint directory
          ssc
        }
    
        val ssc = StreamingContext.getOrCreate("hdfs:///tmp/spark/streaming", functionToCreateContext _)
    
        ssc.start()
        ssc.awaitTermination()
      }
    }
    ```


## Direct模式

Direct模式不需要消费组，使用API在任务运行时直接从服务端请求数据。Direct模式具有如下优势：

-   简化并行：Spark partition个数与Logstore Shard总数一致。只需分裂Shard即可提高任务的并行度。
-   高效：不需要开启Write-Ahead Logs来保证数据不丢失。
-   Exactly-once语义：直接从服务端按需获取数据，任务成功之后再提交Checkpoint。

    由于Spark异常退出或者其他原因导致任务未正常结束，可能会导致部分数据被重复消费。


Direct模式需要依赖ZooKeeper环境，用于临时保存中间状态。同时，必须设置Checkpoint目录。中间状态数据保存在ZooKeeper内对应的Checkpoint目录内。如果任务重启之后希望重新消费，需要在ZooKeeper内删除该目录，并更改消费组名称。

-   参数说明

    |参数|类型|说明|
    |--|--|--|
    |project|String|日志服务Project名称。|
    |logstore|String|日志服务Logstore名称。|
    |consumerGroup|String|消费组名称，仅用于保存消费位置。|
    |endpoint|String|日志服务Project所在地域的Endpoint，详情请参见[服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)。|
    |accessKeyId|String|访问日志服务的Access Key ID。|
    |accessKeySecret|String|访问日志服务的Access Key Secret。|
    |zkAddress|String|ZooKeeper的连接地址。|

-   限流配置

    Spark Streaming流式消费是将数据分成微小的批次进行处理，因此日志服务开始消费时，需预知每个批次的边界，即每个批次需要获取的数据条数。

    日志服务底层的存储模型以LogGroup为单位，正常情况下每个LogGroup对应一次写入请求，例如一次写入请求可能包含数千条日志，这些日志作为一个LogGroup进行存储和消费。而通过WebTracking方式写入日志时，每次写入请求仅包含一条日志，即一个LogGroup只有一条日志。为了能够应对不同写入场景的消费需求，SDK提供如下两个参数进行限流配置。

    |参数|说明|默认值|
    |--|--|---|
    |spark.loghub.batchGet.step|限制单次消费请求获取的最大LogGroup个数。|100|
    |spark.streaming.loghub.maxRatePerShard|限制单批次内每个Shard消费的日志条数。|10000|

    通过spark.streaming.loghub.maxRatePerShard可指定每个批次每个Shard期望消费的最大日志条数。Spark SDK的实现原理是每次从服务端获取spark.loghub.batchGet.step中的LogGroup个数并累计其中的日志条数，直到达到或超过spark.streaming.loghub.maxRatePerShard。因此spark.streaming.loghub.maxRatePerShard并非一个精确控制单批次消费日志条数的参数，实际上每个批次消费的日志条数与spark.loghub.batchGet.step以及每个LogGroup中包含的日志条数相关。

-   示例

    ```
    import com.aliyun.openservices.loghub.client.config.LogHubCursorPosition
    import org.apache.spark.SparkConf
    import org.apache.spark.streaming.{Milliseconds, StreamingContext}
    import org.apache.spark.streaming.aliyun.logservice.{CanCommitOffsets, LoghubUtils}
    
    object TestDirectLoghub {
      def main(args: Array[String]): Unit = {
        if (args.length < 7) {
          System.err.println(
            """Usage: TestDirectLoghub <project> <logstore> <loghub group name> <endpoint>
              |         <access key id> <access key secret> <batch interval seconds> <zookeeper host:port=localhost:2181>
            """.stripMargin)
          System.exit(1)
        }
    
        val project = args(0)
        val logstore = args(1)
        val consumerGroup = args(2)
        val endpoint = args(3)
        val accessKeyId = args(4)
        val accessKeySecret = args(5)
        val batchInterval = Milliseconds(args(6).toInt * 1000)
        val zkAddress = if (args.length >= 8) args(7) else "localhost:2181"
    
        def functionToCreateContext(): StreamingContext = {
          val conf = new SparkConf().setAppName("Test Direct Loghub")
          val ssc = new StreamingContext(conf, batchInterval)
          val zkParas = Map("zookeeper.connect" -> zkAddress,
            "enable.auto.commit" -> "false")
          val loghubStream = LoghubUtils.createDirectStream(
            ssc,
            project,
            logStore,
            consumerGroup,
            accessKeyId,
            accessKeySecret,
            endpoint,
            zkParas,
            LogHubCursorPosition.END_CURSOR)
    
          loghubStream.checkpoint(batchInterval).foreachRDD(rdd => {
            println(s"count by key: ${rdd.map(s => {
              s.sorted
              (s.length, s)
            }).countByKey().size}")
            loghubStream.asInstanceOf[CanCommitOffsets].commitAsync()
          })
          ssc.checkpoint("hdfs:///tmp/spark/streaming") // set checkpoint directory
          ssc
        }
    
        val ssc = StreamingContext.getOrCreate("hdfs:///tmp/spark/streaming", functionToCreateContext _)
        ssc.start()
        ssc.awaitTermination()
      }
    }
    ```


更多信息请参见[GitHub](https://github.com/aliyun/aliyun-emapreduce-sdk/tree/master-2.x/emr-logservice/src/main)。

