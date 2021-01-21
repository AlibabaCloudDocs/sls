# ListMachineGroup

You can call this operation to list the ListMachineGroup of a machine Group Project.

## Request syntax

```
GET /machinegroups? offset=1&size=100 HTTP/1.1
Authorization: AuthorizationString
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The UpdateMachineGroup API operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |offset|Integer|No|1|The start row of the query. Default value: 0.|
    |size|Integer|No|10|The maximum number of entries to return on each page. Default value: 500.|
    |groupName|String|No|test-machine-group|The name of the machine group. The machine group name that is used as a filtering condition. Fuzzy match is supported.|


## Response parameters

-   Response headers

    The UpdateMachineGroup API operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the request succeeds, the HTTP status code 200 is returned. The response body contains a list of all the machine groups in the specified project. The following table lists the parameters in the response body.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |count|Integer|2|The number of machines that are returned on the current page.|
    |total|Integer|2|The total number of the machine groups that meet the query conditions.|
    |machinegroups|Json Array|\[ "test-machine-group-1", "test-machine-group-2" \]|The list of the machine groups that meet the query conditions.|


## Examples

-   Sample requests

    ```
    GET /machinegroups? groupName=test-machine-group&offset=0&size=3 HTTP/1.1
    Header :
    {
        "x-log-apiversion": "0.6.0",
        "Authorization": "LOG yourAccessKeyId:yourSignature",
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
        "Date": "Tue, 10 Nov 2015 18:34:44 GMT",
        "Content-Length": "0",
        "x-log-signaturemethod": "hmac-sha1",
        "User-Agent": "sls-java-sdk-v-0.6.0",
        "Content-Type": "application/x-protobuf",
        "x-log-bodyrawsize": "0"
    }
    ```

-   Sample responses

    ```
    HTTP/1.1 200 OK
    Header :
    {
        "Date": "Tue, 10 Nov 2015 18:34:44 GMT",
        "Content-Length": "83",
        "x-log-requestid": "564238C499248C8F7B0001DE",
        "Connection": "close",
        "Content-Type": "application/json",
        "Server": "nginx/1.6.1"
    }
    Body :
    {
        "machinegroups":     [
            "test-machine-group-1",
            "test-machine-group-2"
        ],
        "count": 2,
        "total": 2
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|500|InternalServerError|Internal server error.|The error message returned because an internal server error has occurred.|

For more information about the error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

