# Use the SDK to collect time series data

This topic describes how to use the Log Service SDK for Java, Golang, or Python to collect time series data.

**Note:**

-   The time series data to be collected must follow the [required format](/intl.en-US/Developer Guide/SDK Reference/Overview.md).
-   We recommend that you use a Producer library to transfer data whenever possible. The Producer libraries for [Java](/intl.en-US/Data Collection/Other collection methods/SDK collection/Producer Library.md), [Golang](/intl.en-US/Data Collection/Other collection methods/SDK collection/Golang Producer Library.md), and [C](/intl.en-US/Data Collection/Other collection methods/SDK collection/C Producer Library.md) are provided. If you use other programming languages, put multiple data records into a log group and transfer data by log group to reduce network requests.

## Use the SDK for Java to collect time series data

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

        // Sort the labels in the lexicographic order. If the labels are already sorted, skip this step.
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

