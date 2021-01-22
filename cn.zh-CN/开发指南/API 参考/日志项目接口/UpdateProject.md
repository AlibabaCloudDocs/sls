# UpdateProject

调用UpdateProject接口更新一个Project信息。

## 请求语法

```
PUT / HTTP/1.1 
Authorization: LOG yourAccessKeyId:yourSignature
x-log-bodyrawsize: 0 
User-Agent:  UserAgent
x-log-apiversion: 0.6.0 
Host:  ProjectName.Endpoint
x-log-signaturemethod: hmac-sha1 
Date:  GMT Date
Content-Type: application/json 
Content-MD5:  Content-MD5
Content-Length:  54
Connection: Keep-Alive
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    UpdateProject接口无特有请求头，关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-project-test|Project的名称，作为Header中Host的一部分。|
    |description|String|否|Description of my-project-test|Project的描述。默认为空字符串。|


## 返回数据

-   响应头

    UpdateProject接口无特有响应头，关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，表示请求成功。该接口调用成功后无任何响应元素。


## 示例

-   请求示例

    ```
    PUT / HTTP/1.1 
    Header :
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-bodyrawsize: 0 
    x-log-apiversion: 0.6.0 
    Host: ali-project-test.cn-shanghai.log.aliyuncs.com 
    x-log-signaturemethod: hmac-sha1 
    Date: Sun, 27 May 2018 07:43:26 GMT 
    Content-Type: application/json 
    Content-MD5: A7967D81EFF5E3CD447FB6D8DF294E20 
    Content-Length: 40 
    Connection: Keep-Alive 
    }
    Body :    
    { 
      "description": "Description of my-project-test" 
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header :
    {
    Server: nginx 
    Content-Length: 0 
    Connection: close 
    Access-Control-Allow-Origin: * 
    Date: Sun, 27 May 2018 07:43:27 GMT 
    x-log-requestid: 5B0A619F205DC3F30EDA9322
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|The Project does not exist : projectName.|Project不存在。|
|400|ParameterInvalid|The body is not valid json string.|无效的参数。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

