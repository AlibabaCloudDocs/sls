# CreateConfig

Creates a Logtail configuration file.

## Description

You can create a maximum of 100 Logtail configuration files in a project.

## Request syntax

```
POST /configs HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
Content-Type:application/json
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
{
    "configName": "testcategory1",
    "inputType": "file",
    "inputDetail": {
        "logType": "common_reg_log",
        "logPath": "/var/log/httpd/",
        "filePattern": "access*.log",
        "localStorage": true,
        "timeFormat": "%Y/%m/%d %H:%M:%S",
        "logBeginRegex": ".*",
        "regex": "(\w+)(\s+)",
        "key" :["key1", "key2"],
        "filterKey":["key1"],
        "filterRegex":["regex1"],
        "fileEncoding":"utf8",
        "topicFormat": "none"
    },
    "outputType": "LogService",
    "outputDetail": 
    {
        "logstoreName": "perfcounter"
    }
}
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The CreateConfig operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Request parameters

    For information about the request parameters and examples of Logtail configuration files in various modes, see [Logtail configuration files](/intl.en-US/Developer Guide/API Reference/Common resources/Logtail configuration files.md).


## Response parameters

-   Response headers

    The CreateConfig operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the request succeeds, the HTTP status code 200 is returned. If the request is successful, no other elements are returned.


## Examples

-   Sample requests

    ```
    POST /configs HTTP/1.1
    Header :
    {
    Content-Length: 737
    Host: ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com 
    x-log-bodyrawsize: 737 
    Content-MD5: FBA01ECF7255BE143379BC70C56BBF68
    x-log-signaturemethod: hmac-sha1
    Date: Mon, 09 Nov 2015 07:45:30 GMT
    x-log-apiversion: 0.6.0
    User-Agent: log-python-sdk-v-0.6.0
    Content-Type: application/json
    Authorization: LOG yourAccessKeyId:yourSignature
    }
    Body:
    {
        "configName": "sample-logtail-config",
        "inputType": "file",
        "inputDetail": {
            "logType": "common_reg_log", 
            "logPath": "/var/log/httpd/",
            "filePattern": "access*.log",
            "localStorage": true, 
            "timeFormat": "%d/%b/%Y:%H:%M:%S", 
            "logBeginRegex": "\\d+\\.\\d+\\.\\d+\\.\\d+ - .*", 
            "regex": "([\\d\\.] +) \\S+ \\S+ \\[(\\S+) \\S+\\] \"(\\w+) ([^\"]*)\" ([\\d\\.]+) (\\d+) (\\d+) (\\d+|-) \"([^\"]*)\" \"([^\"]*)\".*", 
            "key": ["ip", "time", "method", "url", "request_time", "request_length", "status", "length", "ref_url", "browser"], 
            "filterKey": [], 
            "filterRegex": [],
            "topicFormat": "none", 
            "fileEncoding": "utf8"
        }, 
        "outputType": "LogService", 
        "outputDetail": 
        {
            "logstoreName": "sls-test-logstore"
        }
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header
    {
    date: Mon, 09 Nov 2015 07:45:30 GMT
    connection: close
    x-log-requestid: 56404F1A99248CA26C002180
    content-length: 0
    server: nginx/1.6.1
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|The Project does not exist : projectName.|The error message returned because the specified project does not exist.|
|400|ConfigAlreadyExist|config configname already exists.|The error message returned because the specified Logtail configuration file already exists.|
|400|InvalidParameter|Invalid config resource json.|The error message returned because a parameter value is invalid.|
|500|InternalServerError|Internal server error.|The error message returned because an internal server error has occurred.|

For more information about the error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

