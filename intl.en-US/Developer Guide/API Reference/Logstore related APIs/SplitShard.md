# SplitShard

Splits a shard that is in the readwrite state.

## Request syntax

```
POST /logstores/logstoreName/shards/shardID HTTP/1.1
Content-Length: 0
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: ProjectName.Endpoint
Date': GMT Date
Authorization': LOG yourAccessKeyId:yourSignature
x-log-date: Tue, 01 Dec 2020 01:36:00 GMT
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The SplitShard operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |logstoreName|String|Yes|logstorename|The name of the Logstore.|
    |shardID|Integer|Yes|33|The ID of the shard.|
    |splitKey|String|Yes|ef000000000000000000000000000000|The location where the shard is split.|


## Response parameters

-   Response headers

    The SplitShard operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. After the request succeeds, an array that consists of the parameters of the three shards is returned. The first shard is the original shard. The other two shards are the split shards. The following table describes the parameters.

    |Parameter|Type|Example|Description|
    |---------|----|-------|-----------|
    |shardID|Integer|33|The ID of the shard.|
    |status|String|readonly|The status of the shard. Valid values: readonly and readwrite.    -   readonly: The shard supports only read operations.
    -   readwrite: The shard supports read and write operations. |
    |inclusiveBeginKey|String|ee000000000000000000000000000000|Indicates the start of the hash key range.|
    |exclusiveEndKey|String|ffffffffffffffffffffffffffffffff|Indicates the end of the hash key range.|
    |createTime|String|1453949705|The timestamp when the shard was created. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|


## Examples

-   Sample requests

    ```
    POST /logstores/logstorename/shards/33 HTTP/1.1
    Header :
    {
    'Content-Length': '0',
    'x-log-bodyrawsize': '0',
    'x-log-apiversion': '0.6.0',
    'x-log-signaturemethod': 'hmac-sha1',
    'Host': 'ali-test-project.cn-hangzhou.sls.aliyuncs.com',
    'Date': 'Tue, 01 Dec 2020 01:36:00 GMT',
    'Authorization': 'LOG yourAccessKeyId:yourSignature',
    'x-log-date': 'Tue, 01 Dec 2020 01:36:00 GMT'
    }
    ```

-   Sample success responses

    ```
    Header:
    {
        "content-length": "57", 
        "server": "nginx/1.6.1", 
        "connection": "close", 
        "date": "Thu, 12 Nov 2015 03:40:31 GMT", 
        "content-type": "application/json", 
        "x-log-requestid": "56440A2F99248C050600C74C"
    }
    Body :
    [
        {
            "shardID": 33,
            "status": "readonly",
            "inclusiveBeginKey": "ee000000000000000000000000000000",
            "exclusiveEndKey": "ffffffffffffffffffffffffffffffff",
            "createTime": 1453949705
        },
        {
            "shardID": 163,
            "status": "readwrite",
            "inclusiveBeginKey": "ee000000000000000000000000000000",
            "exclusiveEndKey": "ef000000000000000000000000000000",
            "createTime": 1453949705
        },
        {
            "shardID": 164,
            "status": "readwrite",
            "inclusiveBeginKey": "ef000000000000000000000000000000",
            "exclusiveEndKey": "ffffffffffffffffffffffffffffffff",
            "createTime": 1453949705
        }
    ]
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|LogStoreNotExist|logstore logstoreName does not exist.|The error message returned because the specified Logstore does not exist.|
|400|ParameterInvalid|invalid shard id.|The error message returned because the specified shard ID is invalid.|
|400|ParameterInvalid|invalid mid hash.|The error message returned because the specified splitkey parameter is invalid.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|
|400|LogStoreWithoutShard|logstore has no shard.|The error message returned because no shards exist in the Logstore.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

