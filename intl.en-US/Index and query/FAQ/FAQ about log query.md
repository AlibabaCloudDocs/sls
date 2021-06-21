# FAQ about log query

This topic describes the FAQ about log query.

## How do I identify the source server from which Logtail collects logs during a query?

If a machine group uses IP addresses as its identifier when logs are collected by using Logtail, servers in the machine group are distinguished by internal IP addresses. When you query logs, you can use the hostname and custom IP address to identify the source server from which logs are collected.

For example, you can use the following statement to count the times different hostnames appear in logs:

**Note:** You must configure an index for the \_\_tag\_\_:\_\_hostname\_\_ field and enable the statistics feature.

```
* | select "__tag__:__hostname__" , count(1) as count group by "__tag__:__hostname__"
```

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3254624261/p41659.png)

## How do I query IP addresses in logs?

You can use the exact match method to query IP addresses in logs. You can search for log data by IP address. For example, you can specify whether to include or exclude an IP address. However, you cannot use the partial match method to query log data. This is because decimal points contained in an IP address are not default delimiters in Log Service. You can also filter data by using other methods. For example, you can use an SDK to download data and then use a regular expression or the string.indexof\(\) method to search for results.

For example, if you execute the following statement, the log entries that are retrieved from the 121.42.0 CIDR block are still returned.

```
not ip:121.42.0 not status:200 not 360jk not DNSPod-Monitor not status:302 not jiankongbao
        not 301 and status:403
```

## How do I use two conditions to query log data?

If you need to use two conditions to query logs, enter two statements at the same time.

For example, you want to query log entries whose status field is neither OK nor Unknown in a Logstore. You can use the `not OK not Unknown` statement to retrieve expected results.

## How can I query collected logs in Log Service?

You can use one of the following methods to query logs in Log Service:

1.  Use the Log Service console. For more information, see [Query logs](/intl.en-US/Index and query/Query logs.md).
2.  Use an SDK. For more information, see [SDK overview](/intl.en-US/Developer Guide/SDK Reference/Overview.md).
3.  Use the Restful API. For more information, see [GetLogs](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/GetLogs.md).

