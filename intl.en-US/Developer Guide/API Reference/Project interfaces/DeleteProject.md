# DeleteProject

Deletes a specified project.

## Request syntax

```
DELETE / HTTP/1.1
Content-Length: 0
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: ProjectName.Endpoint
Date: GMT Date
Authorization: LOG yourAccessKeyId:yourSignature
x-log-date: GMT Date
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The DeleteProject operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-project-test|The name of the project. The name is a part of the Host parameter in the header.|


## Response parameters

-   Response headers

    The DeleteProject operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. If the request is successful, no other elements are returned.


## Examples

-   Sample requests

    ```
    DELETE / HTTP/1.1
    Header :
    {
    Content-Length: 0
    x-log-bodyrawsize: 0
    x-log-apiversion: 0.6.0
    x-log-signaturemethod: hmac-sha1
    Host: ali-project-test.cn-shanghai.log.aliyuncs.com
    Date: Sun, 27 May 2018 08:25:04 GMT
    Authorization: LOG yourAccessKeyId:/yourSignature
    x-log-date: Sun, 27 May 2018 08:25:04 GMT
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header :
    {
    Server: nginx
    Content-Type: application/json
    Content-Length: 0
    Connection: close
    Access-Control-Allow-Origin: *
    Date: Sun, 27 May 2018 08:25:04 GMT
    x-log-requestid: 5B0A6B60BB6EE39764D458B5
    }                    
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|The Project does not exist : projectName.|The error message returned because the specified project does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

