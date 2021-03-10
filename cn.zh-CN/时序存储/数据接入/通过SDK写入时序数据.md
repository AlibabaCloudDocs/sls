# 通过SDK写入时序数据

日志服务支持通过SDK写入时序数据，本文列举了Java、Golang和Python语言的SDK demo。

**说明：**

-   使用SDK写入时序数据时，需遵循[时序数据格式](/cn.zh-CN/时序存储/基本概念/时序数据.md)。
-   尽可能使用Producer Library发送数据，目前支持ProducerLibrary的语言有[Java](/cn.zh-CN/数据采集/其他采集方式/SDK采集/Producer Library.md)、[Golang](/cn.zh-CN/数据采集/其他采集方式/SDK采集/Golang Producer Library.md)和[C](/cn.zh-CN/数据采集/其他采集方式/SDK采集/C Producer Library.md)。使用其他语言时，可以将多条数据放到同一个LogGroup中发送，尽可能减少网络请求次数。
-   \_\_time\_nano\_\_字段支持秒、毫秒、微秒和纳秒。

## Java SDK示例

```
import com.aliyun.openservices.aliyun.log.producer.Callback;
import com.aliyun.openservices.aliyun.log.producer.LogProducer;
import com.aliyun.openservices.aliyun.log.producer.Producer;
import com.aliyun.openservices.aliyun.log.producer.ProducerConfig;
import com.aliyun.openservices.aliyun.log.producer.ProjectConfig;
import com.aliyun.openservices.aliyun.log.producer.Result;
import com.aliyun.openservices.aliyun.log.producer.errors.ProducerException;
import com.aliyun.openservices.log.common.LogItem;

import java.util.*;
import java.util.concurrent.CountDownLatch;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.atomic.AtomicInteger;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Main {

  private static final Logger LOGGER = LoggerFactory.getLogger(Main.class);

  private static final Random random = new Random();

  public static void main(String[] args) throws InterruptedException {
    final String project = "";
    final String logStore = "";
    final String endpoint = "https://cn-hangzhou.log.aliyuncs.com";
    final String accessKeyId = "";
    final String accessKeySecret = "";
    int sendThreadCount = 8;
    final int times = 10;
    LOGGER.info(
        "project={}, logStore={}, endpoint={}, sendThreadCount={}, times={}",
        project,
        logStore,
        endpoint,
        sendThreadCount,
        times);
    ExecutorService executorService = Executors.newFixedThreadPool(sendThreadCount);
    ProducerConfig producerConfig = new ProducerConfig();
    producerConfig.setBatchSizeThresholdInBytes(3 * 1024 * 1024);
    producerConfig.setBatchCountThreshold(40960);

    final Producer producer = new LogProducer(producerConfig);
    producer.putProjectConfig(new ProjectConfig(project, endpoint, accessKeyId, accessKeySecret));

    final AtomicInteger successCount = new AtomicInteger(0);
    final CountDownLatch latch = new CountDownLatch(sendThreadCount);
    LOGGER.info("Test started.");
    long t1 = System.currentTimeMillis();
    Map labels = new HashMap<>();
    labels.put("test_k", "test_v");
    for (int i = 0; i < sendThreadCount; ++i) {
      executorService.submit(
          new Runnable() {
            @Override
            public void run() {
              try {
                for (int i = 0; i < times; ++i) {
                  int r = random.nextInt(times);
                  producer.send(
                      project,
                      logStore,
                      generateTopic(r),
                      generateSource(r),
                      buildLogItem("test_metric", labels, i),
                      new Callback() {
                        @Override
                        public void onCompletion(Result result) {
                          if (result.isSuccessful()) {
                            successCount.incrementAndGet();
                          }
                        }
                      });
                }
              } catch (Exception e) {
                LOGGER.error("Failed to send log, e=", e);
              } finally {
                latch.countDown();
              }
            }
          });
    }
    latch.await();
    while (true) {
      if (successCount.get() == sendThreadCount * times) {
        break;
      }
      Thread.sleep(100);
    }
    long t2 = System.currentTimeMillis();
    LOGGER.info("Test end.");
    LOGGER.info("======Summary======");
    LOGGER.info("Total count " + sendThreadCount * times + ".");
    long timeCost = t2 - t1;
    LOGGER.info("Time cost " + timeCost + " millis");
    try {
      producer.close();
    } catch (ProducerException e) {
      LOGGER.error("Failed to close producer, e=", e);
    }
    executorService.shutdown();
  }

  private static String generateTopic(int r) {
    return "topic-" + r % 5;
  }

  private static String generateSource(int r) {
    return "source-" + r % 10;
  }

  /**
   * @param metricName: the metric name, eg: http_requests_count
   * @param labels: labels map, eg: {'idc': 'idc1', 'ip': '1.2.3.4', 'hostname': 'appserver1'}
   * @param value: double value, eg: 1.234
   * @return LogItem
   */
  public static LogItem buildLogItem(String metricName, Map labels, double value) {
    String labelsKey = "__labels__";
    String timeKey = "__time_nano__";
    String valueKey = "__value__";
    String nameKey = "__name__";
    LogItem logItem = new LogItem();

    int timeInSec = (int)(System.currentTimeMillis() / 1000);
    logItem.SetTime(timeInSec);

    logItem.PushBack(timeKey, String.valueOf(timeInSec)+"000000");
    logItem.PushBack(nameKey, metricName);
    logItem.PushBack(valueKey, String.valueOf(value));

    // 按照字典序对labels排序, 如果您的labels已排序, 请忽略此步骤。
    TreeMap sortedLabels = new TreeMap<>(labels);
    StringBuilder labelsBuilder = new StringBuilder();

    Iterator> it = sortedLabels.entrySet().iterator();
    while (it.hasNext()) {
      Map.Entry entry = it.next();
      labelsBuilder.append(entry.getKey());
      labelsBuilder.append("#$#");
      labelsBuilder.append(entry.getValue());
      if (it.hasNext()) {
        labelsBuilder.append("|");
      }
    }

    logItem.PushBack(labelsKey, labelsBuilder.toString());
    return logItem;
  }
}
```

## Python SDK示例

```
# encoding: utf-8
import time

from aliyun.log import *

def build_log_item(metric_name, labels, value):
    """
    build log item
    :param metric_name: the metric name, eg: http_requests_count
    :param labels: dict labels, eg: {'idc': 'idc1', 'ip': '1.2.3.4', 'hostname': 'appserver1'}
    :param value: double value, eg: 1.234
    :return: LogItem
    """
    sorted_labels = sorted(labels.items())
    log_item = LogItem()
    now = int(time.time())
    log_item.set_time(now)
    contents = [('__time_nano__', str(now)), ('__name__', metric_name), ('__value__', str(value))]

    labels_str = ''
    for i, kv in enumerate(sorted_labels):
        labels_str += kv[0]
        labels_str += '#$#'
        labels_str += kv[1]
        if i < len(sorted_labels) - 1:
            labels_str += '|'

    contents.append(('__labels__', labels_str))
    log_item.set_contents(contents)
    return log_item

def main():
    endpoint = 'cn-hangzhou.log.aliyuncs.com'
    accessKeyId = ''
    accessKey = ''
    project = ''
    logstore = ''

    client = LogClient(endpoint, accessKeyId, accessKey)
    item1 = build_log_item('test1', {'k1': 'v1'}, 1)
    item2 = build_log_item('test2', {'k2': 'v2'}, 2)
    item3 = build_log_item('test3', {'k3': 'v3'}, 3)
    items = [item1, item2, item3]
    res = client.put_logs(PutLogsRequest(project=project, logstore=logstore, logitems=items))
    res.log_print()

if __name__ == '__main__':
    main()
```

## GO SDK示例

```
package main

import (
   "fmt"
   sls "github.com/aliyun/aliyun-log-go-sdk"
   "github.com/aliyun/aliyun-log-go-sdk/producer"
   "github.com/golang/protobuf/proto"
   "os"
   "os/signal"
   "sort"
   "strconv"
   "sync"
   "time"
)

func buildLogItem(metricName string, labels map[string]string, value float64) *sls.Log {

   now := uint32(time.Now().Unix())
   log := &sls.Log{Time: proto.Uint32(now)}

   var contents []*sls.LogContent
   contents = append(contents, &sls.LogContent{
      Key:   proto.String("__time_nano__"),
      Value: proto.String(strconv.FormatInt(int64(now), 10)),
   })
   contents = append(contents, &sls.LogContent{
      Key:   proto.String("__name__"),
      Value: proto.String(metricName),
   })
   contents = append(contents, &sls.LogContent{
      Key:   proto.String("__value__"),
      Value: proto.String(strconv.FormatFloat(value, 'f', 6, 64)),
   })

   keys := make([]string, 0, len(labels))
   for k := range labels {
      keys = append(keys, k)
   }
   sort.Strings(keys)

   labelsStr := ""
   for i, k := range keys {
      labelsStr += k
      labelsStr += "#$#"
      labelsStr += labels[k]
      if i < len(keys) - 1 {
         labelsStr += "|"
      }
   }

   contents = append(contents, &sls.LogContent{Key: proto.String("__labels__"), Value: proto.String(labelsStr)})
   log.Contents = contents
   return log

}

func main() {
   project := ""
   logstore := ""

   producerConfig := producer.GetDefaultProducerConfig()
   producerConfig.Endpoint = "https://cn-hangzhou.log.aliyuncs.com"
   producerConfig.AccessKeyID = ""
   producerConfig.AccessKeySecret = ""
   producerInstance := producer.InitProducer(producerConfig)
   ch := make(chan os.Signal)
   signal.Notify(ch)
   producerInstance.Start()
   var m sync.WaitGroup
   for i := 0; i < 10; i++ {
      m.Add(1)
      go func() {
         defer m.Done()
         for i := 0; i < 1000; i++ {
            // GenerateLog  is producer's function for generating SLS format logs
            // GenerateLog has low performance, and native Log interface is the best choice for high performance.
            log := buildLogItem("test_metric", map[string]string{"test_k":"test_v"}, float64(i))
            err := producerInstance.SendLog(project, logstore, "topic", "127.0.0.1", log)
            if err != nil {
               fmt.Println(err)
            }
         }
      }()
   }
   m.Wait()
   fmt.Println("Send completion")
   if _, ok := <-ch; ok {
      fmt.Println("Get the shutdown signal and start to shut down")
      producerInstance.Close(60000)
   }
}
       
```

