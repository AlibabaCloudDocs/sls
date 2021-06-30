# CreateLogstore

Creates a Logstore.

## Request syntax

```
POST /logstores HTTP/1.1
x-log-bodyrawsize: 0
Content-Type: application/json
Content-Length: 140
Content-MD5: F3EFCA28442BEEC487451FAD30D78650
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: ProjectName.Endpoint
Date: GMT Date
Authorization: LOG yourAccessKeyId:yourSignature,
x-log-date: Fri, 27 Nov 2020 08:25:10 GMT
{
    "logstoreName" : logStoreName,
    "ttl": ttl,
    "shardCount": shardCount,
    "enable_tracking": enable\_tracking,
    "autoSplit": autoSplit,
    "maxSplitShard": maxSplitShard,
    "appendMeta": appendMeta
}
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name in the Host parameter.

## Request parameters

-   Request headers

    The CreateLogstore operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |logstoreName|String|Yes|test-logstore|The name of the Logstore. The name must meet the following requirements:    -   The name must be unique in a project.
    -   The name can contain only lowercase letters, digits, hyphens \(-\), and underscores \(\_\).
    -   The name must start and end with a lowercase letter or digit.
    -   The name must be 3 to 63 characters in length. |
    |ttl|Integer|Yes|30|The data retention period. Unit: days. Valid values: 1 to 3650. If you set the value to 3650, the data is permanently stored.|
    |shardCount|Integer|Yes|2|The number of shards. Valid values: 1 to 10.|
    |enable\_tracking|Boolean|No|false|Specifies whether to enable the WebTracking feature. Default value: false.     -   true: enables the WebTracking feature
    -   false: disables the WebTracking feature |
    |autoSplit|Boolean|No|true|Specifies whether to enable automatic sharding. Default value: false.     -   true: enables automatic sharding
    -   false: disables automatic sharding |
    |maxSplitShard|Integer|No|6|The maximum number of shards for automatic sharding. Valid values: 1 to 64. If the autoSplit parameter is set to true, you must specify the maxSplitShard parameter.|
    |appendMeta|Boolean|No|false|Specifies whether to record public IP addresses. Default value: false.     -   If you set the value to true, public IP addresses are recorded.
    -   If you set the value to false, public IP addresses are not recorded. |


## Response parameters

-   Response headers

    The CreateLogstore operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the Logstore is created.


## Examples

-   Sample requests

    ```
    POST /logstores HTTP/1.1
    Header :
    {
        'x-log-bodyrawsize': '0',
        'Content-Type': 'application/json',
        'Content-Length': '143',
        'Content-MD5': '2ABE4729F2FE8031328CFA4B9D8A34FB',
        'x-log-apiversion': '0.6.0',
        'x-log-signaturemethod': 'hmac-sha1',
        'Host': 'ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com,
        'Date' : 'Wed, 11 Nov 2015 07:35:00 GMT',
        'Authorization': 'LOG yourAccessKeyId:yourSignature,
        'x-log-date': 'Wed, 11 Nov 2015 07:35:00 GMT'
    
    }
    Body : 
    {
        "logstoreName": "test-logstore",
        "ttl": 30,
        "shardCount": 2,
        "enable_tracking": false,
        "autoSplit": true,
        "maxSplitShard": 6,
        "appendMeta": false
    }
    ```

-   Sample responses

    ```
    HTTP/1.1 200 OK
    Header:
    {
        'Server': 'Tengine',
        'Content-Length': '0',
        'Connection': 'close', 
        'Access-Control-Allow-Origin': '*', 
        'Date': 'Wed, 11 Nov 2015 07:35:00 GMT', 
        'x-log-requestid': '5FC0B8115F2EBE6550063F90'
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|Project ProjectName does not exist.|The error message returned because the specified project does not exist.|
|400|LogstoreAlreadyExist|logstore logstoreName already exists.|The error message returned because the specified Logstore already exists.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|
|400|LogStoreInfoInvalid|store name logstoreName is invalid|The error message returned because the Logstore information is invalid.|
|400|ProjectQuotaExceed|Project Quota Exceed.|The error message returned because the project quota has been exceeded.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

