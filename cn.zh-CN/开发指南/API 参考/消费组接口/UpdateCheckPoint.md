# UpdateCheckPoint

调用UpdateCheckPoint接口更新指定消费组消费数据时Shard的checkpoint。

## 接口说明

当不指定消费者时，必须指定forceSuccess为true才能更新checkpoint。

## 请求语法

```
POST /logstores/logstoreName/consumergroups/consumerGroup HTTP/1.1
Authorization: AuthorizationString
x-log-bodyrawsize: 0
User-Agent: UserAgent
x-log-apiversion: 0.6.0
Host: ProjectName.Endpoint
x-log-signaturemethod: hmac-sha1
Date: GMT Date
Content-Type: application/json
Content-Length: 54
Connection: Keep-Alive
{
  "shard": shardID,
  "checkpoint": checkpoint
}
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    UpdateCheckPoint接口无特有请求头，关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|my-project|Project名称。|
    |logstoreName|String|是|logstore-4|Logstore名称。|
    |consumerGroup|String|是|consumer\_group\_test|消费组名称。|
    |shard|Integer|是|0|Shard ID。|
    |checkpoint|String|是|MTUyNDE1NTM3OTM3MzkwODQ5Ng==|checkpoint值。|
    |consumer|String|否|consumer\_1|消费者。|
    |forceSuccess|Boolean|否|False|是否强制更新。    -   True：强制更新
    -   False：不强制更新 |


## 返回数据

-   响应头

    UpdateCheckPoint接口无特有响应头，关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功。该接口调用成功后无任何响应元素。


## 示例

-   请求示例

    ```
    POST /logstores/logstore-4/consumergroups/consumer_group_test?forceSuccess=false&type=checkpoint&consumer=consumer_1 HTTP/1.1
    Header ：
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: my-project.cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Sun, 06 May 2018 09:14:53 GMT
    Content-Type: application/json
    Content-MD5: 59457BAFB3F85A5117BDE243947583B0
    Content-Length: 55
    Connection: Keep-Alive
    }
    Body ：
    {
      "shard": 0,
      "checkpoint": "MTUyNDE1NTM3OTM3MzkwODQ5Ng=="
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
    Date: Sun, 06 May 2018 09:14:54 GMT
    x-log-requestid: 5AEEC78E1FFC0366B24F1C63
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|400|InvalidShardCheckPoint|shard checkpoint not encoded by base64.|checkpoint不是Base64编码，格式错误。|
|404|ProjectNotExist|The Project does not exist : ProjectName.|Project不存在。|
|404|LogStoreNotExist|logstore \{logstoreName\} dose not exist.|Logstore不存在。|
|404|ConsumerGroupNotExist|consumer group not exist.|消费组不存在。|
|404|ConsumerNotExist|consumer not exist in the consumer group|消费组中不存在该消费者。|
|404|ShardNotExist|shard not exist.|Shard不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

