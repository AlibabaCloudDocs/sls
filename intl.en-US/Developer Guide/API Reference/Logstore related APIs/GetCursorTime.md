# GetCursorTime

Queries the server time based on a cursor.

## Request syntax

```
GET /logstores/logstoreName/shards/shardID? cursor=cursor&type=cursor_time HTTP/1.1 
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

    The GetCursorTime operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Data type|Required|Example|Description|
    |---------|---------|--------|-------|-----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |logstoreName|String|Yes|internal-operation\_log|The name of the Logstore.|
    |shardID|Integer|Yes|0|The ID of the shard.|
    |cursor|String|Yes|MTU0NzQ3MDY4MjM3NjUxMzQ0Ng%3D%3D|The cursor that is used to query a timestamp.|
    |type|String|Yes|cursor\_time|The type of data to be queried. Default value: cursor\_time.|


## Response parameters

-   Response headers

    The GetCursorTime operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. After the request succeeds, the response body obtains the server time. The following table describes the parameter.

    |Parameter|Type|Example|Description|
    |---------|----|-------|-----------|
    |cursor\_time|String|1554260243|The server time that is queried based on the cursor. The time is a timestamp. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|


## Examples

-   Sample requests

    ```
    GET /logstores/internal-operation_log/shards/0? cursor=MTU0NzQ3MDY4MjM3NjUxMzQ0Ng%3D%3D&type=cursor_time HTTP/1.1 
    Header :
    { 
    'Content-Type': 'application/json',
    'Content-Length': '0',
    'x-log-bodyrawsize': '0',
    'x-log-apiversion': '0.6.0',
    'x-log-signaturemethod': 'hmac-sha1',
    'Host': 'ali-test-project.cn-hangzhou.log.aliyuncs.com',
    'Date': 'Tue, 01 Dec 2020 06:41:14 GMT',
    'Authorization': 'LOG yourAccessKeyId:yourSignature',
    'x-log-date': 'Tue, 01 Dec 2020 06:41:14 GMT'
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header : 
    {
    'Server': 'Tengine',
    'Content-Type': 'application/json',
    'Content-Length': '26',
    'Connection': 'close',
    'Access-Control-Allow-Origin': '*',
    'Date': 'Tue, 01 Dec 2020 06:42:01 GMT',
    'x-log-requestid': '5FC5E5B9C920EAE5D5B9D1D4'
    }
    Body :
    { 
      "cursor_time": 1554260243
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|LogStoreNotExist|Logstore logstoreName does not exist.|The error message returned because the specified Logstore does not exist.|
|400|ShardNotExist|Shard shardID does not exist.|The error message returned because the specified shard does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|
|400|LogStoreWithoutShard|the logstore has no shard.|The error message returned because no shards exist in the Logstore.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

