# Overview

Log Service provides the real-time log consumption feature. This feature reads and writes full data in the first-in, first-out \(FIFO\) order, which is similar to Kafka. This topic describes the types of real-time log consumption.

After log data is sent to Log Service, you can process the data in the following three methods.

|Method|Scenario|Timeliness|Retention period|
|:-----|:-------|:---------|:---------------|
|Real-time log consumption|Stream computing and real-time computing|Real time|You can set a retention period based on your business requirements.|
|Log query|Online query of recent hot data|Near real time \(delayed for 1 second in 99.9% cases, and up to 3 seconds\)|You can set a retention period based on your business requirements.|
|Log shipping|Full log storage for offline analysis|Delayed for 5 to 30 minutes|The retention period is subject to the storage system.|

**Note:** For more information about the differences between reading and querying logs, see [What are the differences between LogHub and LogSearch?](/intl.en-US/Index and query/FAQ/What are the differences between LogHub and LogSearch?.md).

## Real-time log consumption

-   [t248948.md\#section\_9ph\_v1t\_uoq](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Consume logs.md)

    Log Service provides SDKs in various programming languages, such as Java, Python, and Go. You can use an SDK to consume log data. For more information about the SDKs, see [SDK reference](/intl.en-US/Developer Guide/SDK Reference/Overview.md).

-   [Use consumer groups to consume logs](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Consumption by consumer groups/Use consumer groups to consume logs.md)
-   [Configure Function Compute log consumption](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Use Fuction Compute to cosume LogHub Logs/Configure Function Compute log consumption.md)
-   Consume logs by using cloud services
    -   [Use Storm to consume LogHub logs](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Use Storm to consume LogHub logs.md)
    -   [Use Spark Streaming to consume log data](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Use Spark Streaming to consume log data.md)
    -   [Use Realtime Compute to consume LogHub logs](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Use Realtime Compute to consume LogHub logs.md)
    -   [Use CloudMonitor to consume LogHub logs](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Use CloudMonitor to consume LogHub logs.md)
    -   [Use Logstash to consume LogHub logs](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Use Logstash to consume LogHub logs.md)
-   Consume logs by using open source services
    -   [Use Flume to consume log data](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Use Flume to consume log data.md)
    -   [Use open source Flink to consume log data](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Use open source Flink to consume log data.md)

