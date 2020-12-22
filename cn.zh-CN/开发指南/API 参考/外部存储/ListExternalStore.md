# ListExternalStore

调用ListExternalStore接口查询外部存储数据列表。支持查询指定Project下所有外部存储列表，也支持查询指定Project下具体外部存储信息。

## 请求语法

```
GET /externalstores?externalStoreName=externalStoreName&offset=offset&sizes=sizes
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

    ListExternalStore接口无特有请求头，关于Log Service API的公共请求头请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-test-project|Project名称。|
    |externalStoreName|String|否|store|外部存储名称。支持查询出包含该字符串的外部存储。|
    |offset|Integer|否|0|查询开始行。默认值为0。|
    |sizes|Integer|否|10|分页查询时，设置的每页行数。最大值为500。|


## 返回数据

-   响应头

    ListExternalStore接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，表示请求成功。响应Body中包括外部存储名称，具体格式如下：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |count|Integer|3|当前页返回的外部存储数量。|
    |total|Integer|3|符合查询条件的外部存储总数。|
    |externalstores|Array|\["ecs\_store", "rds\_store", "ecs\_store1"\]|返回的外部存储名称列表。|


## 示例

-   请求示例

    ```
    GET http://ali-test-project.cn-chengdu.log.aliyuncs.com:80/externalstores?externalStoreName=store&offset=0&sizes=10
    Header ：
    {
    Content-Length: 0 
    x-log-bodyrawsize: 0 
    x-log-apiversion: 0.6.0 
    x-log-signaturemethod':hmac-sha1
    Host: ali-test-project.cn-chengdu.log.aliyuncs.com
    Date: Thu, 19 Apr 2018 03:03:16 GMT 
    Authorization: LOG yourAccessKeyId:yourSignature
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header :
    {
    content-length: 103 
    server: nginx/1.6.1
    connection: close
    date: Mon, 09 Nov 2015 09:19:13 GMT
    content-type: application/json
    x-log-requestid: 5640651199248CAA2300C2BA
    }
    Body:
    {
        "count": 3, 
        "total": 3,
        "externalstores": 
        [
            "ecs_store", 
            "rds_store", 
            "ecs_store1"
        ]
    
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|The Project does not exist : projectName|Project不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

