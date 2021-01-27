# GetLogstore

Queries the parameters of a specified Logstore.

## Request syntax

```
GET /logstores/logstoreName HTTP/1.1
Content-Length: 0
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: Projectname.Endpoint
Date: GMT Date
Authorization: LOG yourAccessKeyId:yourSignature
x-log-date: Wed, 11 Nov 2015 07:53:29 GMT
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The GetLogstore operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |logstoreName|String|Yes|test-logstore|The name of the Logstore.|


## Response parameters

-   Response headers

    The GetLogstore operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. The response body contains the detailed information of the specified Logstore. The following table lists the parameters in the response body.

    |Parameter|Type|Example|Description|
    |---------|----|-------|-----------|
    |logstoreName|String|test-logstore|The name of the Logstore.|
    |ttl|Integer|1|The data retention period. Unit: days.|
    |shardCount|Integer|2|The number of shards.|
    |enable\_tracking|Boolean|false|Specifies whether to enable the WebTracking feature. You can use the WebTracking feature to collect logs from the HTML, HTML5, iOS, and Android platforms.    -   If you set the value to true, the WebTracking feature is enabled.
    -   If you set the value to false, the WebTracking feature is disabled. |
    |autoSplit|Boolean|true|Specifies whether to enable automatic sharding.    -   If you set the value to true, automatic sharding is enabled.
    -   If you set the value to false, automatic sharding is disabled. |
    |maxSplitShard|Integer|64|The maximum number of shards for automatic sharding. Valid values: 1 to 64.|
    |appendMeta|Boolean|false|Specifies whether to record public IP addresses.    -   If you set the value to true, public IP addresses are recorded.
    -   If you set the value to false, public IP addresses are not recorded. |
    |createTime|Integer|1447833064|The timestamp when the Logstore was created. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|
    |lastModifyTime|Integer|1447833064|The timestamp when the project was last modified. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|


## Examples

-   Sample requests

    ```
    GET /logstores/test-logstore HTTP/1.1
    Header :
    {
    'Content-Length': '0',
    'x-log-bodyrawsize': '0',
    'x-log-apiversion': '0.6.0',
    'x-log-signaturemethod': 'hmac-sha1',
    'Host': 'ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com',
    'Date': 'Wed, 11 Nov 2015 07:53:29 GMT',
    'Authorization': 'LOG yourAccessKeyId:yourSignature',
    'x-log-date': 'Wed, 11 Nov 2015 07:53:29 GMT'
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header :
    {
    'Server': 'Tengine',
    'Content-Type': 'application/json',
    'Content-Length': '274',
    'Connection': 'close',
    'Access-Control-Allow-Origin': '*',
    'Date': 'Mon, 30 Nov 2020 02:27:58 GMT',
    'x-log-requestid': '5FC458AE54F7D19F960DB515'
    }
    Body :
    {
        "logstoreName": test-logstore,
        "ttl": 1,
        "shardCount": 2,
        "enable_tracking": False,
        "autoSplit": True,
        "maxSplitShard": 64,
        "createTime": 1447833064,
        "lastModifyTime": 1447833064,
        "appendMeta': False,
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|Project ProjectName does not exist.|The error message returned because the specified project does not exist.|
|404|LogstoreNotExist|logstore logstoreName does not exist.|The error message returned because the specified Logstore does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For a list of codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

