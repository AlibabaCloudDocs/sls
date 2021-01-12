# Consume log data

Log Service provides SDKs developed in multiple programming languages. You can use one of the SDKs to consume log data. This topic describes the consumption preview feature and describes how to use an SDK to consume log data.

## Use an SDK to consume log data

The following example shows how to use the [SDK for Java](https://github.com/aliyun/aliyun-log-java-sdk) to consume log data from a shard.

```
Client client = new Client(host, accessId, accessKey);

    String cursor = client.GetCursor(project, logStore, shardId, CursorMode.END).GetCursor();
    System.out.println("cursor = " +cursor);
    try {
      while (true) {
        PullLogsRequest request = new PullLogsRequest(project, logStore, shardId, 1000, cursor);
        PullLogsResponse response = client.pullLogs(request);
        System.out.println(response.getCount());
        System.out.println("cursor = " + cursor + " next_cursor = " + response.getNextCursor());
        if (cursor.equals(response.getNextCursor())) {
            break;
                }
        cursor = response.getNextCursor();
        Thread.sleep(200);
      }
    }
    catch(LogException e) {
      System.out.println(e.GetRequestId() + e.GetErrorMessage());
    }
```

For more information, see [Log Service SDK reference](/intl.en-US/Developer Guide/SDK Reference/Overview.md).

## Consumption preview

Log consumption preview is a type of log data consumption. You can preview some log data in the Log Service console.

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the project from which you want to consume data.

3.  On the page that appears, choose **Log Storage** \> **Logstores**. On the Logstores tab, find the Logstore from which you want to consume data, and then choose **![Log preview](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1033359951/p53655.png)** \> **Consumption Preview**.

4.  In the Consumption Preview dialog box, select a shard and a time range, and then click **Preview**.

    The Consumption Preview page displays the log data of the first 10 packets in the specified time range.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7481220061/p5786.png)


