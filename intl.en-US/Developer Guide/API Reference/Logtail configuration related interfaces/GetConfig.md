# GetConfig

Retrieves the details of a Logtail configuration file.

## Request syntax

```
GET /configs/configName HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The GetConfig operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |configName|String|Yes|logtail-config-sample|The name of the Logtail configuration file.|


## Response parameters

-   Response headers

    The GetConfig operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    For information about the response parameters and examples of Logtail configuration files in various mode, see [Logtail configuration files](/intl.en-US/Developer Guide/API Reference/Common resources/Logtail configuration files.md).


## Examples

-   Sample requests

    ```
    GET /configs/logtail-config-sample 
    Header :
    {
        "Content-Length": 0,
        "x-log-signaturemethod": "hmac-sha1",
        "x-log-bodyrawsize": 0,
        "User-Agent": "log-python-sdk-v-0.6.0",
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
        "Date": "Mon, 09 Nov 2015 08:29:15 GMT",
        "x-log-apiversion": "0.6.0",
        "Authorization": "LOG yourAccessKeyId:yourSignature"
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header :
    {   
        "content-length": "730",
        "server": "nginx/1.6.1",
        "connection": "close",
        "date": "Mon, 09 Nov 2015 08:29:15 GMT",
        "content-type": "application/json",
        "x-log-requestid": "5640595B99248CAA23004A59"
    }
    Body :
    {   
        "configName": "logtail-config-sample",
        "outputDetail": {
            "endpoint": "http://cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
            "logstoreName": "sls-test-logstore"
        },
        "outputType": "LogService",
        "inputType": "file",
        "inputDetail": {
            "regex": "([\\d\\.] +) \\S+ \\S+ \\[(\\S+) \\S+\\] \"(\\w+) ([^\"]*)\" ([\\d\\.]+) (\\d+) (\\d+) (\\d+|-) \"([^\"]*)\" \"([^\"]*)\".*",
            "filterKey": [],
            "logPath": "/var/log/httpd/",
            "logBeginRegex": "\\d+\\.\\d+\\.\\d+\\.\\d+ - .*",
            "logType": "common_reg_log",
            "topicFormat": "none",
            "localStorage": true,
            "key": [
                "ip",
                "time",
                "method",
                "url",
                "request_time",
                "request_length",
                "status",
                "length",
                "ref_url",
                "browser"
            ],
            "filePattern": "access*.log",
            "timeFormat": "%d/%b/%Y:%H:%M:%S",
            "filterRegex": []
        },
        "createTime": 1447040456,
        "lastModifyTime": 1447050456
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|Project ProjectName does not exist.|The error message returned because the specified project does not exist.|
|404|ConfigNotExist|Config configName does not exist.|The error message returned because the specified Logtail configuration file does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For more information about the error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

