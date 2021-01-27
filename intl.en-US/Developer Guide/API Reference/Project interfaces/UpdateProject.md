# UpdateProject

Updates a project.

## Request syntax

```
PUT / HTTP/1.1 
Authorization: LOG yourAccessKeyId:yourSignature
x-log-bodyrawsize: 0 
User-Agent:  UserAgent
x-log-apiversion: 0.6.0 
Host:  ProjectName.Endpoint
x-log-signaturemethod: hmac-sha1 
Date:  GMT Date
Content-Type: application/json 
Content-MD5:  Content-MD5
Content-Length:  54
Connection: Keep-Alive
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The UpdateProject API operation does not have special request headers. For information about common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-project-test|The name of the project. The name is a part of the Host parameter in the header.|
    |description|String|No|Description of my-project-test|The description of the project. By default, this parameter is an empty string.|


## Response parameters

-   Response headers

    The UpdateProject operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. If the request is successful, no other elements are returned.


## Examples

-   Sample requests

    ```
    PUT / HTTP/1.1 
    Header :
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-bodyrawsize: 0 
    x-log-apiversion: 0.6.0 
    Host: ali-project-test.cn-shanghai.log.aliyuncs.com 
    x-log-signaturemethod: hmac-sha1 
    Date: Sun, 27 May 2018 07:43:26 GMT 
    Content-Type: application/json 
    Content-MD5: A7967D81EFF5E3CD447FB6D8DF294E20 
    Content-Length: 40 
    Connection: Keep-Alive 
    }
    Body :    
    { 
      "description": "Description of my-project-test" 
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header :
    {
    Server: nginx 
    Content-Length: 0 
    Connection: close 
    Access-Control-Allow-Origin: * 
    Date: Sun, 27 May 2018 07:43:27 GMT 
    x-log-requestid: 5B0A619F205DC3F30EDA9322
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|The Project does not exist : projectName.|The error message returned because the specified project does not exist.|
|400|ParameterInvalid|The body is not valid json string.|The error message returned because a parameter value is invalid.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

