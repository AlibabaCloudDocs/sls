# Logs

Logs are records of changes that occur in a system when the system runs. These records contain information about the operations on specific objects and the results of the operations. The records are ordered by time.

Log data is stored in different forms, such as log files, log events, binary logs, and metric data. Each log file consists of one or more log entries. A log entry is the basic unit of data that can be processed in Log Service. Each log entry describes a single system event.

Log Service uses a semi-structured data model to define a log entry. This model provides the topic, time, content, source, and tags fields of a log entry.

Log Service has different format requirements for different log fields. The following table describes the fields and the formats.

|Field|Description|Format|
|:----|:----------|:-----|
|Topic|The user-defined field in a log entry. This field can be used to identify a group of logs. For example, you can specify topics for access logs based on sites.|The field value can be a string of up to 128 bytes in length, including an empty string. The default value is an empty string.|
|Time|The time when a log entry is generated. This field is a reserved field. In most cases, the field value is generated based on the time information in the log entry.|This value is a UNIX timestamp. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|
|Content|The specific content of a log entry. The content consists of one or more items. Each item is a key-value pair.|The key is a UTF-8 encoded string of up to 128 bytes in length. It can contain letters, digits, and underscores \(\_\). The key cannot start with a digit or contain the following keywords: -   `__time__`
-   `__source__`
-   `__topic__`
-   `__partition_time__`
-   `_extract_others_`
-   `__extract_others__`

 The value can be a string of up to 1,048,576 bytes in length.|
|Source|The source of a log entry. For example, the value of this field can be the IP address of the server where the log entry is generated.|The value of this field can be a string of up to 128 bytes in length. The default value is an empty string.|
|Tags|The tags of a log entry. The following log tags can be used: -   User-defined tags: the tags that you add when you call the [PutLogs](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/PutLogs.md) operation to write data to a specified Logstore.
-   [System tags](/intl.en-US/Data Collection/Preparation/Manage a Logstore.md): the tags added by Log Service, including `__client_ip__` and `__receive_time__`.

|The field value is a dictionary format. Both keys and values are strings. The field name is prefixed by `__tag__:`.|

Logs generated in different scenarios may have different formats. The following example describes how to convert a raw NGINX access log to the data model supported by Log Service. For example, the IP address of the NGINX server is `10.249.201.117`. The following log entry is generated on this server:

```
10.1.168.193 - - [01/Mar/2012:16:12:07 +0800] "GET /Send?AccessKeyId=8225105404 HTTP/1.1" 200 5 "-" "Mozilla/5.0 (X11; Linux i686 on x86_64; rv:10.0.2) Gecko/20100101 Firefox/10.0.2"
```

The following table describes how to convert the raw log entry to the data model supported by Log Service.

|Field|Field value|Description|
|:----|:----------|:----------|
|Topic|“”'|An empty string \(default value\) is used.|
|Time|1330589527|The timestamp when the log entry was generated. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970. The field value is a UNIX timestamp that is converted from the time information in the raw log entry.|
|Content|Key-value pairs|The specific content of the log entry.|
|Source|"10.249.201.117"|The IP address of the server where the log is collected.|
|Tags|N/A|The field that is added by the user or Log Service.|

You can decide how to extract the original content of a log entry and convert the extracted content to key-value pairs. The following table provides an example.

|Key|Value|
|:--|:----|
|ip|`10.1.168.193`|
|method|`GET`|
|status|`200`|
|length|`5`|
|ref\_url|`-`|
|browser|`Mozilla/5.0 (X11; Linux i686 on x86_64; rv:10.0.2) Gecko/20100101 Firefox/10.0.2`|

