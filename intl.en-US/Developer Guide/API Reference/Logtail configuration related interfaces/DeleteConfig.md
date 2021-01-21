# DeleteConfig

Deletes a specified Logtail configuration file.

## Description

If a Logtail configuration file is applied to a machine group, you cannot collect data from the machine group after you delete the file.

## Request syntax

```
DELETE /configs/configName HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

The value of the Host parameter consists of a project name and Log Service endpoint. You must specify the project in the Host parameter.

## Request parameters

-   Request headers

    The DeleteConfig operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |---------|----|--------|-------|-----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |ConfigName|String|Yes|logtail-config-sample|The name of the Logtail configuration file.|


## Response parameters

-   Response headers

    The DeleteConfig operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the request succeeds, the HTTP status code 200 is returned. If the request is successful, no other elements are returned.


## Examples

-   Sample requests

    ```
    DELETE /configs/logtail-config-sample
    Header :
    {
        "Content-Length": 0, 
        "x-log-signaturemethod": "hmac-sha1", 
        "x-log-bodyrawsize": 0, 
        "User-Agent": "log-python-sdk-v-0.6.0", 
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com", 
        "Date": "Mon, 09 Nov 2015 09:28:21 GMT", 
        "x-log-apiversion": "0.6.0", 
        "Authorization": "LOG yourAccessKeyId:yourSignature"
    }
    ```

-   Sample responses

    ```
    HTTP/1.1 200 OK
    Header : 
    {
        "date": "Mon, 09 Nov 2015 09:28:21 GMT", 
        "connection": "close", 
        "x-log-requestid": "5640673599248CAA230836C6", 
        "content-length": "0", 
        "server": "nginx/1.6.1"
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ConfigNotExist|Config configname does not exist.|The error message returned because the specified Logtail configuration file does not exist.|
|400|InvalidParameter|Invalid config resource json.|The error message returned because a parameter value is invalid.|
|500|InternalServerError|Internal server error.|The error message returned because an internal server error has occurred.|

For more information about the error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

