# ListLogstore

调用ListLogstores接口查询指定Project下所有的Logstore。

## 请求语法

```
GET /logstores HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## 请求参数

-   请求头

    ListLogstore接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |ProjectName|String|是|test-project|Project名称，支持部分匹配。|
    |offset|Integer|否|1|查询开始行。默认值为0。|
    |size|Integer|否|10|分页查询时，设置的每页行数。最大值为500。|


## 返回数据

-   响应头

    ListLogstore接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，表示请求成功。响应Body中包括Logstore信息，如下所示：

    |名称|类型|示例值|描述|
    |:-|:-|---|:-|
    |count|Integer|1|当前页返回的Logstore数目。|
    |total|Integer|1|符合查询条件的Logstore总数。|
    |logstores|Array|\["test-logstore"\]|返回的Logstore名称列表。|


## 示例

-   请求示例

    ```
    GET /logstores HTTP/1.1
    Header: 
    {
    x-log-apiversion=0.6.0, 
    Authorization=LOG <yourAccessKeyId>:<yourSignature>, 
    Host=ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com, 
    Date=Wed, 11 Nov 2015 08:09:39 GMT, 
    Content-Length=0, 
    x-log-signaturemethod=hmac-sha1, 
    User-Agent=sls-java-sdk-v-0.6.0, 
    Content-Type=application/json
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header: 
    {
    Date=Wed, 11 Nov 2015 08:09:39 GMT, 
    Content-Length=52, 
    x-log-requestid=5642F7C399248C8D7B01342F, 
    Connection=close, 
    Content-Type=application/json, 
    Server=nginx/1.6.1
    }
    Body:
    {
    "count":1,
    "logstores":["test-logstore"],
    "total":1
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|Project \{ProjectName\} does not exist.|日志服务Project不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|
|400|ParameterInvalid|Invalid parameter size, \(0.6.0\].|无效参数。|
|400|InvalidLogStoreQuery|logstore Query is invalid.|无效查询。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

