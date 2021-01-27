# ListExternalStore

Queries the external stores in a specified project. You can call this operation to query all external stores or a specified external store in the project.

## Request syntax

```
GET /externalstores? externalStoreName=externalStoreName&offset=offset&sizes=sizes
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

    The ListExternalStore operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |externalStoreName|String|No|store|The name of the external store. You can query external stores that contain a specified string.|
    |offset|Integer|No|0|The line from which the query starts. Default value: 0.|
    |sizes|Integer|No|10|The number of lines to return on each page in a paged query. Maximum value: 500.|


## Response parameters

-   Response headers

    The ListExternalStore operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. The response body contains the information of the specified external store. The following table describes the parameters of the response body.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |count|Integer|3|The number of returned external stores on the current page.|
    |total|Integer|3|The number of external stores that meet the query conditions.|
    |externalstores|Array|\["ecs\_store", "rds\_store", "ecs\_store1"\]|The list of returned external stores.|


## Examples

-   Sample requests

    ```
    GET http://ali-test-project.cn-chengdu.log.aliyuncs.com:80/externalstores?externalStoreName=store&offset=0&sizes=10
    Header :
    {
    Content-Length: 0 
    x-log-bodyrawsize: 0 
    x-log-apiversion: 0.6.0 
    x-log-signaturemethod':hmac-sha1
    Host: ali-test-project.cn-chengdu.log.aliyuncs.com
    Date: Thu, 19 Apr 2018 03:03:16 GMT 
    Authorization: LOG yourAccessKeyId:yourSignature
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header :
    {
    content-length: 103 
    server: nginx/1.6.1
    connection: close
    date: Mon, 09 Nov 2015 09:19:13 GMT
    content-type: application/json
    x-log-requestid: 5640651199248CAA2300C2BA
    }
    Body:
    {
        "count": 3, 
        "total": 3,
        "externalstores": 
        [
            "ecs_store", 
            "rds_store", 
            "ecs_store1"
        ]
    
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|The Project does not exist : projectName|The error message returned because the specified project does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

