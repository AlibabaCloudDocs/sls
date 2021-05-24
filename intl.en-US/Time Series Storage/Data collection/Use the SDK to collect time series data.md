# Use the SDK to collect time series data

This topic describes how to use the Log Service SDK for Java, Golang, or Python to collect time series data.

**Note:**

-   The time series data to be collected must follow the [required format](/intl.en-US/Product Introduction/Basic concepts/Time series.md).
-   We recommend that you use a Producer library to transfer data whenever possible. The Producer libraries for [Java](/intl.en-US/Data Collection/Other collection methods/SDK collection/Producer Library.md), [Golang](/intl.en-US/Data Collection/Other collection methods/SDK collection/Golang Producer Library.md), and [C](/intl.en-US/Data Collection/Other collection methods/SDK collection/C Producer Library.md) are provided. If you use other programming languages, put multiple data records into a log group and transfer data by log group to reduce network requests.

## Use the SDK for Java to collect time series data

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

## Use the SDK for Python to collect time series data

```
from aliyun.log import *


def build_log_item(metric_name, labels, value):
    """
    build log item
    :param metric_name: the metric name, eg: http_requests_count
    :param labels: dict labels, eg: {'idc': 'idc1', 'ip': '1.2.3.4', 'hostname': 'appserver1''}
    :param value: double value, eg: 1.234
    :return: LogItem
    """
    sorted_labels = sorted(labels.items())
    log_item = LogItem()
    now = int(time.time())
    log_item.set_time(now)
    contents = [('__time_nano__', now), ('__name__', metric_name), ('__value__', value)]

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
```

## Use the SDK for Golang to collect time series data

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

