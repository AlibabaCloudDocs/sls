# ListProject

调用ListProject接口列出符合条件的Project信息。

## 请求语法

```
GET /?offset=offset&size=size HTTP/1.1
Content-Length: 0
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: Endpoint
Date: GMT Date
Authorization: LOG yourAccessKeyId:yourSignature
x-log-date: GMT Date
```

## 请求参数

-   请求头

    ListProject接口无特有请求头，关于Log Service API的公共请求头，请参见[公共请求头](/intl.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |offset|Integer|否|0|查询开始行，默认值为0。|
    |size|Integer|否|10|分页查询时，设置的每页行数。默认值为100，最大值为500。|


## 返回数据

-   响应头

    ListProject接口无特有响应头，关于Log Service API的公共响应头，请参见[公共响应头](/intl.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，表示请求成功。其响应Body中包含Project列表信息，如下所示：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |count|Integer|2|当前页返回的Project个数。|
    |total|Integer|11|符合查询条件的Project总数。|
    |projects|Array|不涉及|符合查询条件Project列表。|

    projects中的每个参数说明如下所示：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |createTime|String|1524222931|创建Project的时间。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|
    |description|String|test|Project描述。|
    |lastModifyTime|String|1524539357|最后一次更新Project时间。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|
    |owner|String|12\*\*\*\*34|创建人的阿里云账户ID。|
    |projectName|String|project1|Project名称。|
    |status|String|Normal|Project状态。    -   Normal：正常
    -   Disable：禁用 |
    |region|String|cn-shanghai|Project所属地域。|


## 示例

-   请求示例

    ```
    GET /?offset=0&size=2 HTTP/1.1
    Header :
    {
    Content-Length: 0
    x-log-bodyrawsize: 0
    x-log-apiversion: 0.6.0
    x-log-signaturemethod: hmac-sha1
    Host: cn-shanghai.log.aliyuncs.com
    Date: Sun, 27 May 2018 09:03:33 GMT
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-date: Sun, 27 May 2018 09:03:33 GMT
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header :
    {
        Server: nginx
        Content-Type: application/json
        Content-Length: 345
        Connection: close
        Access-Control-Allow-Origin: *
        Date: Sun, 27 May 2018 09:03:33 GMT
        x-log-requestid: 5B0A7465AAEA20CA70DE3064
    }
    Body :
    {
      "count": 2,
      "total": 11,
      "projects": [
        {
          "projectName": "project1",
          "status": "Normal",
          "owner": "12****34",
          "description": "test",
          "region": "cn-shanghai",
          "createTime": "1524222931",
          "lastModifyTime": "1524539357"
        },
        {
          "projectName": "project123456",
          "status": "Normal",
          "owner": "12****34",
          "description": "test",
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
|400|ParameterInvalid|offset : offset pair is invalid|offset参数取值不合法。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/intl.zh-CN/开发指南/API 参考/通用错误码.md)。

