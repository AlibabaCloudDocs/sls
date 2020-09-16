# （下线）View the status of a consumer group

A consumer group consumes data in real time in an advanced mode. In this mode, automatic load balancing is provided for Logstore data consumed by multiple consumption instances. For more information, see [Use a consumer group to consume logs](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Consumption by consumer groups/Use consumer groups to consume log data.md). Spark Streaming and Storm can process data for consumer groups.

## View consumption progress in the console

1.  Log on to the [Log Service console](https://sls.console.aliyun.com), and then click the target project name.
2.  Click the ![Expand the data consumption node](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2033359951/p53702.png) icon. Choose **Logstore** \> **Data Consumption**.
3.  Click the consumer group whose data consumption progress you want to view. The data consumption progress of each shard in the Logstore is displayed.

    ![](../images/p5788.png "Consumption status")

    As shown in the preceding figure, the Logstore has four shards, which correspond to four consumers. The last consumption time of each consumer is displayed in the second column. You can use the last consumption time to determine whether the current data processing capacity can catch up with data generation. If the data processing takes longer than expected, data consumption cannot catch up with data generation. In this case, we recommend that you increase the number of consumers.


## Use APIs or SDKs to view the data consumption progress

The following example uses the Java SDK to describe how to call API operations to view the data consumption progress.

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
        //Obtain all consumer groups in this Logstore. If no consumer group exists, the length of consumerGroups is 0.
        ArrayList<ConsumerGroup>  consumerGroups;
        try{
            consumerGroups = client.ListConsumerGroup(project, logstore).GetConsumerGroups();
        }
        catch(LogException e){
            if(e.GetErrorCode() == "LogStoreNotExist")
                System.out.println("this logstore does not have any consumer group");
            else{
                //internal server error branch
            }
            return;
        }
        for(ConsumerGroup c: consumerGroups){
            //Print consumer group properties, including the name, heartbeat timeout, and whether data is consumed in order.
            System.out.println("Name: " + c.getConsumerGroupName());
            System.out.println("Heartbeat timeout: " + c.getTimeout());
            System.out.println("Consumption in order: " + c.isInOrder());
            for(ConsumerGroupShardCheckPoint cp: client.GetCheckPoint(project, logstore, c.getConsumerGroupName()).GetCheckPoints()){
                System.out.println("shard: " + cp.getShard());
                //Format the returned time. The time is a long integer that is accurate to milliseconds.
                System.out.println("The last time when data is consumed: " + cp.getUpdateTime());
                System.out.println("Consumer name: " + cp.getConsumer());
                String consumerPrg = "";
                if(cp.getCheckPoint().isEmpty())
                    consumerPrg = "Consumption not started";
                else{
                    //The Unix timestamp, in seconds. Format the output value of the timestamp.
                    try{
                        int prg = client.GetPrevCursorTime(project, logstore, cp.getShard(), cp.getCheckPoint()).GetCursorTime();
                        consumerPrg = "" + prg;
                    }
                    catch(LogException e){
                        if(e.GetErrorCode() == "InvalidCursor")
                            consumerPrg = "Invalid. The previous consumption time has exceeded the lifecycle of the data in the Logstore.";
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
                //The Unix timestamp, in seconds. Format the output value of the timestamp.
                System.out.println("The time when the last data entry is received: " + endPrg);
            }
        }
    }
}
```

