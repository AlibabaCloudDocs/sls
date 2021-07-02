# Log

Logs are records of changes that occur in a system during the running of the system. The records contain information about the operations that are performed on specific objects and the results of the operations. The records are ordered by time.

## Format

Log data is stored in different forms, such as log files, log events, binary logs, and metric data. Log Service uses a semi-structured data model to define logs. A log entry consists of the following fields: topic, time, content, source, and tags. Log Service has different format requirements on different log fields. The following table describes the log fields and provides the format requirements.

|Field|Description|Format|
|:----|:----------|:-----|
|Topic|The custom field in a log entry. This field can be used to identify the log topic. For example, you can set different log topics \(access\_log and operation\_log\) for website logs based on log types. For more information, see [Topic](/intl.en-US/Product Introduction/Basic concepts/Topic.md).|The field value can be a string of up to 128 bytes, including an empty string. If the field is an empty string, it indicates that the log topic is not configured. |
|Time|The time when the log entry is generated, or the system time of the host where Logtail resides when the log data is collected. This field is a reserved field.|The value is a UNIX timestamp. It is the number of seconds that have elapsed since 00:00:00 Thursday, 1 January 1970.|
|Content|The content of the log entry. The content consists of one or more items. Each item is a key-value pair.|A key-value pair must comply with the following requirements:-   The key is a UTF-8 encoded string of up to 128 bytes. The key can contain letters, digits, and underscores \(\_\). The string is 1 to 128 bytes. The following fields cannot be used:
    -   \_\_time\_\_
    -   \_\_source\_\_
    -   \_\_topic\_\_
    -   \_\_partition\_time\_\_
    -   \_extract\_others\_
    -   \_\_extract\_others\_\_
-   The value can be a string of up to 1 MB. |
|Source|The source of the log entry. For example, the value of this field can be the IP address of the server where the log entry is generated.|The value of this field can be a string of up to 128 bytes.|
|Tags|The tags of the log entry. The following log tags can be used: -   Custom tags: the tags that you add when you call the [PutLogs](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/PutLogs.md) operation to write logs to a specified Logstore.
-   System tags: the tags added by Log Service. The tags include \_\_client\_ip\_\_ and \_\_receive\_time\_\_.

|The field value is in the dictionary format. The keys and the values are strings. The field name is prefixed by \_\_tag\_\_:.|

## Examples

The following examples show a website access log that is collected to Log Service in different modes.

-   Raw log entry

    ```
    127.0.0.1 - - [01/Mar/2021:12:36:49  0800] "GET /index.html HTTP/1.1" 200 612 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36
    ```

-   Log entry collected to Log Service in simple mode

    ![Sample log entry](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4266315261/p272301.png)

-   Log entry collected to Log Service in full regex mode

    ![Sample log entry](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5266315261/p272326.png)


