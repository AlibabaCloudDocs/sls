# GetCheckPoint

调用GetCheckPoint接口获取指定消费组消费数据时Shard的checkpoint。

## 请求语法

```
GET /logstores/logstoreName/consumergroups/consumerGroup?shard=shardId HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
Host: ProjectName.Endpoint
x-log-signaturemethod: hmac-sha1
Date: GMT Date
Content-Type: application/json
Connection: Keep-Alive
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    GetCheckPoint接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|my-project|Project名称。|
    |logstoreName|String|是|my-logstore|Logstore名称。|
    |consumerGroup|String|是|consumer\_group\_test|消费组名称。|
    |shardId|Integer|否|0|Shard ID。    -   如果指定的Shard不存在，则返回空列表。
    -   如果不指定Shard，则返回所有Shard的checkpoint。 |


## 返回数据

-   响应头

    GetCheckPoint接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功，其响应Body会包含指定消费组消费数据时Shard的checkpoint，具体如下：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |shard|Integer|0|Shard ID。|
    |checkpoint|String|MTUyNDE1NTM3OTM3MzkwODQ5Ng==|checkpoint值。|
    |updateTime|Integer|1524224984800922|checkpoint最后的更新时间。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|
    |consumer|String|consumer\_1|消费者。|


## 示例

-   请求示例

    ```
    GET /logstores/my-logstore/consumergroups/consumer_group_test?shard=0
    Header ：
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: my-project.cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Fri, 04 May 2018 09:26:53 GMT
    Content-Type: application/x-protobuf
    Connection: Keep-Alive
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header ：
    {
    Server: nginx/1.12.1
    Content-Type: application/json
    Content-Length: 111
    Connection: close
    Access-Control-Allow-Origin: *
    Date: Fri, 04 May 2018 09:26:53 GMT
    x-log-requestid: 5AEC275D1FFC036AB254B20C
    }
    Body ：
    {
    [
      {
        "shard": 0,
        "checkpoint": "MTUyNDE1NTM3OTM3MzkwODQ5Ng==",
        "updateTime": 1524224984800922,
        "consumer": "consumer_1"
      }
    ]
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|The Project does not exist : projectName.|Project不存在。|
|404|LogStoreNotExist|logstore logstoreName dose not exist.|Logstore不存在。|
|404|ConsumerGroupNotExist|consumer group not exist.|消费组不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

