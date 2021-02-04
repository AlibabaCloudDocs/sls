# PutLogs

Writes logs to a specified Logstore.

## Description

-   When you call the PutLogs operation to write logs to Log Service, Log Service checks the format of the logs. If a log entry does not meet the format requirements, the request fails and no logs are written.
-   You can write logs only in the [Protocol Buffers \(protobuf\)](/intl.en-US/Developer Guide/API Reference/Common resources/Data encoding.md) format. The logs are displayed as a log group.
-   You can write logs in one of the following modes:
    -   LoadBalance: In this mode, Log Service writes logs to a shard based on the status of all writable shards. This ensures high availability, with a service level agreement \(SLA\) of 99.95%. The mode applies to scenarios in which data writes and consumption are irrelevant to shards. For example, you can use this mode if you do not need to preserve the order of logs.
    -   KeyHash: In this mode, a key field is added in the URL parameter. Log Service writes logs to a shard based on the key field. This field is optional. If you do not specify this field, Log Service writes logs in LoadBalance mode. For example, Log Service can fix an instance as a producer to a specific shard based on the hash function. This ensures that data in the shard is written and consumed in a strict order. Each key exists only in one shard at a time even if the shard is merged or split. For more information, [Shards](/intl.en-US/Product Introduction/Basic concepts/Shards.md).
-   You can call the PutLogs operation to write up to 3 MB log group data or 4,096 log entries at a time. The value of each log entry in the log group cannot be greater than 1 MB. If one of the preceding limits is exceeded, the request will fail and no log data will be written.

## Protobuf log data

The following tables describe the fields of compressed log data in the protobuf format. For more information, see [Data model](/intl.en-US/Developer Guide/API Reference/Common resources/Data model.md) and [Data encoding](/intl.en-US/Developer Guide/API Reference/Common resources/Data encoding.md).

-   Log

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Time|Integer|Yes|The timestamp when logs are generated. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|
    |Contents|List|Yes|The list of log fields. The list must contain at least one element. For more information, see the [Content](#table_ghl_68w_i35) table.|

-   Content

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Key|String|Yes|The name of the custom key.|
    |Value|String|Yes|The value of the custom key.|

-   LogTag

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Key|String|Yes|The name of the custom key.|
    |Value|String|Yes|The value of the custom key.|

-   LogGroup

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Logs|List|Yes|The list of logs. For more information, see the [Log](#table_7r8_0fs_hm7) table.|
    |Topic|String|No|The subject of the log. This field is user-defined and is used to differentiate logs.|
    |Source|String|No|The source of the log. For example, the IP address of the machine where the log is generated.|
    |LogTags|List|Yes|The list of log tags. For more information, see the [LogTag](#table_ghl_68w_i35) table.|


## Request syntax

-   LoadBalance mode

    ```
    POST /logstores/logstoreName/shards/lb HTTP/1.1
    Authorization: LOG yourAccessKeyId:yourSignature
    Content-Type: application/x-protobuf
    Content-Length: Content Length
    Content-MD5: Content MD5
    Date: GMT Date
    Host: ProjectName.Endpoint
    x-log-apiversion: 0.6.0
    x-log-bodyrawsize: BodyRawSize
    x-log-compresstype: lz4
    x-log-signaturemethod: hmac-sha1
    <Compressed log data in the Protocol Buffer format>
    ```

-   KeyHash mode

    ```
    POST /logstores/logstoreName/shards/route? key=14d2f850ad6ea48e46e4547edbbb27e0
    Authorization: LOG yourAccessKeyId:yourSignature
    Content-Type: application/x-protobuf
    Content-Length: Content Length
    Content-MD5: Content MD5
    Date: GMT Date
    Host: ProjectName.Endpoint
    x-log-apiversion: 0.6.0
    x-log-bodyrawsize: BodyRawSize
    x-log-compresstype: lz4
    x-log-signaturemethod: hmac-sha1
    <Compressed log data in the Protocol Buffer format>
    ```


The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Request parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |logstoreName|String|Yes|sample-logtail-config|The name of the Logstore.|


## Response parameters

-   Response headers

    The PutLogs operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. If the request is successful, no other elements are returned.


## Examples

-   Sample requests

    ```
    POST /logstores/sls-test-logstore/shards/lb
    {
        "Content-Length": 118,
        "Content-Type":"application/x-protobuf",
        "x-log-bodyrawsize":1356,
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
        "Content-MD5":"6554BD042149C844761C2C094A8FECCE",
        "Date":"Thu, 12 Nov 2015 06:54:26 GMT",
        "x-log-apiversion": "0.6.0",
        "x-log-compresstype":"lz4"
        "x-log-signaturemethod": "hmac-sha1",
        "Authorization":"LOG yourAccessKeyId:yourSignature"
    }
    <Binary data that is obtained after logs in the protobuf format are compressed by using the LZ4 algorithm.>
    ```

-   Sample success responses

    ```
    Header
    {   
        "Access-control-allow-origin" : "*", 
        "Date" : "Wed, 11 Nov 2015 08:28:20 GMT", 
        "Server" : "Tengine",
        "Content-Length" : 0, 
        "x-log-requestid" : "5642FC2399248C8F7B0145FD", 
        "Connection" : "close"
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|:----------|
|400|PostBodyInvalid|Protobuffer content cannot be parsed.|The error message returned because the logs in the protobuf format are invalid and cannot be parsed.|
|400|InvalidTimestamp|Invalid timestamps are in logs.|The error message returned because the logs contain invalid timestamps.|
|400|InvalidEncoding|Non-UTF8 characters are in logs.|The error message returned because the logs contain non-UTF-8 characters.|
|400|InvalidKey|Invalid keys are in logs.|The error message returned because the logs contain invalid keys.|
|400|PostBodyTooLarge|Logs must be less than or equal to 3 MB and 4096 entries.|The error message returned because the size of logs is greater than 3 MB or the number of log entries exceeds 4,096.|
|400|PostBodyUncompressError|Failed to decompress logs.|The error message returned because the logs have failed to be decompressed.|
|400|PostBodyInvalid|The post data time is out of range|The error message returned because the log timestamp is not within the valid range: \[-168 hours, +15 minutes\].|
|404|LogStoreNotExist|logstore logstoreName does not exist.|The error message returned because the specified Logstore does not exist.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

