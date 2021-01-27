# GetProject

Queries the details of a project.

## Request syntax

```
GET / HTTP/1.1
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

    The GetProject operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-project-test|The name of the project.|


## Response parameters

-   Response headers

    The GetProject operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the GetProject request is successful. The response body contains the detailed information of the project. The following table describes the parameters of the response body.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |createTime|String|2020-11-18 16:55:57|The time when the project was created.|
    |description|String|test|The description of the project.|
    |lastModifyTime|String|2020-11-18 17:07:26|The time when the project was last modified.|
    |owner|String|174\*\*\*\*745|The ID of the Alibaba Cloud account that was used to create the project.|
    |projectName|String|ali-project-test|The name of the project.|
    |status|String|Normal|The status of the project.|
    |region|String|cn-hangzhou|The region to which the project belongs.|


## Examples

-   Sample requests

    ```
    GET / HTTP/1.1
    Header :
    {
    Content-Length: 0
    x-log-bodyrawsize: 0
    x-log-apiversion: 0.6.0
    x-log-signaturemethod: hmac-sha1
    Host: ali-project-test.cn-shanghai.log.aliyuncs.com
    Date: Sun, 27 May 2018 08:25:04 GMT
    Authorization: LOG yourAccessKeyId:yourSignature
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
    Body :
    {
        "createTime": "2020-11-18 16:55:57",
        "description": "test",
        "lastModifyTime": "2020-11-18 17:07:26",
        "owner": "174****745",
        "projectName": "ali-project-test",
        "region": "cn-hangzhou",
        "status": "Normal"
    }
                        
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|The Project does not exist : projectName|The error message returned because the specified project does not exist.|
|500|InternalServerError|Specified Server Error Message|The error message returned because an internal server error has occurred.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

