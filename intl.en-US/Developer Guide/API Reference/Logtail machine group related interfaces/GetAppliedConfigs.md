# GetAppliedConfigs

Retrieves the Logtail configuration files that are applied to a machine group.

## Request syntax

```
GET /machinegroups/groupName/configs HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The GetAppliedConfigs operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |groupName|String|Yes|test-machine-group|The name of the machine group.|


## Response parameters

-   Response headers

    The GetAppliedConfigs operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the request succeeds, the HTTP status code 200 is returned. The response body contains all the Logtail configuration files that are applied to a machine group. The following table lists the parameters in the response body.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |count|Integer|3|The number of Logtail configuration files.|
    |configs|Array|\[ "two", "three", "test\_logstore" \]|The name list of the Logtail configuration files.|


## Examples

-   Sample requests

    ```
    GET /machinegroups/test-machine-group/configs HTTP/1.1
    Header :
    {
    x-log-apiversion: 0.6.0
    Authorization: LOG yourAccessKeyId:yourSignature
    Host: ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com
    Date: Tue, 10 Nov 2015 19:45:48 GMT
    Content-Length: 0
    x-log-signaturemethod: hmac-sha1
    User-Agent: sls-java-sdk-v-0.6.0
    Content-Type: application/x-protobuf
    x-log-bodyrawsize: 0
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header :
    {
    Date: Tue, 10 Nov 2015 19:45:48 GMT
    Content-Length: 53
    x-log-requestid: 5642496C99248C8C7B00173F
    Connection: close
    Content-Type: application/json
    Server: nginx/1.6.1
    }
    Body :
    {
        "configs":     [
            "two",
            "three",
            "test_logstore"
        ],
        "count": 3
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|The Project does not exist : projectName|The error message returned because the specified project does not exist.|
|404|MachineGroupNotExist|MachineGroup groupName does not exist.|The error message returned because the specified machine group does not exist.|
|500|InternalServerError|Internal server error.|The error message returned because an internal server error has occurred.|

For more information about the error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

