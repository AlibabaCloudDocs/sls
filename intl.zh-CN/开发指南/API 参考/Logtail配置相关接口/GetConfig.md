# GetConfig

调用GetConfig接口获取Logtail配置的详细信息。

## 请求语法

```
GET /configs/configName HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    GetConfig接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/intl.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-test-project|Project名称。|
    |configName|String|是|logtail-config-sample|Logtail配置名称。|


## 返回数据

-   响应头

    GetConfig接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/intl.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    响应内容中包含的参数信息和各种模式的Logtail配置样例，请参见[Logtail配置](/intl.zh-CN/开发指南/API 参考/公共资源说明/Logtail配置.md)。


## 示例

-   请求示例

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

-   正常返回示例

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
            "regex": "([\\d\\.]+) \\S+ \\S+ \\[(\\S+) \\S+\\] \"(\\w+) ([^\"]*)\" ([\\d\\.]+) (\\d+) (\\d+) (\\d+|-) \"([^\"]*)\" \"([^\"]*)\".*",
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


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|Project ProjectName does not exist.|Project不存在。|
|404|ConfigNotExist|Config configName does not exist.|Logtail配置不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/intl.zh-CN/开发指南/API 参考/通用错误码.md)。

