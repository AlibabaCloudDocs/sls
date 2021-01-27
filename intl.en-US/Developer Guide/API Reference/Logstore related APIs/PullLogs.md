# PullLogs

Queries the logs in a specified location based on the cursor.

## Description

-   You must specify a shard when you query the logs.
-   Only data in the [Protocol Buffers \(protobuf\) format](/intl.en-US/Developer Guide/API Reference/Common resources/Data encoding.md) can be read.

## Request syntax

```
GET /logstores/logstoreName/shards/shardID HTTP/1.1
Accept: application/x-protobuf
Accept-Encoding: lz4
Authorization: LOG yourAccessKeyId:yourSignature
Date: GMT Date
Host: Projectname.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The PullLogs operation has the following operation-specific request headers:

    -   Accept: application/x-protobuf
    -   Accept-Encoding: lz4
    The value of Accept-Encoding must contain lz4, deflate, or double quotation marks \(""\).

    For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |logstoreName|String|Yes|sls-test-logstore|The name of the Logstore.|
    |shardID|Integer|Yes|0|The ID of the shard.|
    |type|String|Yes|logs|The request type. Set the value to logs.|
    |cursor|String|Yes|MTQ0NzMyOTQwMTEwMjEzMDkwNA|The cursor. It indicates the position from which data is read.|
    |count|Integer|No|1000|The number of log groups to return. Valid values: 1 to 1000.|


## Response elements

-   Response headers

    The PullLogs operation has the following operation-specific response elements:

    -   x-log-cursor: the cursor of the next log.
    -   x-log-count: the number of returned logs.
    For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    Serialized data in the protobuf format is returned. The data may be compressed.


## Examples

-   Sample requests

    ```
    GET /logstores/sls-test-logstore/shards/0? cursor=MTQ0NzMyOTQwMTEwMjEzMDkwNA==&count=1000&type=log  
    Header:
    {
        "Authorization"="LOG yourAccessKeyId:yourSignature", 
        "x-log-bodyrawsize"=0, 
        "User-Agent" : "sls-java-sdk-v-0.6.0", 
        "x-log-apiversion" : "0.6.0", 
        "Host" : "ali-test-project.cn-hangzhou-failover-intranet.sls.aliyuncs.com", 
        "x-log-signaturemethod" : "hmac-sha1", 
        "Accept-Encoding" : "lz4", 
        "Content-Length": 0,
        "Date" : "Thu, 12 Nov 2015 12:03:17 GMT",
        "Content-Type" : "application/x-protobuf", 
        "accept" : "application/x-protobuf"
    }
    ```

-   Sample success responses

    ```
    Header:
    {
        "x-log-count" : "1000", 
        "x-log-requestid" : "56447FB20351626D7C000874", 
        "Server" : "nginx/1.6.1", 
        "x-log-bodyrawsize" : "34121", 
        "Connection" : "close", 
        "Content-Length" : "4231", 
        "x-log-cursor" : "MTQ0NzMyOTQwMTEwMjEzMDkwNA==", 
        "Date" : "Thu, 12 Nov 2015 12:01:54 GMT", 
        "x-log-compresstype" : "lz4", 
        "Content-Type" : "application/x-protobuf"
    }
    Body:
    <The log group list in the protobuf format> after compression.
    ```

-   Page flip

    To flip the page and get the next token without returning data, you can send an HTTP HEAD request.


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|LogStoreNotExist|Logstore logstoreName does not exist.|The error message returned because the specified Logstore does not exist.|
|400|ParameterInvalid|Invalid cursor cursor.|The error message returned because the value of the cursor parameter is invalid.|
|400|ParameterInvalid|ParameterCount should be in \[0-1000\].|The error message returned because the value of the count parameter is invalid. Value range: \[0-1000\].|
|400|ShardNotExist|Shard ShardID does not exist.|The error message returned because the specified shard does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

