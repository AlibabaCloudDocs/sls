# ListShards

调用ListShards接口列出指定Logstore中所有可用的Shard。

## 请求语法

```
GET /logstores/logstorename/shards HTTP/1.1
Content-Length: 0
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod': hmac-sha1
Host: ProjectName.Endpoint
Date: GMT Date
Authorization: LOG yourAccessKeyId:yourSignature
x-log-date: Mon, 30 Nov 2020 06:22:17 GMT   
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    ListShards接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/intl.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-test-project|Project名称。|
    |logstoreName|String|是|sls-test-logstore|Logstore名称。|


## 返回数据

-   响应头

    ListShards接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/intl.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，表示请求成功。请求成功后，其响应Body中包含Shard信息，如下所示：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |shardID|Integer|0|Shard ID。|
    |status|String|readwrite|Shard状态。|
    |inclusiveBeginKey|String|00000000000000000000000000000000|Shard起始的Key值，在Shard MD5范围中包含该值。|
    |exclusiveEndKey|String|8000000000000000000000000000000|Shard结束的Key值，在Shard MD5范围中不包含该值。|
    |createTime|String|1453949705|Shard的创建时间。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|


## 示例

-   请求示例

    ```
    GET /logstores/sls-test-logstore/shards
    Header :
    {
    'Content-Length': '0'
    'x-log-bodyrawsize': '0'
    'x-log-apiversion': '0.6.0'
    'x-log-signaturemethod': 'hmac-sha1'
    'Host': ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com
    'Date': Mon, 30 Nov 2020 06:22:17 GMT
    'Authorization': 'LOG yourAccessKeyId:yourSignature'
    'x-log-date': 'Mon, 30 Nov 2020 06:22:17 GMT'
    }
    ```

-   返回示例

    ```
    Header:
    {
    'Server': 'Tengine',
    'Content-Type': 'application/json',
    'Content-Length': '335',
    'Connection': 'close',
    'Access-Control-Allow-Origin': '*',
    'Date': 'Mon, 30 Nov 2020 08:32:24 GMT',
    'x-log-requestid': '5FC4AE1821CA7D41C5F14728'   
    }
    Body :
    [
        {
            "shardID": 1,
            "status": "readwrite",
            "inclusiveBeginKey": "00000000000000000000000000000000",
            "exclusiveEndKey": "8000000000000000000000000000000",
            "createTime": 1453949705
        },
        {
            "shardID": 2,
            "status": "readwrite",
            "inclusiveBeginKey": "80000000000000000000000000000000",
            "exclusiveEndKey": "ffffffffffffffffffffffffffffffff",
            "createTime": 1453949705
        },
        {
            "shardID": 0,
            "status": "readonly",
            "inclusiveBeginKey": "00000000000000000000000000000000",
            "exclusiveEndKey": "ffffffffffffffffffffffffffffffff",
            "createTime": 1453949705
        }
    ]
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|The Project does not exist : projectName|Project不存在。|
|404|LogStoreNotExist|logstore logstoreName does not exist.|Logstore不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|
|400|LogStoreWithoutShard|logstore has no shard.|Logstore中没有Shard。|

更多错误码，请参见[通用错误码](/intl.zh-CN/开发指南/API 参考/通用错误码.md)。

