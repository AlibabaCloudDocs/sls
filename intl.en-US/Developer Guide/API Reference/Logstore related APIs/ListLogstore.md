# ListLogstore

Lists the Logstores in a specified project. You can call this operation to query all Logstores or a specified Logstore in the project.

## Request syntax

```
GET /logstores HTTP/1.1
Content-Length: 0
x-log-bodyrawsize': 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: ProjectName.Endpoint
Date: GMT Date
Authorization: LOG yourAccessKeyId:yourSignature
x-log-date: Mon, 30 Nov 2020 06:22:17 GMT         
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The ListLogstore operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |ProjectName|String|Yes|test-project|The name of the project.|
    |logstoreName|String|No|test-logstore|The name of the Logstore. Fuzzy match is supported. For example, if you enter test, all Logstores whose names contain test are returned.|
    |offset|Integer|No|1|The line from which the query starts. Default value: 0.|
    |size|Integer|No|10|The number of lines to return on each page in a paged query. Maximum value: 500.|


## Response parameters

-   Response headers

    The ListLogstore operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. The response body contains the information of the Logstore. The following table describes the parameters of the response body.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |count|Integer|1|The number of returned Logstores.|
    |total|Integer|1|The number of Logstores that meet the query conditions.|
    |logstores|Array|\["test-logstore"\]|The name list of the returned Logstores.|


## Examples

-   Sample requests

    ```
    GET /logstores HTTP/1.1
    Header: 
    {
    'Content-Length': '0',
    'x-log-bodyrawsize': '0',
    'x-log-apiversion': '0.6.0',
    'x-log-signaturemethod': 'hmac-sha1',
    'Host': 'ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com',
    'Date': 'Mon, 30 Nov 2020 06:22:17 GMT',
    'Authorization': 'LOG yourAccessKeyId:yourSignature',
    'x-log-date': 'Mon, 30 Nov 2020 06:22:17 GMT'
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header: 
    {
    'Server': 'Tengine',
    'Content-Type': 'application/json',
    'Content-Length': '85',
    'Connection': 'close',
    'Access-Control-Allow-Origin': '*',
    'Date': 'Mon, 30 Nov 2020 06:23:03 GMT',
    'x-log-requestid': '5FC48FC72068AF63D11472FC'
    }
    Body:
    {
    'count':1,
    'total':1,
    'logstores':["test-logstore"]
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|Project ProjectName does not exist.|The error message returned because the specified project does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|
|400|ParameterInvalid|Invalid parameter size, \(0.6.0\].|The error message returned because a parameter value is invalid.|
|400|InvalidLogStoreQuery|logstore Query is invalid.|The error message returned because the specified query is invalid.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

