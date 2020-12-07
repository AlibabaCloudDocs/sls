# DeleteLogstore

调用DeleteLogstore接口删除指定Logstore，包括所有Shard数据和索引。

## 请求语法

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

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    DeleteLogstore接口无特有请求头，关于Log Service API的公共请求头，请参见[公共请求头](/intl.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |logstoreName|String|是|test\_logstore|日志库名称。|


## 返回数据

-   响应头

    DeleteLogstore接口无特有响应头，关于Log Service API的公共响应头，请参见[公共响应头](/intl.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，表示请求成功。


## 示例

-   请求示例

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

-   正常返回示例

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


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|LogStoreNotExist|logstore logstoreName does not exist|Logstore不存在。|
|405|InvalidMethod|invalid request method: /logstores/logstoreName|logstoreName参数不合法。|
|500|InternalServerError|Specified Server Error Message|内部服务调用错误。|

更多错误码，请参见[通用错误码](/intl.zh-CN/开发指南/API 参考/通用错误码.md)。

