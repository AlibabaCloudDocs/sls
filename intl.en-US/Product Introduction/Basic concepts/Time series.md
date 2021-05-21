# Time series

Log Service stores metric data as time series in Metricstores. Log Service uses the time series data model that is defined by Prometheus. For more information about the time series data model, see [DATA MODEL](https://prometheus.io/docs/concepts/data_model/) in the Prometheus documentation. Each time series consists of samples with the same identifier.

## Metric identifier

Each time series has a unique identifier that consists of a metric name and a label.

Metric names are of the string type and must match the \[a-zA-Z\_:\]\[a-zA-Z0-9\_:\]\* regular expression. In most cases, a metric name indicates a description of the time series. For example, http\_request\_total indicates that each sample of the time series represents the total number of received HTTP requests.

Labels are key-value pairs that indicate the attributes of the time series. Label keys must match the \[a-zA-Z\_\]\[a-zA-Z0-9\_\]\* regular expression. Label values can contain any characters except vertical bars \(\|\). For example, the value of the method key may be POST and the value of the URL key may be /api/v1/get.

## Samples

A sample indicates the value of a metric at a point in time. Each sample consists of a timestamp and a value. Timestamps are accurate to the nanosecond and values are of the double type.

## Format

You must write time series data to Log Service in the Protocol Buffer \(Protobuf\) format, which is the same as log data. For more information, see [Data encoding](/intl.en-US/Developer Guide/API Reference/Common resources/Data encoding.md). The time series identifier and samples are contained in the content field. The following table describes the related subfields.

|Key|Description|Example|
|---|-----------|-------|
|\_\_name\_\_|The metric name of the time series.|nginx\_ingress\_controller\_response\_size|
|\_\_labels\_\_|The labels of the time series. Format: \{key\}\#$\#\{value\}\|\{key\}\#$\#\{value\}\|\{key\}\#$\#\{value\}.**Note:** The labels must be sorted by key in alphabetical order.

|app\#$\#ingress-nginx\|controller\_class\#$\#nginx\|controller\_namespace\#$\#kube-system\|controller\_pod\#$\#nginx-ingress-controller-589877c6b7-hw9cj|
|\_\_time\_nano\_\_|The timestamp of a sample. It is accurate to the nanosecond.|1585727297293000000|
|\_\_value\_\_|The value of a sample.|36.0|

