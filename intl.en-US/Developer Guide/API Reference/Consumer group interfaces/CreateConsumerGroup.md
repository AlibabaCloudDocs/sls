# CreateConsumerGroup

Creates a consumer group in a Logstore.

## Description

You can create a maximum of 30 consumer groups for a Logstore.

## Request syntax

```
POST /logstores/logstoreName/consumergroups HTTP/1.1
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
{
  "consumerGroup": consumerGroup,
  "timeout": timeout,
  "order": order
}
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The CreateConsumerGroup operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|my-project|The name of the project.|
    |logstoreName|String|Yes|my-logstore|The name of the Logstore.|
    |consumerGroup|String|Yes|consumer-group-1|The name of the consumer group. The name must be unique in a project.|
    |timeout|Integer|Yes|300|The timeout period. The timeout period. If Log Service does not receive heartbeat messages from the consumer within the timeout period, the consumer is deleted. Unit: seconds.|
    |order|Boolean|No|true|Specifies whether to consume data in sequence from a single shard. Valid values:     -   true: The data in a single shard is consumed in sequence. After the shard is split, the data in the original shard is consumed. Then, the data in the new shards is consumed at the same time.
    -   false: The data in a single shard is randomly consumed. |


## Response parameters

-   Response headers

    The CreateConsumerGroup operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the request succeeds, the HTTP status code 200 is returned. If the request is successful, no other elements are returned.


## Examples

-   Sample requests

    ```
    POST /logstores/my-logstore/consumergroups HTTP/1.1
    Header :
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: my-project.cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Fri, 04 May 2018 08:02:22 GMT
    Content-Type: application/json
    Content-MD5: F58544E4D022CC28A93D0B7CC208A5AA
    Content-Length: 65
    Connection: Keep-Alive
    }
    Body :
    {
      "consumerGroup": "consumer-group-1",
      "timeout": 300,
      "order": true
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
    Date: Fri, 04 May 2018 08:15:11 GMT
    x-log-requestid: 5AEC168FA796F4195BF404CB
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|400|ConsumerGroupAlreadyExist|Consumer group already exist.|The error message returned because the specified consumer group already exists.|
|400|JsonInfoInvalid|ConsumerGroup or timeout is of error format.|The error message returned because the value of the consumerGroup or timeout parameter is in an invalid format.|
|404|ProjectNotExist|The Project does not exist : ProjectName.|The error message returned because the specified project does not exist.|
|404|LogStoreNotExist|Logstore logstoreName dose not exist.|The error message returned because the specified Logstore does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For more information about the error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

