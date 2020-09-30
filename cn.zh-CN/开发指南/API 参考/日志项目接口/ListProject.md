# ListProject

调用ListProject查询所有Project列表。

## 请求语法

```
GET /?offset={offset}&size={size} HTTP/1.1
Authorization: <AuthorizationString>
x-log-bodyrawsize: 0
User-Agent: <UserAgent>
x-log-apiversion: 0.6.0
Host: <Endpoint>
x-log-signaturemethod: hmac-sha1
Date: <GMT Date>
Content-Type: application/x-protobuf
Connection: Keep-Alive
```

## 请求参数

-   请求头

    ListProject接口无特有请求头，关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |属性名称|类型|是否必须|示例值|描述|
    |:---|:-|:---|---|:-|
    |offset|integer|否|0|返回记录的起始位置，默认值为0。|
    |size|integer|否|2|每页返回最大条目，默认500（最大值）。|


## 返回数据

-   响应头

    ListProject接口无特有响应头，关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    HTTP状态码返回200，ListProject请求成功。其响应Body中包含Project列表，具体格式如下：

    |属性名称|类型|描述|
    |:---|:-|:-|
    |count|integer|返回的 Project 个数。|
    |total|integer|Project 总数。|
    |projects|array|Project 列表。|

    projects中每个元素的格式为：

    |属性名称|类型|描述|
    |:---|:-|:-|
    |createTime|string|创建时间。|
    |description|string|Project描述|
    |lastModifyTime|string|最后一次更新时间。|
    |owner|string|创建人的账户Id。|
    |projectName|string|Project名称。|
    |status|string|Project状态。|
    |region|string|Project所属的区域。|


## 示例

-   请求示例

    ```
    GET /?offset=0&size=2&projectName= HTTP/1.1
    Authorization: LOG <yourAccessKeyId>:<yourSignature>
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Sun, 27 May 2018 09:03:33 GMT
    Content-Type: application/x-protobuf
    Connection: Keep-Alive
    ```

-   正常返回示例

    ```
    HTTP/1.1 200
    Server: nginx
    Content-Type: application/json
    Content-Length: 345
    Connection: close
    Access-Control-Allow-Origin: *
    Date: Sun, 27 May 2018 09:03:33 GMT
    x-log-requestid: 5B0A7465AAEA20CA70DE3064
    {
      "count": 2,
      "total": 11,
      "projects": [
        {
          "projectName": "project1",
          "status": "Normal",
          "owner": "",
          "description": "",
          "region": "cn-shanghai",
          "createTime": "1524222931",
          "lastModifyTime": "1524539357"
        },
        {
          "projectName": "project123456",
          "status": "Normal",
          "owner": "",
          "description": "",
          "region": "cn-shanghai",
          "createTime": "1471963876",
          "lastModifyTime": "1524539357"
        }
      ]
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

