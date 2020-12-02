# DeleteProject

调用DeleteProject接口删除一个指定的Project。

## 请求语法

```
DELETE / HTTP/1.1
Content-Length: 0
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: ProjectName.Endpoint
Date: GMT Date
Authorization: LOG yourAccessKeyId:yourSignature
x-log-date: GMT Date
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project名称。

## 请求参数

-   请求头

    DeleteProject接口无特有请求头，关于Log Service API的公共请求头，请参见[公共请求头](/intl.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|my-project-test|Project名称，作为Host的一部分。|


## 返回数据

-   响应头

    DeleteProject接口无特有响应头，关于Log Service API的公共响应头，请参见[公共响应头](/intl.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    HTTP状态码返回200，表示请求成功。


## 示例

-   请求示例

    ```
    DELETE / HTTP/1.1
    Header :
    {
        'Content-Length': '0',
        'x-log-bodyrawsize': '0',
        'x-log-apiversion': '0.6.0',
        'x-log-signaturemethod': 'hmac-sha1',
        'Host': 'my-project-test.cn-shanghai.log.aliyuncs.com',
        'Date': 'Sun, 27 May 2018 08:25:04 GMT',
        'Authorization': LOG yourAccessKeyId:/yourSignature,
        'x-log-date': 'Sun, 27 May 2018 08:25:04 GMT'
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header :
    {
        'Server': 'nginx',
        'Content-Type': 'application/json',
        'Content-Length': '0'
        'Connection': 'close',
        'Access-Control-Allow-Origin': '*',
        'Date': 'Sun, 27 May 2018 08:25:04 GMT',
        'x-log-requestid': '5B0A6B60BB6EE39764D458B5'
    }                    
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|The Project does not exist : projectName.|项目不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/intl.zh-CN/开发指南/API 参考/通用错误码.md)。

