# Cloud Monitor

You can use Cloud Monitor to monitor Log Service. The monitoring metrics include write traffic, overall QPS, and service status. You can also create alert rules to monitor exceptions that occurred during log collection and shard usage.

If you use a RAM user to view monitoring metrics, the RAM user must be granted the read-only permissions or read/write permissions on Cloud Monitor. You can use an Alibaba Cloud account to authorize the RAM user. For more information, see [Grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Create a RAM user and authorize the RAM user to access Log Service.md).

## View monitoring metrics

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Choose **Log Management** \> **Logstores**. On the Logstores tab, find the target Logstore, and choose **![Management icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9484688951/p52166.png)** \> **Monitor**. In the Cloud Monitor console, view the monitoring metrics.


## Monitoring metrics

|Monitoring metric|Description|
|-----------------|-----------|
|Write Traffic|The size of the data that is written in real time to the Logstore per minute.|
|Size of Raw Data|The size of the uncompressed data that is written to the Logstore per minute.|
|Overall QPS|The QPS of all operations.|
|Number of operations|The number of API operations per minute. For more information, see [API reference](/intl.en-US/Developer Guide/API Reference/Overview.md).|
|Service Status|The number of returned HTTP status codes.|
|Traffic resolved successfully|The raw log data that Logtail collects.|
|Lines resolved successfully|The number of log lines that Logtail collects.|
|Lines failed to be resolved|The number of log lines that Logtail fails to collect. If data is collected for this metric, errors occurred during log data collection.|
|Number of errors|The number of errors that occurred during log collection.|
|Number of error instances|The number of servers where errors occurred when Logtail collected logs.|
|Number of Error IPs|The number of IP addresses that correspond to the servers where log collection errors occurred.Locate the IP address of the server based on the error. Log on to the server and check the `/usr/logtail/ilogtail.LOG` file to analyze the cause of the error. |
|The number of rows written.|The number of log lines that are written to the Logstore per minute.|
|Read Traffic|The size of the data that is read from the Logstore per minute.|
|Delay Time of Consumption|The difference between the receiving time of the log that is being consumed and the latest data receiving time. This is the difference between the consumption times of the shards with the largest difference In a consumer group, the delay time is the largest difference between the latest data receiving time of shards.|

## Set alert rules

This topic describes how to configure alert rules by using Cloud Monitor. If the trigger conditions of an alert rule are met, an SMS or email notification is sent to the specified recipients. You can configure alert rules in the Cloud Monitor console to monitor the status of log collection and shard usage and detect exceptions.

