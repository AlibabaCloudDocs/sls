# DeleteConsumerGroup

Deletes a consumer group.

## Request syntax

```
DELETE /logstores/logstoreName/consumergroups/consumerGroupName HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
Host: ProjectName.Endpoint
x-log-signaturemethod: hmac-sha1
Date: GMT Date
Content-Type:  application/x-protobuf
Content-MD5: F58544E4D022CC28A93D0B7CC208A5AA
```

The value of the Host parameter consists of a project name and Log Service endpoint. You must specify the project in the Host parameter.

## Request parameters

-   Request headers

    The DeleteConsumerGroup operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |logstoreName|String|Yes|logstore-test|The name of the Logstore.|
    |consumerGroup|String|Yes|consumer-group-1|The name of the consumer group.|


## Response parameters

-   Response headers

    The DeleteConsumerGroup operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the request succeeds, the HTTP status code 200 is returned. If the request is successful, no other elements are returned.


## Examples

-   Sample requests

    ```
    DELETE /logstores/logstore-test/consumergroups/consumer-group-1 HTTP/1.1
    Header:
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: ali-test-project.cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Fri, 04 May 2018 08:02:22 GMT
    Content-Type: application/json
    Content-MD5: F58544E4D022CC28A93D0B7CC208A5AA
    Content-Length: 65
    Connection: Keep-Alive
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header :
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
|404|ProjectNotExist|The Project does not exist : projectName.|The error message returned because the specified project does not exist.|
|404|LogStoreNotExist|The logstore logstoreName dose not exist.|The error message returned because the specified Logstore does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For more information about the error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

