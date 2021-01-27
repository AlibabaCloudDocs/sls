# GetCursor

Queries a cursor based on a specific time.

## Description

You can use a cursor to locate a specific log. The following figure shows the relationships among a cursor, project, Logstore, and shard.

![Cursor](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1573029951/p6710.png)

-   Each project has multiple Logstores.
-   Each Logstore has multiple shards.
-   You can use a cursor to locate a specific log.

## Request syntax

```
GET /logstores/logstoreName/shards/shardID HTTP/1.1
Content-Type: application/json
Content-Length: 0
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: ProjectName.Endpoint
Date: GMT Date
Authorization: LOG yourAccessKeyId:yourSignature
x-log-date: Tue, 01 Dec 2020 05:51:27 GMT
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The GetCursor operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |logstoreName|String|Yes|sls-test-logstore|The name of the Logstore.|
    |shardID|String|Yes|0|The ID of the shard.|
    |type|String|Yes|cursor|The type of the request. Set the value to cursor.|
    |from|String|Yes|begin|The time that is used to query a cursor. Set the value to a UNIX timestamp or a string such as `begin` and `end`.|

    You can use the from parameter to locate the logs within the lifecycle of a Logstore. For example, assume that the lifecycle of a Logstore is \[begin\_time, end\_time\) and the from parameter is set to from\_time.

    -   If `from_time is earlier than or equal to begin_time, or from_time is equal to "begin"`, a cursor is returned. The cursor corresponds to the log whose timestamp is begin\_time.
    -   If `from_time is later than or equal to end_time, or from_time is equal to "end"`, a cursor is returned. The cursor corresponds to the next log to be written at the current time \(No data exists at the current cursor position\).
    -   If `from_time is later than begin_time, and from_time is earlier than end_time`, a cursor is returned. The cursor corresponds to the first packet that Log Service receives later than or at from\_time.
    **Note:** The lifecycle of a Logstore is specified by the ttl parameter. For example, the server time is November 11, 2018, 09:00:00 and the ttl parameter is set to 5. The data that was received in the interval `[2018-11-05 09:00:00,2018-11-11 09:00:00)` is consumable.


## Response parameters

-   Response headers

    The GetCursor operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. After the request succeeds, the response body contains the information of the cursor. The following table describes the parameter of the response body.

    |Parameter|Type|Example|Description|
    |---------|----|-------|-----------|
    |cursor|String|MTQ0NzI5OTYwNjg5NjYzMjM1Ng==|The value of the cursor.|


## Examples

-   Sample requests

    ```
    GET /logstores/sls-test-logstore/shards/0? type=cursor&from=begin
    Header:
    {
        "Content-Length": 0, 
        "x-log-signaturemethod": "hmac-sha1", 
        "x-log-bodyrawsize": 0, 
        "User-Agent": "log-python-sdk-v-0.6.0", 
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com", 
        "Date": "Thu, 12 Nov 2015 03:56:57 GMT", 
        "x-log-apiversion": "0.6.0", 
        "Content-Type": "application/json", 
        "Authorization": "LOG yourAccessKeyId:yourSignature"
    }
    ```

-   Sample success responses

    ```
    Header:
    {
        "content-length": "41", 
        "server": "nginx/1.6.1", 
        "connection": "close", 
        "date": "Thu, 12 Nov 2015 03:56:57 GMT", 
        "content-type": "application/json", 
        "x-log-requestid": "56440E0999248C070600C6AA"
    }
    Body:
    {
        "cursor": "MTQ0NzI5OTYwNjg5NjYzMjM1Ng=="
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|LogStoreNotExist|Logstore logstoreName does not exist.|The error message returned because the specified Logstore does not exist.|
|400|ParameterInvalid|Parameter From is not valid.|The error message returned because a parameter value is invalid.|
|400|ShardNotExist|Shard ShardID does not exist.|The error message returned because the specified shard does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|
|400|LogStoreWithoutShard|The logstore has no shard.|The error message returned because no shards exist in the Logstore.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

