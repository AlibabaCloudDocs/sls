# GetAppliedMachineGroups

Retrieves the list of the machines to which a Logtail configuration file is applied.

## Request syntax

```
GET /configs/configName/machinegroups HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
Date: GMT Date
Host: Projectname.Endpoint              
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The GetAppliedMachineGroups operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |configName|String|Yes|logtail-config-sample|The name of the Logtail configuration file.|


## Response parameters

-   Response headers

    The GetAppliedMachineGroups operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the request succeeds, the HTTP status code 200 is returned. The response body contains the list of the machines to which a Logtail configuration file is applied. The following table lists the parameters in the response body.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |count|Integer|1|The number of returned machine groups.|
    |machinegroups|Array|\[ "sample-group1","sample-group2" \]|The list of returned machine groups.|


## Examples

-   Sample requests

    ```
    GET /configs/logtail-config-sample/machinegroups
    Header:
    {
        "Content-Length": 0, 
        "x-log-signaturemethod": "hmac-sha1", 
        "x-log-bodyrawsize": 0, 
        "User-Agent": "log-python-sdk-v-0.6.0", 
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",   
        "Date": "Mon, 09 Nov 2015 09:51:38 GMT", 
        "x-log-apiversion": "0.6.0", 
        "Authorization": "LOG yourAccessKeyId:yourSignature"
    }
    ```

-   Sample responses

    ```
    HTTP/1.1 200 OK
    Header : 
    {
        "content-length": "44", 
        "server": "nginx/1.6.1", 
        "connection": "close", 
        "date": "Mon, 09 Nov 2015 09:51:38 GMT", 
        "content-type": "application/json", 
        "x-log-requestid": "56406CAA99248CAA230BE828"
    }
    Body:
    {
        "count": 1, 
        "machinegroups": 
        [
            "sample-group1",
            "sample-group2"
        ]
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|The Project does not exist : projectName|The error message returned because the specified project does not exist.|
|404|ConfigNotExist|Config confiName does not exist.|The error message returned because the specified Logtail configuration file does not exist.|
|500|InternalServerError|Internal server error.|The error message returned because an internal server error has occurred.|

For more information about the error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

