# CreateConfig

调用CreateConfig接口创建Logtail配置。

## 接口说明

每个Project最多可创建100个Logtail配置。

## 请求语法

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

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    CreateConfig接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   请求参数

    关于请求参数说明和各种模式的Logtail配置样例，请参见[Logtail配置](/cn.zh-CN/开发指南/API 参考/公共资源说明/Logtail配置.md)。


## 返回数据

-   响应头

    CreateConfig接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功。该接口调用成功后无任何响应元素。


## 示例

-   请求示例

    ```
    POST /configs HTTP/1.1
    Header :
    {
        'Content-Length': 737, 
        'Host': 'ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com', 
        'x-log-bodyrawsize': 737, 
        'Content-MD5': 'FBA01ECF7255BE143379BC70C56BBF68', 
        'x-log-signaturemethod': 'hmac-sha1', 
        'Date': 'Mon, 09 Nov 2015 07:45:30 GMT', 
        'x-log-apiversion': '0.6.0', 
        'User-Agent': 'log-python-sdk-v-0.6.0', 
        'Content-Type': 'application/json', 
        'Authorization': 'LOG yourAccessKeyId:yourSignature'
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
            "regex": "([\\d\\.]+) \\S+ \\S+ \\[(\\S+) \\S+\\] \"(\\w+) ([^\"]*)\" ([\\d\\.]+) (\\d+) (\\d+) (\\d+|-) \"([^\"]*)\" \"([^\"]*)\".*", 
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

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header
    {
        'date': 'Mon, 09 Nov 2015 07:45:30 GMT',
        'connection': 'close',
        'x-log-requestid': '56404F1A99248CA26C002180',
        'content-length': '0',
        'server': 'nginx/1.6.1'
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|The Project does not exist : projectName.|Project不存在。|
|400|ConfigAlreadyExist|config configname already exists.|Logtail配置已存在。|
|400|InvalidParameter|invalid config resource json.|无效的Logtail配置参数。|
|500|InternalServerError|internal server error.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

