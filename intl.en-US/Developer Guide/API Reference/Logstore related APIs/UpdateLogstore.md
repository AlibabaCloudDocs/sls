# UpdateLogstore

Modifies the parameters of a Logstore.

## Description

Only the ttl \(data retention period\) parameter can be modified.

## Request syntax

```
PUT /logstores/logstoreName HTTP/1.1
Content-Length: 0
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: ProjectName.Endpoint
Date: GMT Date
Authorization : LOG yourAccessKeyId:yourSignature 
x-log-date: Wed, 11 Nov 2015 08:28:19 GMT
{
    "logstoreName": logstoreName,
    "ttl": ttl,
    "enable_tracking": false,
    "shardCount": shardCount,
    "autoSplit": true,
    "maxSplitShard": 64,
    "appendMeta": false
}
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request elements

-   Request headers

    The UpdateLogstore operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |logstoreName|String|Yes|test-logstore|The name of the Logstore.|
    |ttl|Integer|Yes|1|The data retention period. Unit: days. Valid values: 1 to 3650. If you set the value to 3650, the data is permanently stored.|
    |enable\_tracking|Boolean|No|false|Specifies whether to enable the WebTracking feature.    -   If you set the value to true, the WebTracking feature is enabled.
    -   If you set the value to false, the WebTracking feature is disabled. |
    |shardCount|Integer|Yes|2|The number of shards.**Note:**

    -   You cannot call the UpdateLogstore operation to modify this parameter.
    -   However, you can call the [SplitShard](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/SplitShard.md) or [MergeShards](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/MergeShards.md) operation to modify this parameter. |
    |autoSplit|Boolean|No|true|Specifies whether to enable automatic sharding.    -   If you set the value to true, automatic sharding is enabled.
    -   If you set the value to false, automatic sharding is disabled. |
    |maxSplitShard|Integer|No|64|The maximum number of shards for automatic sharding. Valid values: 1 to 64.**Note:** This parameter must be specified if the autoSplit parameter is set to true. |
    |appendMeta|Boolean|No|false|Specifies whether to record public IP addresses.    -   If you set the value to true, public IP addresses are recorded.
    -   If you set the value to false, public IP addresses are not recorded. |


## Response parameters

-   Response headers

    The UpdateLogstore operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful.


## Examples

-   Sample requests

    ```
    PUT /logstores/test-logstore HTTP/1.1
    Header:
    {
    'x-log-bodyrawsize': '0',
    'Content-Type': 'application/json',
    'Content-Length': '143',
    'Content-MD5': '118D09033B9A4DCBA93A8A496EA095CF',
    'x-log-apiversion': '0.6.0', 
    'x-log-signaturemethod': 'hmac-sha1',
    'Host': 'ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com',
    'Date': 'Wed, 11 Nov 2015 08:28:19 GMT', 
    'Authorization': 'LOG yourAccessKeyId:yourSignature', 
    'x-log-date': 'Wed, 11 Nov 2015 08:28:19 GMT'
    }
    Body : 
    {
        "logstoreName": "test-logstore",
        "ttl": 1,
        "enable_tracking": false,
        "shardCount": 2,
        "autoSplit": true,
        "maxSplitShard": 64
        "appendMeta": false
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header:
    {
    'Server': 'Tengine',
    'Content-Length': '0', 
    'Connection': 'close', 
    'Access-Control-Allow-Origin': '*', 
    'Date': 'Wed, 11 Nov 2015 08:28:20 GMT', 
    'x-log-requestid': '5FC4490F5D13436DA713CBEE'
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|Project ProjectName does not exist.|The error message returned because the specified project does not exist.|
|404|LogStoreNotExist|logstore logstoreName does not exist.|The error message returned because the specified Logstore does not exist.|
|400|LogStoreAlreadyExist|logstore logstoreName already exists.|The error message returned because the specified Logstore already exists.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|
|400|ParameterInvalid|invalid shard count,you can only modify shardCount by split& merge shard.|The error message returned because the specified number of shards is invalid. You can modify the number of shards by calling only the SplitShard or MergeShards operation.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

