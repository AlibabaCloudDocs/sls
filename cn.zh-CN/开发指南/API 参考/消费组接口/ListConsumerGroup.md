# ListConsumerGroup

调用ListConsumerGroup接口查询指定Logstore的所有消费组。

## 请求语法

```
GET /logstores/logstoreName/consumergroups HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
Host: ProjectName.Endpoint
x-log-signaturemethod: hmac-sha1
Date: GMT Date
Content-Type: application/x-protobuf
Connection: Keep-Alive
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    ListConsumerGroup接口无特有请求头，关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|my-project|Project名称。|
    |logstoreName|String|是|my-logstore|Logstore名称。|


## 返回数据

-   响应头

    ListConsumerGroup接口无特有响应头，关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功。其响应Body中包含目标Project下的消费组信息，具体如下：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |count|Integer|1|消费组数量。|
    |consumerGroups|Array|\[\{''name'': ''test-consumer-group'', ''timeout'': 30, ''order'': False\}\]|消费组列表。更多信息，请参见下表consumerGroups参数说明。|

    consumerGroups参数说明如下表所示：

    |参数名称|数据类型|示例值|描述|
    |----|----|---|--|
    |name|String|test-consumer-group|消费组名称。|
    |timeout|Integer|30|超时时间。|
    |order|Boolean|true|是否按顺序消费。|


## 示例

-   请求示例

    ```
    GET /logstores/my-logstore/consumergrops HTTP/1.1
    Header :
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: my-project.cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Fri, 04 May 2018 08:47:30 GMT
    Content-Type: application/x-protobuf
    Connection: Keep-Alive
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header :
    {
    Server: nginx/1.12.1
    Content-Type: application/json
    Connection: close
    Access-Control-Allow-Origin: *
    Date: Fri, 04 May 2018 08:47:31 GMT
    x-log-requestid: 5AEC1E23048191954B42EAB9
    }
    Body :
    {
        "count": "1",
        "consumerGroups": 
    [
      {
        "name": "test-consumer-group",
        "timeout": 30,
        "order": False
      }
    ]
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|The Project does not exist : projectName.|Project不存在。|
|404|LogStoreNotExist|logstore logstoreName dose not exist.|Logstore不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

