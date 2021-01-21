# HeartBeat

Sends heartbeat messages to Log Service for a specified consumer.

## Description

Heartbeat messages can be sent only when the consumer is connected to Log Service.

## Request syntax

```
POST /logstores/logstoreName/consumergroups/consumerGroup? type=heartbeat&consumer=<consumer> HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
x-log-bodyrawsize: 0
User-Agent: UserAgent
x-log-apiversion: 0.6.0
Host: ProjectName.Endpoint
x-log-signaturemethod: hmac-sha1
Date: GMT Date
Content-Type: application/json
Content-MD5: F58544E4D022CC28A93D0B7CC208A5AA
Content-Length: 54
Body :
{ "shards" :shard ID list} 
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The HeartBeat operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|my-project|The name of the project.|
    |logstoreName|String|Yes|my-logstore|The name of the Logstore. The name must be unique in a project.|
    |consumerGroup|String|Yes|consumer\_group\_test|The name of the consumer group. The name must be unique in a project.|
    |consumer|String|Yes|consumer\_1|The consumer.|
    |shards|Array|An array of node roles.|\[0\]|The ID list of shards from which data is being consumed.|


## Response parameters

-   Response headers

    The HeartBeat operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the request succeeds, the HTTP status code 200 is returned. In addition, the ID list of all shards from which data is consumed by the consumer is returned.


## Examples

-   Sample requests

    ```
    POST /logstores/my-logstore/consumergroups/consumer_group_test? type=heartbeat&consumer=consumer_1 HTTP/1.1
    Header :
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: my-project.cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Sun, 06 May 2018 09:44:10 GMT
    Content-Type: application/json
    Content-MD5: 8D5162CA104FA7E79FE80FD92BB657FB
    Content-Length: 3
    Connection: Keep-Alive
    }
    Body :
    { "shards" : [0]}
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header :
    {
    Server: nginx/1.12.1
    Content-Type: application/json
    Content-Length: 5
    Connection: close
    Access-Control-Allow-Origin: *
    Date: Sun, 06 May 2018 09:44:11 GMT
    x-log-requestid: 5AEECE6B1FFC0366B2553FB5
    }
    Body :
    { "shards" : [0,1]}
    ```


## Error code

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|400|NotExistConsumerWithBody|Non-exist consumer with non-empty body of heartbeat message.|The error message returned because a consumer with non-empty heartbeat messages does not exist.|
|404|ProjectNotExist|The Project does not exist : ProjectName.|The error message returned because the specified project does not exist.|
|404|LogStoreNotExist|Logstore logstoreName dose not exist.|The error message returned because the specified Logstore does not exist.|
|404|ConsumerGroupNotExist|Consumer group not exist.|The error message returned because the specified consumer group does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For more information about the error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

