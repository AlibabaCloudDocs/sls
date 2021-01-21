# GetCheckPoint

Queries the checkpoints of the shards from which data is consumed by a consumer group.

## Request syntax

```
GET /logstores/logstoreName/consumergroups/consumerGroup? shard=shardId HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
Host: ProjectName.Endpoint
x-log-signaturemethod: hmac-sha1
Date: GMT Date
Content-Type: application/json
Connection: Keep-Alive
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The GetCheckPoint does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|my-project|The name of the project.|
    |logstoreName|String|Yes|my-logstore|The name of the Logstore.|
    |consumerGroup|String|Yes|consumer\_group\_test|The name of the consumer group.|
    |shardId|Integer|No|0|The ID of the shard.    -   If the specified shard does not exist, an empty list is returned.
    -   If the shard ID is not specified, the checkpoints of all shards are returned. |


## Response parameters

-   Response headers

    The GetCheckPoint operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the request succeeds, the HTTP status code 200 is returned. The response body contains the checkpoints of the shards from which data is consumed by the specified consumer group. The following table lists the parameters in the response body.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |shard|Integer|0|The ID of the shard.|
    |checkpoint|String|MTUyNDE1NTM3OTM3MzkwODQ5Ng==|The value of the checkpoint.|
    |updateTime|Integer|1524224984800922|The time when the checkpoint was last updated. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00, January 1, 1970.|
    |consumer|String|consumer\_1|The consumer.|


## Examples

-   Sample requests

    ```
    GET /logstores/my-logstore/consumergroups/consumer_group_test? shard=0
    Header:
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: my-project.cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Fri, 04 May 2018 09:26:53 GMT
    Content-Type: application/x-protobuf
    Connection: Keep-Alive
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header:
    {
    Server: nginx/1.12.1
    Content-Type: application/json
    Content-Length: 111
    Connection: close
    Access-Control-Allow-Origin: *
    Date: Fri, 04 May 2018 09:26:53 GMT
    x-log-requestid: 5AEC275D1FFC036AB254B20C
    }
    Body:
    {
    [
      {
        "shard": 0,
        "checkpoint": "MTUyNDE1NTM3OTM3MzkwODQ5Ng==",
        "updateTime": 1524224984800922,
        "consumer": "consumer_1"
      }
    ]
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|The Project does not exist : projectName.|The error message returned because the specified project does not exist.|
|404|LogStoreNotExist|Logstore logstoreName dose not exist.|The error message returned because the specified Logstore does not exist.|
|404|ConsumerGroupNotExist|Consumer group not exist.|The error message returned because the specified consumer group does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For more information about the error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

