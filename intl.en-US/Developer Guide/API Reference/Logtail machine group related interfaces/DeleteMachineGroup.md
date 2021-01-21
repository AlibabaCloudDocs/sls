# DeleteMachineGroup

Deletes a machine group. If a Logtail configuration file for log collection is applied to a machine group, the configuration file is disassociated from the machine group after the machine group is deleted.

## Request syntax

```
DELETE /machinegroups/groupName HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The DeleteMachineGroup API operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |groupName|String|Yes|test-machine-group-4|The name of the machine group.|


## Response parameters

-   Response headers

    The DeleteMachineGroup API operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the request succeeds, the HTTP status code 200 is returned. If the request is successful, no other elements are returned.


## Examples

-   Sample requests

    ```
    DELETE /machinegroups/test-machine-group-4 HTTP/1.1
    Header :
    {
        "x-log-apiversion": "0.6.0",
        "Authorization": "LOG yourAccessKeyId:yourSignature",
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
        "Date": "Tue, 10 Nov 2015 19:13:28 GMT",
        "Content-Length": "0",
        "x-log-signaturemethod": "hmac-sha1",
        "User-Agent": "sls-java-sdk-v-0.6.0",
        "Content-Type": "application/x-protobuf",
        "x-log-bodyrawsize": "0"
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header :
    {
        "Date": "Tue, 10 Nov 2015 19:13:28 GMT",
        "Content-Length": "0",
        "x-log-requestid": "564241D899248C827B000CFE",
        "Connection": "close",
        "Server": "nginx/1.6.1"
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|----------------|----------|-------------|-----------|
|404|ProjectNotExist|The Project does not exist : projectName.|The error message returned because the specified project does not exist.|
|404|MachineGroupNotExist|MachineGroup groupName does not exist.|The error message returned because the specified machine group does not exist.|
|500|InternalServerError|Internal server error.|The error message returned because an internal server error has occurred.|

For more information about the error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

