# ApplyConfigToMachineGroup

Applies a Logtail configuration file to a machine group.

## Request syntax

```
PUT /machinegroups/groupName HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

The value of the Host parameter consists of a project name and Log Service endpoint. You must specify the project in the Host parameter.

## Request parameters

-   Request headers

    The ApplyConfigToMachineGroup operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |groupName|String|Yes|sample-group|The name of the machine group.|
    |configName|String|Yes|logtail-config-sample|The name of the Logtail configuration file.|


## Response parameters

-   Response headers

    The ApplyConfigToMachineGroup operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the request succeeds, the HTTP status code 200 is returned. If the request is successful, no other elements are returned.


## Examples

-   Sample requests

    ```
    PUT /machinegroups/sample-group/configs/logtail-config-sample
    Header :
    {
        "Content-Length": 0, 
        "x-log-signaturemethod": "hmac-sha1", 
        "x-log-bodyrawsize": 0, 
        "User-Agent": "log-python-sdk-v-0.6.0", 
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com", 
        "Date": "Mon, 09 Nov 2015 09:44:43 GMT", 
        "x-log-apiversion": "0.6.0", 
        "Authorization": "LOG yourAccessKeyId:yourSignature"
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header:
    {
        "date": "Mon, 09 Nov 2015 09:44:43 GMT", 
        "connection": "close", 
        "x-log-requestid": "56406B0B99248CAA230BA094", 
        "content-length": "0", 
        "server": "nginx/1.6.1"
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|The Project does not exist : projectName|The error message returned because the specified project does not exist.|
|404|GroupNotExist|group groupName does not exist.|The error message returned because the specified machine group does not exist.|
|404|ConfigNotExist|Config configName does not exist.|The error message returned because the specified Logtail configuration file does not exist.|
|500|InternalServerError|Internal server error.|The error message returned because an internal server error has occurred.|

For more information about the error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

