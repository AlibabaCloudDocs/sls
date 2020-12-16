# CreateConsumerGroup

调用CreateConsumerGroup接口在指定的Logstore上创建一个消费组。

## 接口说明

每个Logstore最多创建10个消费组。

## 请求语法

```
POST /logstores/logstoreName/consumergroups HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
x-log-bodyrawsize: 0
User-Agent: UserAgent
x-log-apiversion: 0.6.0
Host: ProjectName.Endpoint
x-log-signaturemethod: hmac-sha1
Date: GMT Date
Content-Type: application/json
Content-MD5: F58544E4D022CC28A93D0B7CC208A5AA
Content-Length: 54
{
  "consumerGroup": consumerGroup,
  "timeout": timeout,
  "order": order
}
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    CreateConsumerGroup接口无特有请求头，关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|my-project|Project名称。|
    |logstoreName|String|是|my-logstore|Logstore名称。|
    |consumerGroup|String|是|consumer-group-1|消费组名称，在Project下必须唯一。|
    |timeout|Integer|是|300|超时时间。在超时时间段内没有收到心跳，消费者将被删除。单位：秒。|
    |order|Boolean|否|true|在单个Shard中是否按顺序消费。     -   true：表示在单个Shard中按顺序消费。Shard分裂后，先消费原Shard数据，然后并列消费两个新Shard的数据。
    -   false：表示不按顺序消费。 |


## 返回数据

-   响应头

    CreateConsumerGroup接口无特有响应头，关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功。该接口调用成功后无任何响应元素。


## 示例

-   请求示例

    ```
    POST /logstores/my-logstore/consumergroups HTTP/1.1
    Header :
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: my-project.cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Fri, 04 May 2018 08:02:22 GMT
    Content-Type: application/json
    Content-MD5: F58544E4D022CC28A93D0B7CC208A5AA
    Content-Length: 65
    Connection: Keep-Alive
    }
    Body :
    {
      "consumerGroup": "consumer-group-1",
      "timeout": 300,
      "order": true
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header ：
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
|400|ConsumerGroupAlreadyExist|consumer group already exist.|消费组已存在。|
|400|JsonInfoInvalid|consumerGroup or timeout is of error format.|参数consumerGroup或timeout格式错误。|
|404|ProjectNotExist|The Project does not exist : ProjectName.|Project不存在。|
|404|LogStoreNotExist|logstore logstoreName dose not exist.|Logstore不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

