# Query MNS logs

This topic describes how to query Alibaba Cloud Message Service \(MNS\) logs in different scenarios. You can query the logs in real time after MNS logs are pushed to Log Service. You can also use multiple keywords to retrieve fine-grained query results.

-   MNS logs are collected. For more information, see [Overview](/intl.en-US/Data Collection/Cloud product collection/MNS operations logs/Overview.md).
-   The indexing feature is enabled and configured. For more information, see [Enable and configure the index feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

MNS logs include queue logs and topic logs. The logs contain all information about messages throughout the lifecycle, such as the time, client, and operation. You can analyze the logs by using real-time query, real-time computing, and offline computing.

-   Real-time query: Query logs in the Log Service console in real time. For example, you can query the message tracing, the number of written messages, and the number of deleted messages.
-   Real-time computing: Calculate MNS logs in real time by using Spark, Storm, StreamCompute, or Consumer Library. For example, you can calculate the message producers and consumers of the top 10 messages in a queue. You can calculate the speed at which messages are produced and consumed to check whether the production and consumption are balanced. You can also calculate the processing latency of specified consumers to check whether a bottleneck exists.
-   Offline computing: Use MaxCompute, E-MapReduce, or Hive to calculate messages that are generated within a long period of time. For example, you can calculate the average latency of messages from publication to consumption in the last week.

## View the tracing of a message in a queue

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects list, click the project where the message resides.

3.  On the page that appears, choose **Log Management** \> **Logstores**, and then click the Logstore in which the message is stored.

4.  Enter a search statement.

    In this example, the message tracing of a message in a queue is queried. You must enter the queue name and the message ID in the format of $QueueName and $MessageId, for example, log and FF973C9C6572630D7F963C527CC5A82C.

5.  In the upper-right corner of the page, click **15 Minutes \(Relative\)** to set a time range for the query.

    You can select a relative time, set a time frame, or customize a time range.

    **Note:** The query results may contain logs that are generated one minute earlier or later than the specified time range.

6.  Click **Search & Analyze**.

    The following query result records the process from the time when the message is sent to the time when the message is received.

    ![View the tracing of a message in a queue](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1476554061/p32448.jpg)


## View the number of messages that are sent to Log Service in a queue

1.  Enter a search statement on the Search & Analysis page of the Logstore where the messages are stored.

    In this example, the number of messages sent in a queue is queried. You must enter the queue name and send operation in the format of $QueueName and \(SendMessage or BatchSendMessage\), for example, log and \(SendMessage or BatchSendMessage\).

2.  In the upper-right corner of the page, click **15 Minutes \(Relative\)** to set a time range for the query.

    You can select a relative time, set a time frame, or customize a time range.

    **Note:** The query results may contain logs that are generated one minute earlier or later than the specified time range.

3.  Click **Search & Analyze**.

    The following figure shows the query results. Three queue messages are sent by the message producer to a queue named log in the specified time range.

    ![View the number of messages that are sent to Log Service in a queue](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1476554061/p32449.jpg)


## View the number of messages that are consumed by Log Service in a queue

1.  Enter a search statement on the Search & Analysis page of the Logstore where the messages are stored.

    In this example, the number of messages that are consumed by Log Service in a queue is queried. You must enter the queue name and consumption operation in the format of $QueueName and \(ReceiveMessage or BatchReceiveMessage\), for example, log and \(ReceiveMessage or BatchReceiveMessage\).

2.  In the upper-right corner of the page, click **15 Minutes \(Relative\)** to set a time range for the query.

    You can select a relative time, set a time frame, or customize a time range.

    **Note:** The query results may contain logs that are generated one minute earlier or later than the specified time range.

3.  Click **Search & Analyze**.

    The following figure shows the query results. Twelve queue messages are consumed in the specified time range.

    ![View the number of messages that are consumed by Log Service in a queue](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1476554061/p32450.jpg)


## View the number of messages that are deleted from a queue

1.  Enter a search statement on the Search & Analysis page of the Logstore where the messages are stored.

    In this example, the number of messages that are deleted from a queue is queried. You must enter the queue name and delete operation in the format of $QueueName and \(DeleteMessage or BatchDeleteMessage\), for example, log and \(DeleteMessage or BatchDeleteMessage\).

2.  In the upper-right corner of the page, click **15 Minutes \(Relative\)** to set a time range for the query.

    You can select a relative time, set a time frame, or customize a time range.

    **Note:** The query results may contain logs that are generated one minute earlier or later than the specified time range.

3.  Click **Search & Analyze**.

    The following figure shows the query results. Sixty-one queue messages are deleted in the specified time range.

    ![View the number of the messages that are deleted from a queue](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1476554061/p32451.jpg)


## View the tracing of a message in a topic

1.  Enter a search statement on the Search & Analysis page of the Logstore where the messages are stored.

    In this example, the message tracing of a message in a topic is queried. You must enter the topic name and MessageId in the format of $TopicName and $MessageId, for example, logtest and 979628CD657261357FCB3C8A68BFA0E3.

2.  In the upper-right corner of the page, click **15 Minutes \(Relative\)** to set a time range for the query.

    You can select a relative time, set a time frame, or customize a time range.

    **Note:** The query results may contain logs that are generated one minute earlier or later than the specified time range.

3.  Click **Search & Analyze**.

    The following query result records the process from the time when the message is sent to the time when the message is received.

    ![View the tracing of a message in a topic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1476554061/p32452.jpg)


## View the number of messages that are published in a topic

1.  Enter a search statement on the Search & Analysis page of the Logstore where the messages are stored.

    In this example, the number of messages that are published in a topic is queried. You must enter the topic name and publish operation in the format of $TopicName and PublishMessage, for example, logtest and PublishMessage.

2.  In the upper-right corner of the page, click **15 Minutes \(Relative\)** to set a time range for the query.

    You can select a relative time, set a time frame, or customize a time range.

    **Note:** The query results may contain logs that are generated one minute earlier or later than the specified time range.

3.  Click **Search & Analyze**.

    The following figure shows the query results. Three messages are published to the logtest topic in the specified time range.

    ![View the number of messages that are published in a topic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2476554061/p32453.jpg)


## View the number of messages that are processed by a client

1.  Enter a search statement on the Search & Analysis page of the Logstore where the messages are stored.

    In this example, the number of messages that are processed by a client is queried. You must enter the client IP address in the format of $ClientIP, for example, 10.10.10.0.

    If you need to query a specific type of operations log, you can use multiple keywords, for example, $ClientIP and \(SendMessage or BatchSendMessage\).

2.  In the upper-right corner of the page, click **15 Minutes \(Relative\)** to set a time range for the query.

    You can select a relative time, set a time frame, or customize a time range.

    **Note:** The query results may contain logs that are generated one minute earlier or later than the specified time range.

3.  Click **Search & Analyze**.

    The following figure shows the query results. The client processed 66 messages in the specified time range.

    ![View the number of messages that are processed by a client](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2476554061/p32454.jpg)


