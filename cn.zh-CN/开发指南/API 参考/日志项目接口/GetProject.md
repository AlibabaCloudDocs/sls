# GetProject

调用GetProject接口查询Project信息。

## 请求语法

```
GET / HTTP/1.1
Authorization: <AuthorizationString>
x-log-bodyrawsize: 0
User-Agent: <UserAgent>
x-log-apiversion: 0.6.0
Host: <Project Endpoint>
x-log-signaturemethod: hmac-sha1
Date: <GMT Date>
Content-Type: application/x-protobuf
Connection: Keep-Alive
```

## 请求参数

-   请求头

    GetProject接口无特有请求头，关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|my-project-test|Project名称。|


## 返回参数

-   响应头

    GetProject接口无特有响应头，关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    HTTP状态码返回200，表示GetProject请求成功。其响应Body中包含Project的详细信息，如下所示：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |createTime|String|2020-11-18 16:55:57|创建Project的时间。|
    |description|String|test|Project描述。|
    |lastModifyTime|String|2020-11-18 17:07:26|最后一次更新Project的时间。|
    |owner|String|174\*\*\*\*745|创建人的阿里云账户ID。|
    |projectName|String|my-project-test|Project名称。|
    |status|String|Normal|Project状态。|
    |region|String|cn-hangzhou|Project所属地域。|


## 示例

-   请求示例

    ```
    GET / HTTP/1.1
    Authorization: LOG <yourAccessKeyId>:<yourSignature>
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: my-project-test.cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Sun, 27 May 2018 08:25:04 GMT
    Content-Type: application/x-protobuf
    Connection: Keep-Alive
    ```

-   正常返回示例

    ```
    HTTP/1.1 200
    Server: nginx
    Content-Length: 0
    Connection: close
    Access-Control-Allow-Origin: *
    Date: Sun, 27 May 2018 08:25:04 GMT
    x-log-requestid: 5B0A6B60BB6EE39764D458B5
    {
        "createTime":"2020-11-18 16:55:57",
        "description":"test",
        "lastModifyTime":"2020-11-18 17:07:26",
        "owner":"174****745",
        "projectName":"my-project-test",
        "region":"cn-hangzhou",
        "status":"Normal"
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|The Project does not exist : \{Project\}|Project不存在。|
|500|InternalServerError|Specified Server Error Message|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

