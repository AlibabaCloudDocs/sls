# Use Realtime Compute to consume log data

You can use Realtime Compute \(Blink\) to create a schema for data in Log Service and consume the data. This topic describes how to use Realtime Compute to create a schema for data in Log Service. This topic also describes the attribute fields and data type mapping that you can configure when you create a schema.

## Create a schema for data in Log Service

Log Service stores streaming data. Realtime Compute can use the streaming data as input data. The following example is a sample log entry:

```
__source__:  11.85.123.199
__tag__:__receive_time__:  1562125591
__topic__:  test-topic
a:  1234
b:  0
c:  hello
```

The following example is a DDL statement used to create a schema for data in Log Service:

```
create table sls_stream(
  a int,
  b int,
  c varchar
) with (
  type ='sls',
  endPoint ='<your endpoint>',
  accessId ='<your AccessKey ID>',
  accessKey ='<your AccessKey Secret>',
  startTime = '2017-07-05 00:00:00',
  project ='ali-cloud-streamtest',
  logStore ='stream-test',
  consumerGroup ='consumerGroupTest1'
);
```

The following table describes the parameters in the WITH clause.

|Parameter|Required|Description|
|---------|--------|-----------|
|endPoint|Yes|The endpoint of Log Service. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|
|accessId|Yes|The AccessKey ID used to access Log Service.|
|accessKey|Yes|The AccessKey secret used to access Log Service.|
|project|Yes|The name of the project in Log Service.|
|logStore|Yes|The name of the Logstore in Log Service.|
|consumerGroup|No|The name of the consumer group.|
|startTime|No|The time when Realtime Compute starts to consume data.|
|heartBeatIntervalMills|No|The heartbeat interval of the consumption client. Unit: seconds. Default value: 10.|
|maxRetryTimes|No|The maximum number of retries to read data. Default value: 5.|
|batchGetSize|No|The number of log groups that are read at a time. Default value: 10. If the Blink version is 1.4.2 or later, the default value is 100 and the maximum value is 1000.**Note:** If the size of a single log entry and the number of log groups in a batch are large, the Java system may frequently recycle the data stored in the memory. |
|columnErrorDebug|No|Specifies whether to enable debugging. If debugging is enabled, log entries that fail to be parsed are displayed. Default value: false.|

## Attribute fields

In addition to the fields in the log content, Realtime Compute can also extract three attribute fields and custom fields in tags, such as \_\_receive\_time\_\_. The following table describes the three attribute fields.

|Field|Description|
|:----|:----------|
|`__source__`|The source of the log entry.|
|`__topic__`|The topic of the log entry.|
|`__timestamp__`|The time when the log entry is generated.|

To extract the preceding fields, you must add HEADERs in the DDL statement. Example:

```
create table sls_stream(
  __timestamp__ bigint HEADER,
  __receive_time__ bigint HEADER
  b int,
  c varchar
) with (
  type ='sls',
  endPoint ='<your endpoint>',
  accessId ='<your AccessKey ID>',
  accessKey ='<your AccessKey Secret>',
  startTime = '2017-07-05 00:00:00',
  project ='ali-cloud-streamtest',
  logStore ='stream-test',
  consumerGroup ='consumerGroupTest1'
);
```

## Field type mapping

The following table lists the mapping between Log Service and Realtime Compute data types. We recommend that you declare the type mapping in the DDL statement.

|Log Service data type|Realtime Compute data type|
|---------------------|--------------------------|
|STRING|VARCHAR|

If you specify another data type to convert data in Log Service, an automatic conversion attempt is performed. For example, you can specify BIGINT as the data type to convert the string `1000` and specify timestamp as the data type to convert the string `2018-01-12 12:00:00`.

## Precautions

-   Blink 2.2.0 or earlier versions do not support shard scaling. If you split or merge shards when a job is reading data from a Logstore, the job fails and cannot continue. In this case, you must restart the job.
-   None of the Blink versions allow you to delete or recreate a Logstore whose log data is being consumed.
-   For Blink 1.6.0 and earlier versions, if you specify a consumer group to consume log data from a Logstore that contains a large number of shards, the read performance may be affected.
-   You cannot define the map data type in Realtime Compute when you create a schema for data in Log Service.
-   Nonexistent fields are set to null.
-   We recommend that you define the fields in the same order as the fields are in the preceding table. Unordered fields are also supported.
-   If no new data is written to a shard, the overall latency of a job increases. In this case, you need to adjust the number of concurrent tasks in the job to the same as the number of shards where data is read and written.
-   To extract fields from tags such as `__tag__:__hostname__` and `__tag__:__path__`, you can delete the \_\_tag\_\_: prefix and follow the method used to extract attribute fields.

    **Note:** This type of data cannot be extracted during debugging. We recommend that you use the local debugging method and the print method to display data in logs.


