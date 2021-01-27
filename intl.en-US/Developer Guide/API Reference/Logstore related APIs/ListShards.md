# ListShards

Lists all available shards of a Logstore.

## Request syntax

```
GET /logstores/logstorename/shards HTTP/1.1
Content-Length: 0
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod': hmac-sha1
Host: ProjectName.Endpoint
Date: GMT Date
Authorization: LOG yourAccessKeyId:yourSignature
x-log-date: Mon, 30 Nov 2020 06:22:17 GMT   
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The ListShards operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |logstoreName|String|Yes|sls-test-logstore|The name of the Logstore.|


## Response parameters

-   Response headers

    The ListShards operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. After the request succeeds, the response body contains the information of the shard. The following table describes the parameters of the response body.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |shardID|Integer|0|The ID of the shard.|
    |status|String|readwrite|The status of the shard.|
    |inclusiveBeginKey|String|00000000000000000000000000000000|Indicates the start of the hash key range. The value of this parameter is included in the range.|
    |exclusiveEndKey|String|8000000000000000000000000000000|Indicates the end of the hash key range. The value of this parameter is excluded in the range.|
    |createTime|String|1453949705|The timestamp when the shard was created. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|


## Examples

-   Sample requests

    ```
    GET /logstores/sls-test-logstore/shards
    Header :
    {
    'Content-Length': '0'
    'x-log-bodyrawsize': '0'
    'x-log-apiversion': '0.6.0'
    'x-log-signaturemethod': 'hmac-sha1'
    'Host': ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com
    'Date': Mon, 30 Nov 2020 06:22:17 GMT
    'Authorization': 'LOG yourAccessKeyId:yourSignature'
    'x-log-date': 'Mon, 30 Nov 2020 06:22:17 GMT'
    }
    ```

-   Sample responses

    ```
    Header:
    {
    'Server': 'Tengine',
    'Content-Type': 'application/json',
    'Content-Length': '335',
    'Connection': 'close',
    'Access-Control-Allow-Origin': '*',
    'Date': 'Mon, 30 Nov 2020 08:32:24 GMT',
    'x-log-requestid': '5FC4AE1821CA7D41C5F14728'   
    }
    Body :
    [
        {
            "shardID": 1,
            "status": "readwrite",
            "inclusiveBeginKey": "00000000000000000000000000000000",
            "exclusiveEndKey": "8000000000000000000000000000000",
            "createTime": 1453949705
        },
        {
            "shardID": 2,
            "status": "readwrite",
            "inclusiveBeginKey": "80000000000000000000000000000000",
            "exclusiveEndKey": "ffffffffffffffffffffffffffffffff",
            "createTime": 1453949705
        },
        {
            "shardID": 0,
            "status": "readonly",
            "inclusiveBeginKey": "00000000000000000000000000000000",
            "exclusiveEndKey": "ffffffffffffffffffffffffffffffff",
            "createTime": 1453949705
        }
    ]
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|The Project does not exist : projectName|The error message returned because the specified project does not exist.|
|404|LogStoreNotExist|logstore logstoreName does not exist.|The error message returned because the specified Logstore does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|
|400|LogStoreWithoutShard|logstore has no shard.|The error message returned because no shards exist in the Logstore.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

