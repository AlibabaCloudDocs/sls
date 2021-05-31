# FAQ about log query

## How do I identify the source server from which Logtail collects logs during a query?

If a machine group uses IP addresses as its identifier when logs are collected by using Logtail, servers in the machine group are distinguished from one another by internal IP addresses. When querying logs, you can use the hostname and custom working IP address to identify the source server from which logs are collected.

For example, you can use the following statement to count the times different hostnames appear in logs:

**Note:** You must enable the indexing feature for the Logstore and enable the statistics feature for the \_\_tag\_\_:\_\_hostname\_\_ field in advance.

```
* | select "__tag__:__hostname__" , count(1) as count group by "__tag__:__hostname__"
```

![](../images/p41659.png)

## How do I search for IP addresses in logs?

IP addresses in logs can be searched for by means of exact match. You can directly search for log information related to a specified IP address in the log data, such as whether to include a specified IP address or whether to filter out a specified IP address. Currently, partial match cannot be used for searching data. That is, you cannot search for part of an IP address because the decimal point is not a default word-breaking item in Log Service. We recommend that you filter data by yourself if necessary. For example, you can use the SDK to download data, and then use regular expressions or the string.indexof method in the code.

For example, you set the following search criteria in a Log Service project:

```
not ip:121.42.0 not status:200 not 360jk not DNSPod-Monitor not status:302 not jiankongbao
        not 301 and status:403
```

The search result still contains 121.42.0.x. Log Service regards 121.42.0.x as a word. To include or filter out 121.42.0.x in the search result, you must specify 121.42.0.x in the search criteria, but not 121.42.0.

## How do I search for a keyword that contains a space in logs?

When you directly search for a keyword that contains a space, you can obtain all the logs that contain part of the keyword on the left or right of the space. We recommend that you enclose the keyword that contains a space in double quotation marks \(" "\) so that the entire enclosed content is regarded as a keyword to be searched for. In this way, you can obtain the expected search result.

For example, you need to search for the keyword `POS version` in the following log data:

```
post():351]:&nbsp;device_id:&nbsp;BTAddr&nbsp;:&nbsp;B6:xF:xx:65:xx:A1&nbsp;IMEI&nbsp;:&nbsp;35847xx22xx81x9&nbsp;WifiAddr&nbsp;:&nbsp;4c:xx:0e:xx:4e:xx&nbsp;|&nbsp;user_id:&nbsp;bb07263xxd2axx43xx9exxea26e39e5f&nbsp;POS&nbsp;version:903
```

If you directly search for `POS version`, all the logs that contain `POS` or `version` are displayed, which do not meet your expectations. If you search for `"POS version"`, all the logs that contain the keyword `POS version` are displayed.

## How do I perform a double-condition search?

A double-condition search requires you to enter two statements in one search.

For example, you need to search for logs whose data status is neither OK nor Unknown in a Logstore. You can obtain the expected search result by searching for `not OK not Unknown`.

## How can I query collected logs in Log Service?

You can use one of the following methods to query logs in Log Service:

1.  Use the Log Service console.
2.  Use the SDK.
3.  Use the Restful API.

