# DeleteConsumerGroup

调用DeleteConsumerGroup接口删除一个指定的消费组。

## 请求语法

```
DELETE /logstores/logstoreName/consumergroups/consumerGroupName HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
Host: ProjectName.Endpoint
x-log-signaturemethod: hmac-sha1
Date: GMT Date
Content-Type:  application/x-protobuf
Content-MD5: F58544E4D022CC28A93D0B7CC208A5AA
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    DeleteConsumerGroup接口无特有请求头，关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-test-project|Project名称。|
    |logstoreName|String|是|logstore-test|Logstore名称。|
    |consumerGroup|String|是|consumer-group-1|消费组名称。|


## 返回数据

-   响应头

    DeleteConsumerGroup接口无特有响应头，关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功。该接口调用成功后无任何响应元素。


## 示例

-   请求示例

    ```
    DELETE /logstores/logstore-test/consumergroups/consumer-group-1 HTTP/1.1
    Header:
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: ali-test-project.cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Fri, 04 May 2018 08:02:22 GMT
    Content-Type: application/json
    Content-MD5: F58544E4D022CC28A93D0B7CC208A5AA
    Content-Length: 65
    Connection: Keep-Alive
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header :
    {
    Server: nginx/1.12.1
    Content-Length: 0
    Connection: close
    Access-Control-Allow-Origin: *
    Date: Fri, 04 May 2018 08:15:11 GMT
    x-log-requestid: 5AEC168FA796F4195BF404CB
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|The Project does not exist : projectName.|Project不存在。|
|404|LogStoreNotExist|The logstore logstoreName dose not exist.|Logstore不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

