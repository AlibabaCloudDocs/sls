# ListConfig

You can call this operation to query all Logtail configurations of a specified Project.

## Request syntax

```
GET /configs? offset=0&size=100 HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The ListConfig operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |offset|Integer|No|0|The start row of the query. Default value: 0.|
    |size|Integer|No|10|The maximum number of entries to return on each page. Default value: 500.|
    |logstoreName|String|No|logstore-4|The name of the Logstore.|
    |configName|String|No|logtail-config-sample|The name of the Logtail configuration file.|


## Response parameters

-   Response headers

    The ListConfig operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the request succeeds, the HTTP status code 200 is returned. The response body contains a list of all Logtail configuration files of the project. The following table lists the parameters in the response body.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |count|Integer|3|The number of Logtail configuration files that are returned on the current page.|
    |total|Integer|3|The total number of Logtail configuration files that meet the query conditions.|
    |configs|Array|\[ "logtail-config-sample", "logtail-config-sample-2", "logtail-config-sample-3" \]|The list of the Logtail configuration files that are returned.|


## Examples

-   Sample requests

    ```
    GET /configs? offset=0&size=10 HTTP/1.1
    Header :
    {
        "Content-Length": 0, 
        "x-log-signaturemethod": "hmac-sha1", 
        "x-log-bodyrawsize": 0, 
        "User-Agent": "log-python-sdk-v-0.6.0", 
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com", 
        "Date": "Mon, 09 Nov 2015 09:19:13 GMT", 
        "x-log-apiversion": "0.6.0", 
        "Authorization": "LOG yourAccessKeyId:yourSignature"
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header :
    {
        "content-length": "103", 
        "server": "nginx/1.6.1", 
        "connection": "close", 
        "date": "Mon, 09 Nov 2015 09:19:13 GMT", 
        "content-type": "application/json", 
        "x-log-requestid": "5640651199248CAA2300C2BA"
    }
    Body:
    {
        "count": 3, 
        "total": 3,
        "configs": 
        [
            "logtail-config-sample", 
            "logtail-config-sample-2", 
            "logtail-config-sample-3"
        ]
    
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|Project ProjectName does not exist.|The error message returned because the specified project does not exist.|
|404|LogstoreNotExist|Logstore logstoreName does not exist.|The error message returned because the specified Logstore does not exist.|
|404|ConfigNotExist|config configName does not exist.|The error message returned because the specified Logtail configuration file does not exist.|
|500|InternalServerError|Internal server error.|The error message returned because an internal server error has occurred.|

For more information about the error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

