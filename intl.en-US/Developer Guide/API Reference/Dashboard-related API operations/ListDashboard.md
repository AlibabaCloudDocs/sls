# ListDashboard

Queries the dashboard information of a specified project. The dashboard information includes dashboard IDs and names. You can call this operation to query all dashboards or a specified dashboard in the project.

## Description

The following figure shows a dashboard ID and name in the Log Service console.

![preview](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7246342161/p238997.png)

|Serial number|Description|
|-------------|-----------|
|1|The dashboard ID.|
|2|The dashboard name.|

## Request syntax

```
GET /dashboards HTTP/1.1
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

    The ListDashboard operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |dashboardName|String|No|dashboard-1609294922657-434834|The ID of the dashboard. The ID must be unique in a project. Fuzzy search is supported. For example, if you enter da, all dashboards whose names start with da are queried.|
    |displayName|String|No|data-ingest|The name of the dashboard.|
    |offset|Integer|No|0|The line from which the query starts. Default value: 0.|
    |sizes|Integer|No|10|The number of lines to return on each page in a paged query. Maximum value: 500.|


## Response elements

-   Response headers

    The ListDashboard operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. The response body contains the information of the dashboard. The following table describes the parameters of the response body.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |count|Integer|1|The number of returned dashboards on the current page.|
    |total|Integer|1|The number of dashboards that meet the query conditions.|
    |dashboards|Array|\[\{"dashboardName":"dashboard-1609294922657-434834","displayName":"data-ingest"\}\]|The list of returned dashboards.|


## Examples

-   Sample requests

    ```
    GET /dashboards HTTP/1.1
    Header:
    {
        "Content-Length": "0",
        "x-log-bodyrawsize": "0",
        "x-log-apiversion": "0.6.0",
        "x-log-signaturemethod": "hmac-sha1",
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
        "Date": "Mon, 30 Nov 2020 06:22:17 GMT",
        "Authorization": "LOG yourAccessKeyId:yourSignature",
        "x-log-date": "Mon, 30 Nov 2020 06:22:17 GMT"
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header :
    {
        "content-length": "85" 
        "server": "nginx/1.6.1"
        "connection": "close"
        "date": "Mon, 09 Nov 2015 09:19:13 GMT"
        "content-type": "application/json"
        "x-log-requestid": "5640651199248CAA2300C2BA"
    }
    Body:
    {
        "count": 1, 
        "total": 1,
        "dashboards": :[{"dashboardName":"dashboard-1609294922657-434834","displayName":"data-ingest"}]
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|The Project does not exist : projectName|The error message returned because the specified project does not exist.|
|400|ParameterInvalid|offset: offset is invalid.|The error message returned because the specified value of the offset parameter is invalid.|
|400|ParameterInvalid|sizes: sizes is invalid.|The error message returned because the specified value of the sizes parameter is invalid.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

