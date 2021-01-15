# 通过SDK写入时序数据

日志服务支持通过SDK写入时序数据，本文列举了Java、Golang和Python语言的SDK demo。

**说明：**

-   使用SDK写入时序数据时，需遵循[时序数据格式](/cn.zh-CN/时序存储/基本概念/时序数据.md)。
-   尽可能使用Producer Library发送数据，目前支持ProducerLibrary的语言有[Java](/cn.zh-CN/数据采集/其他采集方式/SDK采集/Producer Library.md)、[Golang](/cn.zh-CN/数据采集/其他采集方式/SDK采集/Golang Producer Library.md)和[C](/cn.zh-CN/数据采集/其他采集方式/SDK采集/C Producer Library.md)。使用其他语言时，可以将多条数据放到同一个LogGroup中发送，尽可能减少网络请求次数。

## Java SDK示例

```
    /**
     * @param metricName: the metric name, eg: http_requests_count
     * @param labels: labels map, eg: {'idc': 'idc1', 'ip': '1.2.3.4', 'hostname': 'appserver1'}
     * @param value: double value, eg: 1.234
     * @return LogItem
     */
    public static LogItem buildLogItem(String metricName, Map<String, String> labels, double value) {
        String labelsKey = "__labels__";
        String timeKey = "__time_nano__";
        String valueKey = "__value__";
        String nameKey = "__name__";
        LogItem logItem = new LogItem();

        int timeInSec = (int)(System.currentTimeMillis() / 1000);
        logItem.SetTime(timeInSec);

        logItem.PushBack(timeKey, String.valueOf(timeInSec));
        logItem.PushBack(nameKey, metricName);
        logItem.PushBack(valueKey, String.valueOf(value));

        // 按照字典序对labels排序, 如果您的labels已排序, 请忽略此步骤。
        TreeMap<String, String> sortedLabels = new TreeMap<>(labels);
        StringBuilder labelsBuilder = new StringBuilder();

        Iterator<Entry<String, String>> it = sortedLabels.entrySet().iterator();
        while (it.hasNext()) {
            Entry<String, String> entry = it.next();
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
```

## Python SDK示例

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

## Go SDK示例

```
package main

import (
    "fmt"
    "github.com/gogo/protobuf/proto"
    "sort"
    "strconv"
    "time"

    sls "github.com/aliyun/aliyun-log-go-sdk"
)

func buildLogItem(metricName string, labels map[string]string, value float64) *sls.Log {

    now := uint32(time.Now().Unix())
    log := &sls.Log{Time: proto.Uint32(now)}

    contents := []*sls.LogContent{}
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
            
```

