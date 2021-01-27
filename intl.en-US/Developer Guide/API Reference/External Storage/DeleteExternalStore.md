# DeleteExternalStore

Deletes an external store.

## Request syntax

```
DELETE /externalstores/externalStoreName
Content-Length: 0
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: ProjectName.Endpoint 
Date: GMT Date 
Authorization: LOG yourAccessKeyId:yourSignature
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The DeleteExternalStore operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |externalStoreName|String|Yes|rds\_store|The name of the external store.|


## Response parameters

-   Response headers

    The DeleteExternalStore API operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. If the request is successful, no other elements are returned.


## Examples

-   Sample requests

    ```
    DELETE /externalstores/rds_store
    Header :
    {
    Content-Length: 0
    x-log-bodyrawsize: 0
    x-log-apiversion: 0.6.0
    x-log-signaturemethod: hmac-sha1
    Host: ali-test-project.cn-chengdu.log.aliyuncs.com 
    Date: Thu, 19 Apr 2018 03:32:49 GMT
    Authorization: LOG yourAccessKeyId:yourSignature
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header
    {
        date: Mon, 09 Nov 2015 07:45:30 GMT
        connection: close
        x-log-requestid: 56404F1A99248CA26C002180
        content-length: 0
        server: nginx/1.6.1
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|The Project does not exist : projectName|The error message returned because the specified project does not exist.|
|400|ParameterInvalid|External store does not exist|The error message returned because the specified external store does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

