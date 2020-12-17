# SplitShard

调用SplitShard接口分裂一个指定的readwrite状态的Shard。

## 请求语法

```
POST /logstores/logstoreName/shards/shardID HTTP/1.1
Content-Length: 0
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: ProjectName.Endpoint
Date': GMT Date
Authorization': LOG yourAccessKeyId:yourSignature
x-log-date: Tue, 01 Dec 2020 01:36:00 GMT
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    SplitShard接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-test-project|Project名称。|
    |logstoreName|String|是|logstorename|Logstore名称。|
    |shardID|Integer|是|33|Shard ID。|
    |splitKey|String|是|ef000000000000000000000000000000|分裂位置。|


## 返回数据

-   响应头

    SplitShard接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，表示请求成功。请求成功后，其响应Body中包含3个Shard元素组成的数组，第一个Shard为分裂之前的Shard，后两个为分裂的结果。各参数如下所示：

    |参数名称|数据类型|示例值|描述|
    |----|----|---|--|
    |shardID|Integer|33|Shard ID。|
    |status|String|readonly|分区状态包括：    -   readonly：只读数据
    -   readwrite：读写数据 |
    |inclusiveBeginKey|String|ee000000000000000000000000000000|分区起始的Key值。|
    |exclusiveEndKey|String|ffffffffffffffffffffffffffffffff|分区结束的Key值。|
    |createTime|String|1453949705|分区的创建时间。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|


## 示例

-   请求示例

    ```
    POST /logstores/logstorename/shards/33 HTTP/1.1
    Header :
    {
    'Content-Length': '0',
    'x-log-bodyrawsize': '0',
    'x-log-apiversion': '0.6.0',
    'x-log-signaturemethod': 'hmac-sha1',
    'Host': 'ali-test-project.cn-hangzhou.sls.aliyuncs.com',
    'Date': 'Tue, 01 Dec 2020 01:36:00 GMT',
    'Authorization': 'LOG yourAccessKeyId:yourSignature',
    'x-log-date': 'Tue, 01 Dec 2020 01:36:00 GMT'
    }
    ```

-   正常返回示例

    ```
    Header:
    {
        "content-length": "57", 
        "server": "nginx/1.6.1", 
        "connection": "close", 
        "date": "Thu, 12 Nov 2015 03:40:31 GMT", 
        "content-type": "application/json", 
        "x-log-requestid": "56440A2F99248C050600C74C"
    }
    Body :
    [
        {
            "shardID": 33,
            "status": "readonly",
            "inclusiveBeginKey": "ee000000000000000000000000000000",
            "exclusiveEndKey": "ffffffffffffffffffffffffffffffff",
            "createTime": 1453949705
        },
        {
            "shardID": 163,
            "status": "readwrite",
            "inclusiveBeginKey": "ee000000000000000000000000000000",
            "exclusiveEndKey": "ef000000000000000000000000000000",
            "createTime": 1453949705
        },
        {
            "shardID": 164,
            "status": "readwrite",
            "inclusiveBeginKey": "ef000000000000000000000000000000",
            "exclusiveEndKey": "ffffffffffffffffffffffffffffffff",
            "createTime": 1453949705
        }
    ]
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|LogStoreNotExist|logstore logstoreName does not exist.|Logstore不存在。|
|400|ParameterInvalid|invalid shard id.|无效Shard ID。|
|400|ParameterInvalid|invalid mid hash.|无效分裂位置参数。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|
|400|LogStoreWithoutShard|logstore has no shard.|Logstore没有Shard。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

