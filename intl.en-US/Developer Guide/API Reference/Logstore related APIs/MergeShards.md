# MergeShards

Merges two adjacent shards that are in the readwrite state. You can specify a shard ID in the request. Then, Log Service locates the adjacent shard and combines the two shards into a single shard.

## Request syntax

```
POST /logstores/logstoreName/shards/shardID HTTP/1.1
Content-Length: 0
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: ProjectName.Endpoint
Date: GMT Date
Authorization: LOG yourAccessKeyId:yourSignature
x-log-date: Mon, 30 Nov 2020 09:37:18 GMT
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The MergeShards operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |logstoreName|String|Yes|logstorename|The name of the Logstore.|
    |shardID|Integer|Yes|30|The ID of the shard.|


## Response parameters

-   Response headers

    The MergeShards operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. After the request succeeds, an array that consists of the parameters of the three shards is returned. The first shard is the merged shard. The other two shards are the original shards. The following table shows the parameters.

    |Parameter|Type|Example|Description|
    |---------|----|-------|-----------|
    |shardID|Integer|167|The ID of the shard.|
    |status|String|readwrite|The status of the shard. Valid values: readonly and readwrite.    -   readonly: The shard supports only read operations.
    -   readwrite: The shard supports read and write operations. |
    |inclusiveBeginKey|String|ee000000000000000000000000000000|Indicates the start of the hash key range.|
    |exclusiveEndKey|String|ffffffffffffffffffffffffffffffff|Indicates the end of the hash key range.|
    |createTime|String|1453953105|The timestamp when the shard was created. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|


## Examples

-   Sample requests

    ```
    POST /logstores/logstorename/shards/30
    Header :
    {
        "Content-Length": 0, 
        "x-log-signaturemethod": "hmac-sha1", 
        "x-log-bodyrawsize": 0, 
        "User-Agent": "log-python-sdk-v-0.6.0", 
        "Host": "ali-test-project.cn-hangzhou.sls.aliyuncs.com", 
        "Date": "Thu, 12 Nov 2015 03:40:31 GMT", 
        "x-log-apiversion": "0.6.0", 
        "Authorization": "LOG yourAccessKeyId:yourSignature"
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
            'shardID': 167, 
            'status': 'readwrite', 
            'inclusiveBeginKey': 'e0000000000000000000000000000000',
            'createTime': 1453953105,
            'exclusiveEndKey': 'ffffffffffffffffffffffffffffffff'
        }, 
        {
            'shardID': 30, 
            'status': 'readonly', 
            'inclusiveBeginKey': 'e0000000000000000000000000000000', 
            'createTime': 0, 
            'exclusiveEndKey': 
            'e7000000000000000000000000000000'
        },
        {
            'shardID': 166, 
            'status': 'readonly', 
            'inclusiveBeginKey': 'e7000000000000000000000000000000', 
            'createTime': 1453953073, 
            'exclusiveEndKey': 'ffffffffffffffffffffffffffffffff'
        }
    ]
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|The Project does not exist : projectName|The error message returned because the specified project does not exist.|
|404|LogStoreNotExist|logstore logstoreName does not exist.|The error message returned because the specified Logstore does not exist.|
|400|ParameterInvalid|invalid shard id.|The error message returned because the specified shard ID is invalid.|
|400|ParameterInvalid|can not merge the last shard.|The error message returned because the last shard cannot be merged.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|
|400|LogStoreWithoutShard|logstore has no shard.|The error message returned because no shards exist in the Logstore.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

