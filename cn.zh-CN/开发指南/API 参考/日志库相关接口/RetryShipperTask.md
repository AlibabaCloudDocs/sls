# RetryShipperTask

调用RetryShipperTask接口重新执行失败日志投递任务。

## 接口说明

每次最多只能重新执行10个失败的投递任务。

## 请求语法

```
PUT /logstores/logstoreName/shipper/shipperName/tasks HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
["task-id-1", "task-id-2", "task-id-2"]
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    RetryShipperTask接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-test-project|Project名称。|
    |logstoreName|String|是|test-logstore|Logstore名称。|
    |shipperName|String|是|test-shipper|日志投递名称。|


## 返回数据

-   响应头

    RetryShipperTask接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功。该接口调用成功后无任何响应元素。


## 示例

-   请求示例

    ```
    PUT /logstores/test-logstore/shipper/test-shipper/tasks HTTP/1.1
    Header:
    {
        "x-log-apiversion" : 0.6.0, 
        "Authorization" : "LOG yourAccessKeyId:yourSignature", 
        "Host" : "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com", 
        "Date" : "Wed, 11 Nov 2015 08:28:19 GMT", 
        "Content-Length" : 55, 
        "x-log-signaturemethod" : "hmac-sha1", 
        "Content-MD5" : "757C60FC41CC7D3F60B88E0D916D051E", 
        "User-Agent" : "sls-java-sdk-v-0.6.0", 
        "Content-Type" : "application/json"
    }
    Body : 
    ["task-id-1", "task-id-2", "task-id-2"]
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header:
    {
        "Date" : "Wed, 11 Nov 2015 08:28:20 GMT", 
        "Content-Length" : 0, 
        "x-log-requestid" : "5642FC2399248C8F7B0145FD", 
        "Connection" : "close", 
        "Server" : "nginx/1.6.1"
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|Project projectName does not exist.|Project不存在。|
|404|LogStoreNotExist|Logstore logstoreName does not exist.|Logstore不存在。|
|400|ShipperNotExist|Shipper does not exist.|投递任务不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|
|400|ParameterInvalid|Each time allows 10 task retries only.|每次最多只能重新执行10个任务。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

