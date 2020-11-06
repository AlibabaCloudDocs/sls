# Use consumer groups to consume log data

Consumer groups allow you to focus on the business logic during log data consumption. Compared with SDKs, consumer groups are easy to use. You do not need to consider factors such as Log Service implementation, load balancing among consumers, and failovers that may occur when you use SDKs to consume log data.

## Terms

|Term|Description|
|:---|:----------|
|consumer group|A consumer group consists of multiple consumers. Each consumer in a consumer group consumes different data in a Logstore. You can create a maximum of 10 consumer groups for a Logstore.|
|consumer|The consumers in a consumer group consume data from specified data sources. The name of each consumer in a consumer group must be unique.|

A Logstore has multiple shards. A consumer library allocates shards to consumers in a consumer group based on the following principles:

-   Each shard can be allocated to only one consumer.
-   Each consumer can consume data from multiple shards.

After a new consumer joins a consumer group, shards allocated to the consumer group are reallocated to each consumer for load balancing. The reallocation is based on the preceding principles and cannot be viewed by users.

A consumer library stores checkpoints. This allows consumers to resume consumption from a breakpoint and avoid repetitive consumption after a program fault is resolved.

## Procedure

You can use Java, Python, or Go to create consumers and consume data. The following procedure uses Java as an example:

1.  Add Maven dependencies.

    ```
    <dependency>
      <groupId>com.google.protobuf</groupId>
      <artifactId>protobuf-java</artifactId>
      <version>2.5.0</version>
    </dependency>
    <dependency>
      <groupId>com.aliyun.openservices</groupId>
      <artifactId>loghub-client-lib</artifactId>
      <version>0.6.16</version>
    </dependency>
    ```

2.  Create a main.Java file.

    ```
    import com.aliyun.openservices.loghub.client.ClientWorker;
    import com.aliyun.openservices.loghub.client.config.LogHubConfig;
    import com.aliyun.openservices.loghub.client.exceptions.LogHubClientWorkerException;
    
    public class Main {
        // The endpoint of Log Service.
        private static String sEndpoint = "cn-hangzhou.log.aliyuncs.com";
        // The name of a Log Service project.
        private static String sProject = "ali-cn-hangzhou-sls-admin";
        // The name of a Logstore.
        private static String sLogstore = "sls_operation_log";
        // The name of a consumer group.
        private static String sConsumerGroup = "consumerGroupX";
        // The AccessKey pair that is used to access Log Service.
        private static String sAccessKeyId = "";
        private static String sAccessKey = "";
    
        public static void main(String[] args) throws LogHubClientWorkerException, InterruptedException {
            // consumer_1 is the consumer name. The name of each consumer in a consumer group must be unique. When different consumers start multiple processes on multiple servers to consume the data of a Logstore, you can use a server IP address to identify a consumer. The maxFetchLogGroupSize parameter indicates the maximum number of log groups that are retrieved from the server at a time. Valid values: 1 to 1000. You can use the default value or specify a value based on your needs.
            LogHubConfig config = new LogHubConfig(sConsumerGroup, "consumer_1", sEndpoint, sProject, sLogstore, sAccessKeyId, sAccessKey, LogHubConfig.ConsumePosition.BEGIN_CURSOR);
            ClientWorker worker = new ClientWorker(new SampleLogHubProcessorFactory(), config);
            Thread thread = new Thread(worker);
            // After the thread is executed, the ClientWorker instance automatically runs and extends the Runnable interface.
            thread.start();
            Thread.sleep(60 * 60 * 1000);
            // The shutdown function of the ClientWorker instance is called to exit the consumption instance. The associated thread is automatically stopped.
            worker.shutdown();
            // Multiple asynchronous tasks are generated when the ClientWorker instance is running. We recommend that you wait for 30 seconds to ensure that all running tasks exit after shutdown.
            Thread.sleep(30 * 1000);
        }
    }
    ```

3.  Create a file named SampleLogHubProcessor.java.

    ```
    import com.aliyun.openservices.log.common.FastLog;
    import com.aliyun.openservices.log.common.FastLogContent;
    import com.aliyun.openservices.log.common.FastLogGroup;
    import com.aliyun.openservices.log.common.FastLogTag;
    import com.aliyun.openservices.log.common.LogGroupData;
    import com.aliyun.openservices.loghub.client.ILogHubCheckPointTracker;
    import com.aliyun.openservices.loghub.client.exceptions.LogHubCheckPointException;
    import com.aliyun.openservices.loghub.client.interfaces.ILogHubProcessor;
    import com.aliyun.openservices.loghub.client.interfaces.ILogHubProcessorFactory;
    
    import java.util.List;
    
    public class SampleLogHubProcessor implements ILogHubProcessor {
        private int shardId;
        // Record the last persistent checkpoint time.
        private long mLastCheckTime = 0;
    
        public void initialize(int shardId) {
            this.shardId = shardId;
        }
    
        // The main logic of data consumption. All exceptions must be captured and cannot be thrown out.
        public String process(List<LogGroupData> logGroups,
                              ILogHubCheckPointTracker checkPointTracker) {
            // Display the retrieved data.
            for (LogGroupData logGroup : logGroups) {
                FastLogGroup flg = logGroup.GetFastLogGroup();
                System.out.println(String.format("\tcategory\t:\t%s\n\tsource\t:\t%s\n\ttopic\t:\t%s\n\tmachineUUID\t:\t%s",
                        flg.getCategory(), flg.getSource(), flg.getTopic(), flg.getMachineUUID()));
                System.out.println("Tags");
                for (int tagIdx = 0; tagIdx < flg.getLogTagsCount(); ++tagIdx) {
                    FastLogTag logtag = flg.getLogTags(tagIdx);
                    System.out.println(String.format("\t%s\t:\t%s", logtag.getKey(), logtag.getValue()));
                }
                for (int lIdx = 0; lIdx < flg.getLogsCount(); ++lIdx) {
                    FastLog log = flg.getLogs(lIdx);
                    System.out.println("--------\nLog: " + lIdx + ", time: " + log.getTime() + ", GetContentCount: " + log.getContentsCount());
                    for (int cIdx = 0; cIdx < log.getContentsCount(); ++cIdx) {
                        FastLogContent content = log.getContents(cIdx);
                        System.out.println(content.getKey() + "\t:\t" + content.getValue());
                    }
                }
            }
            long curTime = System.currentTimeMillis();
            // Write checkpoints to the server every 30 seconds. If a ClientWorker instance does not respond within 30 seconds, the newly started ClientWorker instance continues to consume data from the last checkpoint. A small amount of data may be repeatedly consumed.
            if (curTime - mLastCheckTime > 30 * 1000) {
                try {
                    // If the parameter is set to true, checkpoints are immediately synchronized to the server. If the parameter is set to false, checkpoints are locally cached. The default synchronization interval of checkpoints is 60 seconds.
                    checkPointTracker.saveCheckPoint(true);
                } catch (LogHubCheckPointException e) {
                    e.printStackTrace();
                }
                mLastCheckTime = curTime;
            }
            return null;
        }
    
        // The ClientWorker instance calls this function upon exit. You can perform a cleanup.
        public void shutdown(ILogHubCheckPointTracker checkPointTracker) {
            // Save checkpoints to the server.
            try {
                checkPointTracker.saveCheckPoint(true);
            } catch (LogHubCheckPointException e) {
                e.printStackTrace();
            }
        }
    }
    
    class SampleLogHubProcessorFactory implements ILogHubProcessorFactory {
        public ILogHubProcessor generatorProcessor() {
            // Generate a consumption instance.
            return new SampleLogHubProcessor();
        }
    }
    ```

    For more information, visit [Java](https://github.com/aliyun/aliyun-log-consumer-java), [Python](https://github.com/aliyun/aliyun-log-python-sdk), and [Go](https://github.com/aliyun/aliyun-log-go-sdk/blob/master/consumer/README.md).


## View consumer group status

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the project.

3.  On the page that appears, choose **Log Management** \> **Logstores**. Find the Logstore, and then choose **![Expand the data consumption node](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2033359951/p53702.png)** \> **Data Consumption**.

4.  Click the consumer group whose data consumption progress you want to view. The data consumption progress of each shard in the Logstore is displayed.


You can also use the API of Log Service or an SDK to view the consumption progress. The following code uses the Java SDK as an example:

```
package test;
import java.util.ArrayList;
import com.aliyun.openservices.log.Client;
import com.aliyun.openservices.log.common.Consts.CursorMode;
import com.aliyun.openservices.log.common.ConsumerGroup;
import com.aliyun.openservices.log.common.ConsumerGroupShardCheckPoint;
import com.aliyun.openservices.log.exception.LogException;
public class ConsumerGroupTest {
    static String endpoint = "";
    static String project = "";
    static String logstore = "";
    static String accesskeyId = "";
    static String accesskey = "";
    public static void main(String[] args) throws LogException {
        Client client = new Client(endpoint, accesskeyId, accesskey);
        // Obtain all consumer groups created for the Logstore. If no consumer group exists, an empty string is returned.
        List<ConsumerGroup> consumerGroups = client.ListConsumerGroup(project, logstore).GetConsumerGroups();
        for(ConsumerGroup c: consumerGroups){
            // Display the properties of consumer groups, including the name, heartbeat timeout, and whether the consumer group consumes data in order.
            System.out.println("Name: " + c.getConsumerGroupName());
            System.out.println("Heartbeat timeout: " + c.getTimeout());
            System.out.println("Consumption in order: " + c.isInOrder());
            for(ConsumerGroupShardCheckPoint cp: client.GetCheckPoint(project, logstore, c.getConsumerGroupName()).GetCheckPoints()){
                System.out.println("shard: " + cp.getShard());
                Return the consumption time. The time is a LONG integer that is accurate to milliseconds.
                System.out.println("The last time when data is consumed: " + cp.getUpdateTime());
                System.out.println("Consumer name: " + cp.getConsumer());
                String consumerPrg = "";
                if(cp.getCheckPoint().isEmpty())
                    consumerPrg = "Consumption not started";
                else{
                    //The UNIX timestamp. Unit: seconds. Format the output value of the timestamp.
                    try{
                        int prg = client.GetPrevCursorTime(project, logstore, cp.getShard(), cp.getCheckPoint()).GetCursorTime();
                        consumerPrg = "" + prg;
                    }
                    catch(LogException e){
                        if(e.GetErrorCode() == "InvalidCursor")
                            consumerPrg = "Invalid. The previous consumption time has exceeded the retention period of the data in the Logstore.";
                        else{
                            //internal server error
                            throw e;
                        }
                    }
                }
                System.out.println("Consumption progress: " + consumerPrg);
                String endCursor = client.GetCursor(project, logstore, cp.getShard(), CursorMode.END).GetCursor();
                int endPrg = 0;
                try{
                    endPrg = client.GetPrevCursorTime(project, logstore, cp.getShard(), endCursor).GetCursorTime();
                }
                catch(LogException e){
                    //do nothing
                }
                //The UNIX timestamp. Unit: seconds. Format the output value of the timestamp.
                System.out.println("The time when the last data entry is received: " + endPrg);
            }
        }
    }
}
```

## References

-   Configure Log4j

    We recommend that you configure Log4j for the consumer program to throw error messages within consumer groups. This improves the troubleshooting efficiency. The following script shows a typical log4j.properties configuration file:

    ```
    log4j.rootLogger = info,stdout
    log4j.appender.stdout = org.apache.log4j.ConsoleAppender
    log4j.appender.stdout.Target = System.out
    log4j.appender.stdout.layout = org.apache.log4j.PatternLayout
    log4j.appender.stdout.layout.ConversionPattern = [%-5p] %d{yyyy-MM-dd HH:mm:ss,SSS} method:%l%n%m%n
    ```

    After Log4j is configured, you may see exceptions similar to the following message when you run the consumer program:

    ```
    [WARN ] 2018-03-14 12:01:52,747 method:com.aliyun.openservices.loghub.client.LogHubConsumer.sampleLogError(LogHubConsumer.java:159)
    com.aliyun.openservices.log.exception.LogException: Invalid loggroup count, (0,1000]
    ```

-   Use a consumer group to consume data that is logged from a certain time point

    ```
    // consumerStartTimeInSeconds indicates that the data generated after the time point is consumed.
    public LogHubConfig(String consumerGroupName, 
                          String consumerName, 
                          String loghubEndPoint,
                          String project, String logStore,
                          String accessId, String accessKey,
                          int consumerStartTimeInSeconds);
    // The position parameter is an enumeration variable. LogHubConfig.ConsumePosition.BEGIN_CURSOR indicates that the consumption starts from the earliest data. LogHubConfig.ConsumePosition.END_CURSOR indicates that the consumption starts from the latest data.
    public LogHubConfig(String consumerGroupName, 
                          String consumerName, 
                          String loghubEndPoint,
                          String project, String logStore,
                          String accessId, String accessKey,
                          ConsumePosition position);
    ```

    **Note:**

    -   You can use different constructors based on your requirements.
    -   If a checkpoint is stored on the server, data consumption starts from this checkpoint.
-   Reset a checkpoint

    ```
    public static void updateCheckpoint() throws Exception {
            Client client = new Client(host, accessId, accessKey);
            long timestamp = Timestamp.valueOf("2017-11-15 00:00:00").getTime() / 1000;
            ListShardResponse response = client.ListShard(new ListShardRequest(project, logStore));
            for (Shard shard : response.GetShards()) {
                int shardId = shard.GetShardId();
                String cursor = client.GetCursor(project, logStore, shardId, timestamp).GetCursor();
                client.UpdateCheckPoint(project, logStore, consumerGroup, shardId, cursor);
            }
        }
    ```


## Authorize a RAM user to access consumer groups

Before you use a RAM user to access consumer groups, you must grant relevant permissions to the RAM user. For more information, see [Grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Create a RAM user and authorize the RAM user to access Log Service.md).

The following table lists the actions that a RAM user can perform.

|Action|Description|Resource|
|:-----|-----------|:-------|
|log:GetCursorOrData \([GetCursor](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/GetCursor.md) and [PullLogs](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/PullLogs.md)\)|Obtains a cursor based on the log receiving time.|acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/logstore/$\{logstoreName\}|
|[log:CreateConsumerGroup](/intl.en-US/Developer Guide/API Reference/Consumer group interfaces/CreateConsumerGroup.md)|Creates a consumer group for a specified Logstore.|acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/logstore/$\{logstoreName\}/consumergroup/\*|
|[log:ListConsumerGroup](/intl.en-US/Developer Guide/API Reference/Consumer group interfaces/ListConsumerGroup.md)|Lists all consumer groups of a specified Logstore.|acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/logstore/$\{logstoreName\}/consumergroup/\*|
|[log:UpdateCheckPoint](/intl.en-US/Developer Guide/API Reference/Consumer group interfaces/UpdateCheckPoint.md)|Updates the consumption checkpoint in a shard of a specified consumer group.|acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/logstore/$\{logstoreName\}/consumergroup/$\{consumerGroupName\}|
|[log:ConsumerGroupHeartBeat](/intl.en-US/Developer Guide/API Reference/Consumer group interfaces/HeartBeat.md)|Sends a heartbeat packet to Log Service for a consumer.|acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/logstore/$\{logstoreName\}/consumergroup/$\{consumerGroupName\}|
|[log:UpdateConsumerGroup](/intl.en-US/Developer Guide/API Reference/Consumer group interfaces/UpdateConsumerGroup.md)|Modifies the attributes of a specified consumer group.|acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/logstore/$\{logstoreName\}/consumergroup/$\{consumerGroupName\}|
|[log:ConsumerGroupUpdateCheckPoint](/intl.en-US/Developer Guide/API Reference/Consumer group interfaces/GetCheckPoint.md)|Retrieves the consumption checkpoints in one or all shards of a specified consumer group.|acs:log:$\{regionName\}:$\{projectOwnerAliUid\}:project/$\{projectName\}/logstore/$\{logstoreName\}/consumergroup/$\{consumerGroupName\}|

For example, a project named project-test resides in the China \(Hangzhou\) region. The ID of the Alibaba Cloud account to which the project belongs is 1234567. The name of the Logstore to be consumed is logstore-test and the consumer group name is consumergroup-test. To allow a RAM user to access the consumer group, you must grant the following permissions to the RAM user:

```
{
  "Version": "1",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "log:GetCursorOrData"
      ],
      "Resource": "acs:log:cn-hangzhou:1234567:project/project-test/logstore/logstore-test"
    },
    {
      "Effect": "Allow",
      "Action": [
        "log:CreateConsumerGroup",
        "log:ListConsumerGroup"
      ],
      "Resource": "acs:log:cn-hangzhou:1234567:project/project-test/logstore/logstore-test/consumergroup/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "log:ConsumerGroupUpdateCheckPoint",
        "log:ConsumerGroupHeartBeat",
        "log:UpdateConsumerGroup",
        "log:GetConsumerGroupCheckPoint"
      ],
      "Resource": "acs:log:cn-hangzhou:1234567:project/project-test/logstore/logstore-test/consumergroup/consumergroup-test"
    }
  ]
}
```

