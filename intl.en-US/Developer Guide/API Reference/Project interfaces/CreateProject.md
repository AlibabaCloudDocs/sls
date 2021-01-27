# CreateProject

Creates a project.

## Request syntax

```
POST / HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
x-log-bodyrawsize: 0
User-Agent: UserAgent
x-log-apiversion: 0.6.0
Host: ProjectName.Endpoint
x-log-signaturemethod: hmac-sha1
Date: GMT Date
Content-Type: application/json
Content-MD5: Content-MD5
Content-Length: ContentLength
Connection: Keep-Alive
{
  "projectName": ProjectName,
  "description": Description
}
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The CreateProject operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-project-test|The name of the project. The name must meet the following rules:    -   The name must be unique in a project.
    -   The name can contain only lowercase letters, digits, and hyphens \(-\).
    -   The name must start and end with a lowercase letter or digit.
    -   The name must be 3 to 63 characters in length. |
    |description|String|Yes|Description of my-project-test|The description of the project.|


## Response parameters

-   Response headers

    The CreateProject operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful.


## Examples

-   Sample requests

    ```
    POST / HTTP/1.1
    Header :
    {
    Content-Type: application/json
    x-log-bodyrawsize: 54
    Content-Length: 54
    Content-MD5: 6EF60FBBCE512F05F71CC2C4B1AADFCF
    x-log-apiversion: 0.6.0
    x-log-signaturemethod: hmac-sha1
    Host: ali-project-test.cn-shanghai.log.aliyuncs.com
    Date: Sun, 27 May 2018 08:25:04 GMT
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-date: Sun, 27 May 2018 08:25:04 GMT
    }
    Body :
    {
      "projectName": "my-project-test",
      "description": "Description of my-project-test"
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
|400|ProjectAlreadyExist|Project ProjectName already exist.|The error message returned because the specified project already exists.|
|400|ParameterInvalid|The body is not valid json string.|The error message returned because a parameter value is invalid.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

