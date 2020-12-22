# DeleteExternalStore

调用DeleteExternalStore接口删除指定外部存储数据。

## 请求语法

```
DELETE /externalstores/externalStoreName
Content-Length: 0
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: ProjectName.Endpoint 
Date: GMT Date 
Authorization: LOG yourAccessKeyId:yourSignature
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    DeleteExternalStore接口无特有请求头，关于Log Service API的公共请求头请参见[公共请求头](/intl.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-test-project|Project名称。|
    |externalStoreName|String|是|rds\_store|外部存储名称。|


## 返回数据

-   响应头

    DeleteExternalStore接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/intl.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功。该接口调用成功后无任何响应元素。


## 示例

-   请求示例

    ```
    DELETE /externalstores/rds_store
    Header :
    {
    Content-Length: 0
    x-log-bodyrawsize: 0
    x-log-apiversion: 0.6.0
    x-log-signaturemethod: hmac-sha1
    Host: ali-test-project.cn-chengdu.log.aliyuncs.com 
    Date: Thu, 19 Apr 2018 03:32:49 GMT
    Authorization: LOG yourAccessKeyId:yourSignature
    }
    ```

-   正常返回示例

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


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|The Project does not exist : projectName|Project不存在。|
|400|ParameterInvalid|External store does not exist|外部存储不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/intl.zh-CN/开发指南/API 参考/通用错误码.md)。

