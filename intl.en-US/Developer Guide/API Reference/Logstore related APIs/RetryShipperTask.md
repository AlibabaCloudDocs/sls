# RetryShipperTask

Restarts the failed LogShipper tasks.

## Description

You can restart up to 10 failed LogShipper tasks at a time.

## Request syntax

```
PUT /logstores/logstoreName/shipper/shipperName/tasks HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
["task-id-1", "task-id-2", "task-id-2"]
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The RetryShipperTask operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |logstoreName|String|Yes|test-logstore|The name of the Logstore.|
    |shipperName|String|Yes|test-shipper|The name of the LogShipper task.|


## Response parameters

-   Response headers

    The RetryShipperTask operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. If the request is successful, no other elements are returned.


## Examples

-   Sample requests

    ```
    PUT /logstores/test-logstore/shipper/test-shipper/tasks HTTP/1.1
    Header:
    {
        "x-log-apiversion" : 0.6.0, 
        "Authorization" : "LOG yourAccessKeyId:yourSignature", 
        "Host" : "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com", 
        "Date" : "Wed, 11 Nov 2015 08:28:19 GMT", 
        "Content-Length" : 55, 
        "x-log-signaturemethod" : "hmac-sha1", 
        "Content-MD5" : "757C60FC41CC7D3F60B88E0D916D051E", 
        "User-Agent" : "sls-java-sdk-v-0.6.0", 
        "Content-Type" : "application/json"
    }
    Body : 
    ["task-id-1", "task-id-2", "task-id-2"]
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header:
    {
        "Date" : "Wed, 11 Nov 2015 08:28:20 GMT", 
        "Content-Length" : 0, 
        "x-log-requestid" : "5642FC2399248C8F7B0145FD", 
        "Connection" : "close", 
        "Server" : "nginx/1.6.1"
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|Project projectName does not exist.|The error message returned because the specified project does not exist.|
|404|LogStoreNotExist|Logstore logstoreName does not exist.|The error message returned because the specified Logstore does not exist.|
|400|ShipperNotExist|Shipper does not exist.|The job does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|
|400|ParameterInvalid|Each time allows 10 task retries only.|The error message returned because more than 10 tasks are restarted at a time.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

