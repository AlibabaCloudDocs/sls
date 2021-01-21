# ListConsumerGroup

Lists all consumer groups of a Logstore.

## Request syntax

```
GET /logstores/logstoreName/consumergroups HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
Host: ProjectName.Endpoint
x-log-signaturemethod: hmac-sha1
Date: GMT Date
Content-Type: application/x-protobuf
Connection: Keep-Alive
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The ListConsumerGroup operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|my-project|The name of the project.|
    |logstoreName|String|Yes|my-logstore|The name of the Logstore.|


## Response parameters

-   Response headers

    The ListConsumerGroup operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the request succeeds, the HTTP status code 200 is returned. The response body lists all consumer groups of the specified consumer group in the specified project. The following table lists the parameters of the response body.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |count|Integer|1|The number of consumer groups.|
    |consumerGroups|Array|\[\{''name'': ''test-consumer-group'', ''timeout'': 30, ''order'': False\}\]|The list of consumer groups. For more information, see the following table.|

    The consumerGroups parameter consists of the following parameters.

    |Parameter|Type|Example|Description|
    |---------|----|-------|-----------|
    |name|String|test-consumer-group|The name of the consumer group.|
    |timeout|Integer|30|The timeout period.|
    |order|Boolean|true|Specifies whether to consume log data in sequence.|


## Examples

-   Sample requests

    ```
    GET /logstores/my-logstore/consumergrops HTTP/1.1
    Header :
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: my-project.cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Fri, 04 May 2018 08:47:30 GMT
    Content-Type: application/x-protobuf
    Connection: Keep-Alive
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header :
    {
    Server: nginx/1.12.1
    Content-Type: application/json
    Connection: close
    Access-Control-Allow-Origin: *
    Date: Fri, 04 May 2018 08:47:31 GMT
    x-log-requestid: 5AEC1E23048191954B42EAB9
    }
    Body :
    {
        "count": "1",
        "consumerGroups": 
    [
      {
        "name": "test-consumer-group",
        "timeout": 30,
        "order": False
      }
    ]
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|The Project does not exist : projectName.|The error message returned because the specified project does not exist.|
|404|LogStoreNotExist|Logstore logstoreName dose not exist.|The error message returned because the specified Logstore does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For more information about the error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

