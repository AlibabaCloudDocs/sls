# HeartBeat

调用HeartBeat接口为指定消费者发送心跳到服务端。

## 接口说明

只有消费者和服务端建立连接之后才能发送心跳。

## 请求语法

```
POST /logstores/logstoreName/consumergroups/consumerGroup?type=heartbeat&consumer=<consumer> HTTP/1.1
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
Body :
{ "shards" :shard ID list} 
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    HeartBeat接口无特有请求头，关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|my-project|Project名称。|
    |logstoreName|String|是|my-logstore|日志库名称，同一Project中日志库名称为唯一值。|
    |consumerGroup|String|是|consumer\_group\_test|消费组名称，同一Project中消费组名称为唯一值。|
    |consumer|String|是|consumer\_1|消费者。|
    |shards|Array|否|\[0\]|正在消费的Shard ID列表。|


## 返回数据

-   响应头

    HeartBeat接口无特有响应头，关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示HeartBeat请求成功，同时返回消费者消费的所有Shard ID列表。


## 示例

-   请求示例

    ```
    POST /logstores/my-logstore/consumergroups/consumer_group_test?type=heartbeat&consumer=consumer_1 HTTP/1.1
    Header :
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: my-project.cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Sun, 06 May 2018 09:44:10 GMT
    Content-Type: application/json
    Content-MD5: 8D5162CA104FA7E79FE80FD92BB657FB
    Content-Length: 3
    Connection: Keep-Alive
    }
    Body :
    { "shards" : [0]}
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header :
    {
    Server: nginx/1.12.1
    Content-Type: application/json
    Content-Length: 5
    Connection: close
    Access-Control-Allow-Origin: *
    Date: Sun, 06 May 2018 09:44:11 GMT
    x-log-requestid: 5AEECE6B1FFC0366B2553FB5
    }
    Body :
    { "shards" : [0,1]}
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|400|NotExistConsumerWithBody|non-exist consumer with non-empty body of heartbeat message.|不存在非空心跳消息的消费者。|
|404|ProjectNotExist|The Project does not exist : ProjectName.|Project不存在。|
|404|LogStoreNotExist|logstore logstoreName dose not exist.|Logstore不存在。|
|404|ConsumerGroupNotExist|consumer group not exist.|消费组不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

