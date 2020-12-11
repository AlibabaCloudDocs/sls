# ListConfig

调用ListConfig接口查询指定Project下所有的Logtail配置。

## 请求语法

```
GET /configs?offset=0&size=100 HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    ListConfig接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-test-project|Project名称。|
    |offset|Integer|否|0|查询开始行。默认值为0。|
    |size|Integer|否|10|分页查询时，设置的每页行数。最大值为500。|
    |logstoreName|String|否|logstore-4|Logstore名称。|
    |configName|String|否|logtail-config-sample|Logtail配置名称。|


## 返回数据

-   响应头

    ListConfig接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，表示请求成功。ListConfig接口的响应Body中包括Logtail采集配置信息，如下所示：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |count|Integer|3|当前页返回的Logtail配置数量。|
    |total|Integer|3|符合查询条件的Logtail配置总数。|
    |configs|Array|\[ "logtail-config-sample", "logtail-config-sample-2", "logtail-config-sample-3" \]|当前页返回的Logtail配置列表。|


## 示例

-   请求示例

    ```
    GET /configs?offset=0&size=10 HTTP/1.1
    Header :
    {
        "Content-Length": 0, 
        "x-log-signaturemethod": "hmac-sha1", 
        "x-log-bodyrawsize": 0, 
        "User-Agent": "log-python-sdk-v-0.6.0", 
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com", 
        "Date": "Mon, 09 Nov 2015 09:19:13 GMT", 
        "x-log-apiversion": "0.6.0", 
        "Authorization": "LOG <yourAccessKeyId>:<yourSignature>"
    }
    ```

-   正常返回示例

    ```
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


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|Project ProjectName does not exist.|Project不存在。|
|404|LogstoreNotExist|logstore logstoreName does not exist.|Logstore不存在。|
|404|ConfigNotExist|config configName does not exist.|Logtail配置不存在。|
|500|InternalServerError|internal server error.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

