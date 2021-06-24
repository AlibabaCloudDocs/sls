# Use Flume to consume log data

This topic describes how to use Flume to consume log data. You can use the aliyun-log-flume agent to connect Log Service to Flume, and then write data to Log Service or consume log data from Log Service.

The aliyun-log-flume agent connects Log Service to Flume. When Log Service is connected to Flume, you can connect Log Service to other systems such as HDFS and Kafka by using Flume. The aliyun-log-flume agent provides sinks and sources to connect Log Service to Flume.

-   Sink: uses Flume to read data from other data sources and then writes the data to LogHub.
-   Source: uses Flume to consume data from Log Service and then writes the data to other systems.

For more information about the aliyun-log-flume agent, visit [GitHub](https://github.com/aliyun/aliyun-log-flume).

## Procedure

1.  Download and install Flume. For more information, visit [Flume](http://flume.apache.org/download.html).

2.  Download the aliyun-log-flume agent and save the agent in the cd/\*\*\*/flume/lib directory. To download the agent, click [aliyun-log-flume-1.3.jar](https://github.com/aliyun/aliyun-log-flume/releases/download/1.3/aliyun-log-flume-1.3.jar).

3.  In the cd/\*\*\*/flume/conf directory, create a configuration file named flumejob.conf.

    -   For information about how to configure a sink, see [Sink](#section_wsy_7qg_7zx).
    -   For information about how to configure a source, see [Source](#section_wcf_ixl_8zq).
4.  Launch Flume.


## Sink

You can configure a sink for Flume to write data from other data sources to Log Service. Data can be parsed into the following two formats:

-   SIMPLE: writes a Flume event to Log Service as a field.
-   DELIMITED: delimits a Flume event with delimiters, parses an event into fields based on the configured column names, and then writes the fields to Log Service.

The following table describes the configuration parameters of a sink.

|Parameter|Required|Description|
|---------|--------|-----------|
|type|Yes|Default value: com.aliyun.loghub.flume.sink.LoghubSink.|
|endpoint|Yes|The endpoint of the region where the Log Service project resides. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|
|project|Yes|The name of the project.|
|logstore|Yes|The name of the Logstore.|
|accessKeyId|Yes|The AccessKey ID used to access Log Service.|
|accessKey|Yes|The AccessKey secret used to access Log Service.|
|batchSize|No|The number of log entries that are written to Log Service at a time. Default value: 1000.|
|maxBufferSize|No|The maximum number of log entries in the buffer. Default value: 1000.|
|serializer|No|The serialization format of log data. Valid values: -   **DELIMITED**: Data is parsed into the CSV format.
-   **SIMPLE**: Data is parsed into the single-line format. This is the default value.
-   Custom **serializer**: Data is parsed into a custom serialization format. If you set this parameter to a custom deserializer, you must specify the full names of columns. |
|columns|No|The column names. You must specify this parameter if you set the serializer parameter to **DELIMITED**. Separate multiple columns with commas \(,\). Make sure that the columns are sorted in the same order as they are in the log entries.|
|separatorChar|No|The delimiter, which must be a single character. You must specify this parameter if you set the serializer parameter to **DELIMITED**. The default delimiter is a comma \(,\).|
|quoteChar|No|The quote character. You must specify this parameter if you set the serializer parameter to **DELIMITED**. The default quote character is a quote \("\).|
|escapeChar|No|The escape character. You must specify this parameter if you set the serializer parameter to **DELIMITED**. The default escape character is a quote \("\).|
|useRecordTime|No|Specifies whether to use the value of the timestamp field as the log time when log data is written to Log Service. Default value: false. This value indicates that the current time is used as the log time.|

For more information about how to configure a sink, visit [GitHub](https://github.com/aliyun/aliyun-log-flume/blob/master/src/test/resources/sink-example.conf).

## Source

You can configure a source for Flume to ship data from Log Service to other data sources. Data can be parsed into the following two formats:

-   DELIMITED: writes delimited log data to Flume.
-   JSON: writes JSON-formatted log data to Flume.

The following table describes the configuration parameters of a source.

|Parameter|Required|Description|
|---------|--------|-----------|
|type|Yes|Default value: com.aliyun.loghub.flume.source.LoghubSource.|
|endpoint|Yes|The endpoint of the region where the Log Service project resides. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|
|project|Yes|The name of the project.|
|logstore|Yes|The name of the Logstore.|
|accessKeyId|Yes|The AccessKey ID used to access Log Service.|
|accessKey|Yes|The AccessKey secret used to access Log Service.|
|heartbeatIntervalMs|No|The heartbeat interval between the client and Log Service. Unit: milliseconds. Default value: 30000.|
|fetchIntervalMs|No|The interval at which data is pulled from Log Service. Default value: 100. Unit: milliseconds.|
|fetchInOrder|No|Specifies whether to consume log data in the order that the log data was written to Log Service. Default value: false.|
|batchSize|No|The number of log entries that are read at a time. Default value: 100.|
|consumerGroup|No|The name of the consumer group that reads log data.|
|initialPosition|No|The start point from which data is read. Valid values: **begin**, **end**, and **timestamp**. Default value: **begin**. **Note:** If a checkpoint exists on the server, the checkpoint is used. |
|timestamp|No|The UNIX timestamp. You must specify this parameter if you set the initialPosition parameter to **timestamp**.|
|deserializer|Yes|The deserialization format of log data. Valid values: -   **DELIMITED**: Data is parsed into the CSV format. This is the default value.
-   **JSON**: Data is parsed into the JSON format.
-   Custom **deserializer**: Data is parsed into a custom deserialization format. If you set this parameter to a custom deserializer, you must specify the full names of the columns. |
|columns|No|The column names. You must specify this parameter if you set the deserializer parameter to **DELIMITED**. Separate multiple columns with commas \(,\). Make sure that the columns are sorted in the same order as they are in the log entries.|
|separatorChar|No|The delimiter, which must be a single character. You must specify this parameter if you set the deserializer parameter to **DELIMITED**. The default delimiter is a comma \(,\).|
|quoteChar|No|The quote character. You must specify this parameter if you set the deserializer parameter to **DELIMITED**. The default quote character is a quote \("\).|
|escapeChar|No|The escape character. You must specify this parameter if you set the deserializer parameter to **DELIMITED**. The default escape character is a quote \("\).|
|appendTimestamp|No|Specifies whether to append the timestamp as a field to the end of each log entry. You must specify this parameter if you set the deserializer parameter to **DELIMITED**. Default value: false.|
|sourceAsField|No|Specifies whether to add the log source as a field named \_\_source\_\_. You must specify this parameter if you set the deserializer parameter to **JSON**. Default value: false.|
|tagAsField|No|Specifies whether to add the log tags as a field named \_\_tag\_\_: \{tag names\}. You must specify this parameter if you set the deserializer parameter to **JSON**. Default value: false.|
|timeAsField|No|Specifies whether to add the log time as a field named \_\_time\_\_. You must specify this parameter if you set the deserializer parameter to **JSON**. Default value: false.|
|useRecordTime|No|Specifies whether to use the value of the timestamp field as the time when log data is read from Log Service. Default value: false. This value indicates that the current time is used as the log time. Default value: false.|

For more information about how to configure a source, visit [GitHub](https://github.com/aliyun/aliyun-log-flume/blob/master/src/test/resources/source-example.conf).

