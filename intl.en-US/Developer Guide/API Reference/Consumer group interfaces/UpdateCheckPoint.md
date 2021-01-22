# UpdateCheckPoint

Modifies the checkpoint of a shard for a consumer group in a specified Logstore of a project.

## Description

If you do not specify the consumer, you must set the forceSuccess parameter to true. Otherwise, the checkpoint cannot be updated.

## Request syntax

```
POST /logstores/logstoreName/consumergroups/consumerGroup HTTP/1.1
Authorization: AuthorizationString
x-log-bodyrawsize: 0
User-Agent: UserAgent
x-log-apiversion: 0.6.0
Host: ProjectName.Endpoint
x-log-signaturemethod: hmac-sha1
Date: GMT Date
Content-Type: application/json
Content-Length: 54
Connection: Keep-Alive
{
  "shard": shardID,
  "checkpoint": checkpoint
}
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The UpdateCheckPoint API operation does not have operation-specific request headers. For information about common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|my-project|The name of the project.|
    |logstoreName|String|Yes|logstore-4|The name of the Logstore.|
    |consumerGroup|String|Yes|consumer\_group\_test|The name of the consumer group.|
    |shard|Integer|Yes|0|The ID of the shard.|
    |checkpoint|String|Yes|MTUyNDE1NTM3OTM3MzkwODQ5Ng==|The value of the checkpoint.|
    |consumer|String|No|consumer\_1|The consumer.|
    |forceSuccess|Boolean|No|False|Specifies whether to enable forcible updates.    -   True: enables forcible updates.
    -   False: disables forcible updates. |


## Response parameters

-   Response headers

    The UpdateCheckPoint API operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the request succeeds, the HTTP status code 200 is returned. If the request is successful, no other elements are returned.


## Examples

-   Sample requests

    ```
    POST /logstores/logstore-4/consumergroups/consumer_group_test? forceSuccess=false&type=checkpoint&consumer=consumer_1 HTTP/1.1
    Header:
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: my-project.cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Sun, 06 May 2018 09:14:53 GMT
    Content-Type: application/json
    Content-MD5: 59457BAFB3F85A5117BDE243947583B0
    Content-Length: 55
    Connection: Keep-Alive
    }
    Body:
    {
      "shard": 0,
      "checkpoint": "MTUyNDE1NTM3OTM3MzkwODQ5Ng=="
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header:
    {
    Server: nginx/1.12.1
    Content-Length: 0
    Connection: close
    Access-Control-Allow-Origin: *
    Date: Sun, 06 May 2018 09:14:54 GMT
    x-log-requestid: 5AEEC78E1FFC0366B24F1C63
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|400|InvalidShardCheckPoint|Shard checkpoint not encoded by base64.|The error message returned because the checkpoint is not Base64-encoded.|
|404|ProjectNotExist|The Project does not exist : ProjectName.|The error message returned because the specified project does not exist.|
|404|LogStoreNotExist|Logstore \{logstoreName\} dose not exist.|The error message returned because the specified Logstore does not exist.|
|404|ConsumerGroupNotExist|Consumer group not exist.|The error message returned because the specified consumer group does not exist.|
|404|ConsumerNotExist|Consumer not exist in the consumer group|The error message returned because the consumer in the specified consumer group does not exist.|
|404|ShardNotExist|Shard not exist.|The error message returned because the specified shard does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For more information about the error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

