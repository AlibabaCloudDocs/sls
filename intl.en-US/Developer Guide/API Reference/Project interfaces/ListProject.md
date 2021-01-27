# ListProject

Lists the projects that meet specified conditions.

## Request syntax

```
GET /? offset=offset&size=size HTTP/1.1
Content-Length: 0
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: Endpoint
Date: GMT Date
Authorization: LOG yourAccessKeyId:yourSignature
x-log-date: GMT Date
```

## Request parameters

-   Request headers

    The ListProject operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |offset|Integer|No|0|The line from which the query starts. Default value: 0.|
    |size|Integer|No|10|The number of lines to return on each page in a paged query. Default value: 100. Maximum value: 500.|


## Response parameters

-   Response headers

    The ListProject operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. The response body contains the information of the project list. The following table describes the parameters of the response body.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |count|Integer|2|The number of returned projects on the current page.|
    |total|Integer|11|The number of projects that meets the query conditions.|
    |projects|Array|None|The project lists that meet the query conditions.|

    The following table describes the parameters of the projects parameter.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |createTime|String|1524222931|The timestamp when the project was created. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|
    |description|String|test|The description of the project.|
    |lastModifyTime|String|1524539357|The timestamp when the project was last modified. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|
    |owner|String|12\*\*\*\*34|The ID of the Alibaba Cloud account that was used to create the project.|
    |projectName|String|project1|The name of the project.|
    |status|String|Normal|The status of the project. Valid values: Normal and Disable.    -   Normal: The status of the project is normal.
    -   Disable: The status of the project is disabled. |
    |region|String|cn-shanghai|The region to which the project belongs.|


## Examples

-   Sample requests

    ```
    GET /? offset=0&size=2 HTTP/1.1
    Header :
    {
    Content-Length: 0
    x-log-bodyrawsize: 0
    x-log-apiversion: 0.6.0
    x-log-signaturemethod: hmac-sha1
    Host: cn-shanghai.log.aliyuncs.com
    Date: Sun, 27 May 2018 09:03:33 GMT
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-date: Sun, 27 May 2018 09:03:33 GMT
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header :
    {
        Server: nginx
        Content-Type: application/json
        Content-Length: 345
        Connection: close
        Access-Control-Allow-Origin: *
        Date: Sun, 27 May 2018 09:03:33 GMT
        x-log-requestid: 5B0A7465AAEA20CA70DE3064
    }
    Body :
    {
      "count": 2,
      "total": 11,
      "projects": [
        {
          "projectName": "project1",
          "status": "Normal",
          "owner": "12****34",
          "description": "test",
          "region": "cn-shanghai",
          "createTime": "1524222931",
          "lastModifyTime": "1524539357"
        },
        {
          "projectName": "project123456",
          "status": "Normal",
          "owner": "12****34",
          "description": "test",
          "region": "cn-shanghai",
          "createTime": "1471963876",
          "lastModifyTime": "1524539357"
        }
      ]
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|400|ParameterInvalid|offset : offset pair is invalid|The error message returned because the specified value of the offset parameter is invalid.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

