# DeleteLogstore

Deletes a Logstore, including all shards and indexes in the Logstore.

## Request syntax

```
DELETE /logstores/logstoreName HTTP/1.1
Content-Length: 0
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: ProjectName.Endpoint
Date: GMT Date
Authorization : LOG yourAccessKeyId:yourSignature
x-log-date: GMT Date
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The DeleteLogstore operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |logstoreName|String|Yes|test\_logstore|The name of the Logstore.|


## Response parameters

-   Response headers

    The DeleteLogstore operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful.


## Examples

-   Sample requests

    ```
    DELETE /logstores/test_logstore HTTP/1.1
    Header :
    {
        'Content-Length': '0',
        'x-log-bodyrawsize': '0',
        'x-log-apiversion': '0.6.0',
        'x-log-signaturemethod': 'hmac-sha1',
        'Host' : 'ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com',
        'Date' : 'Wed, 11 Nov 2015 08:09:38 GMT',
        'Authorization' : 'LOG yourAccessKeyId:yourSignature',
        'x-log-date' : 'Wed, 11 Nov 2015 08:09:38 GMT'      
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header:
    {
        'Server': 'Tengine',
        'Content-Length': '0',
        'Connection': 'close', 
        'Access-Control-Allow-Origin': '*', 
        'Date': 'Wed, 11 Nov 2015 07:35:00 GMT', 
        'x-log-requestid': '5FC0B8115F2EBE6550063F90'
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|LogStoreNotExist|logstore logstoreName does not exist|The error message returned because the specified Logstore does not exist.|
|405|InvalidMethod|invalid request method: /logstores/logstoreName|The error message returned because the value of the logstoreName parameter is invalid.|
|500|InternalServerError|Specified Server Error Message|The error message returned because an internal server error has occurred.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

