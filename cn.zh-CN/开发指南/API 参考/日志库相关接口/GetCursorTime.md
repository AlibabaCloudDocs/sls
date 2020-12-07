# GetCursorTime

调用GetCursorTime接口可以根据Cursor获取服务端时间。

## 请求语法

```
GET /logstores/logstoreName/shards/shardID?cursor=cursor&type=cursor_time HTTP/1.1 
Content-Type: application/json
Content-Length: 0
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: ProjectName.Endpoint
Date: GMT Date
Authorization: LOG yourAccessKeyId:yourSignature
x-log-date: Tue, 01 Dec 2020 05:51:27 GMT
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    GetCursorTime接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|参数类型|是否必填|示例值|参数说明|
    |----|----|----|---|----|
    |projectName|String|是|ali-test-project|Project名称。|
    |logstoreName|String|是|internal-operation\_log|Logstore名称。|
    |shardID|Integer|是|0|Shard ID。|
    |cursor|String|是|MTU0NzQ3MDY4MjM3NjUxMzQ0Ng%3D%3D|希望获取时间戳的Cursor。|
    |type|String|是|cursor\_time|默认为cursor\_time。|


## 返回数据

-   响应头

    GetCursorTime接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功，其响应Body会获取服务端时间信息，具体如下：

    |参数名称|数据类型|示例值|描述|
    |----|----|---|--|
    |cursor\_time|String|1554260243|Cursor的服务端时间。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|


## 示例

-   请求示例

    ```
    GET /logstores/internal-operation_log/shards/0?cursor=MTU0NzQ3MDY4MjM3NjUxMzQ0Ng%3D%3D&type=cursor_time HTTP/1.1 
    Header :
    { 
    'Content-Type': 'application/json',
    'Content-Length': '0',
    'x-log-bodyrawsize': '0',
    'x-log-apiversion': '0.6.0',
    'x-log-signaturemethod': 'hmac-sha1',
    'Host': 'ali-test-project.cn-hangzhou.log.aliyuncs.com',
    'Date': 'Tue, 01 Dec 2020 06:41:14 GMT',
    'Authorization': 'LOG yourAccessKeyId:yourSignature',
    'x-log-date': 'Tue, 01 Dec 2020 06:41:14 GMT'
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header : 
    {
    'Server': 'Tengine',
    'Content-Type': 'application/json',
    'Content-Length': '26',
    'Connection': 'close',
    'Access-Control-Allow-Origin': '*',
    'Date': 'Tue, 01 Dec 2020 06:42:01 GMT',
    'x-log-requestid': '5FC5E5B9C920EAE5D5B9D1D4'
    }
    Body :
    { 
      "cursor_time": 1554260243
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|LogStoreNotExist|Logstore logstoreName does not exist.|Logstore不存在。|
|400|ShardNotExist|Shard shardID does not exist.|Shard不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|
|400|LogStoreWithoutShard|the logstore has no shard.|Logstore没有Shard。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

