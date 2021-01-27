# DeleteIndex

Deletes the indexes for a specified Logstore.

## Request syntax

```
DELETE /logstores/logstoreName/index HTTP/1.1
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

    The DeleteIndex operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|my-project|The name of the project.|
    |logstoreName|String|Yes|my-logstore|The name of the Logstore.|


## Response parameters

-   Response headers

    The DeleteIndex operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. If the request is successful, no other elements are returned.


## Examples

-   Sample requests

    ```
    DELETE /logstores/my-logstore/index HTTP/1.1
    Header :
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: my-project.cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Sun, 06 May 2018 13:22:57 GMT
    Content-Type: application/x-protobuf
    Connection: Keep-Alive
    ```

-   Sample success responses

    ```
    HTTP/1.1 200
    
    Server: nginx/1.12.1
    Content-Length: 0
    Connection: close
    Access-Control-Allow-Origin: *
    Date: Sun, 06 May 2018 13:22:57 GMT
    x-log-requestid: 5AEF01B1A458E41C2F8326A8
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|400|ParameterInvalid|Log store index not created.|The error message returned because no indexes were created for the specified Logstore.|
|404|ProjectNotExist|The Project does not exist : projectName.|The error message returned because the specified project does not exist.|
|404|LogStoreNotExist|logstore logstoreName does not exist.|The error message returned because the specified Logstore does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

