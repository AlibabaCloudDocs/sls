# UpdateLogstore

调用UpdateLogstore接口更新Logstore的属性信息。

## 接口说明

目前该接口只支持更新数据保存时间（TTL）属性。

## 请求语法

```
PUT /logstores/logstoreName HTTP/1.1
Content-Length: 0
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: ProjectName.Endpoint
Date: GMT Date
Authorization : LOG yourAccessKeyId:yourSignature 
x-log-date: Wed, 11 Nov 2015 08:28:19 GMT
{
    "logstoreName": logstoreName,
    "ttl": ttl,
    "enable_tracking": false,
    "shardCount": shardCount,
    "autoSplit": true,
    "maxSplitShard": 64,
    "appendMeta": false
}
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    UpdateLogstore接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |logstoreName|String|是|test-logstore|日志库名称。|
    |ttl|Integer|是|1|数据的保存时间，单位为天。取值范围为1~3650。如果配置为3650，表示永久保存。|
    |enable\_tracking|Boolean|否|false|是否开启WebTracking。    -   true：开启WebTracking。
    -   false：不开启WebTracking。 |
    |shardCount|Integer|是|2|Shard分区个数。**说明：**

    -   该接口不支持更新分区个数。
    -   只能通过[SplitShard](/cn.zh-CN/开发指南/API 参考/日志库相关接口/SplitShard.md)或[MergeShards](/cn.zh-CN/开发指南/API 参考/日志库相关接口/MergeShards.md)接口修改分区个数。 |
    |autoSplit|Boolean|否|true|是否自动分裂Shard。    -   true：自动分裂Shard。
    -   false：不自动分裂Shard。 |
    |maxSplitShard|Integer|否|64|自动分裂时最大的Shard个数 ，最小值是1，最大值是64。**说明：** 当autoSplit为true时必须指定。 |
    |appendMeta|Boolean|否|false|是否开启记录外网IP地址。    -   true：开启记录外网IP地址。
    -   false：不开启记录外围IP地址。 |


## 返回数据

-   响应头

    UpdateLogstore接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功。


## 示例

-   请求示例

    ```
    PUT /logstores/test-logstore HTTP/1.1
    Header:
    {
    'x-log-bodyrawsize': '0',
    'Content-Type': 'application/json',
    'Content-Length': '143',
    'Content-MD5': '118D09033B9A4DCBA93A8A496EA095CF',
    'x-log-apiversion': '0.6.0', 
    'x-log-signaturemethod': 'hmac-sha1',
    'Host': 'ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com',
    'Date': 'Wed, 11 Nov 2015 08:28:19 GMT', 
    'Authorization': 'LOG yourAccessKeyId:yourSignature', 
    'x-log-date': 'Wed, 11 Nov 2015 08:28:19 GMT'
    }
    Body : 
    {
        "logstoreName": "test-logstore",
        "ttl": 1,
        "enable_tracking": false,
        "shardCount": 2,
        "autoSplit": true,
        "maxSplitShard": 64
        "appendMeta": false
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header:
    {
    'Server': 'Tengine',
    'Content-Length': '0', 
    'Connection': 'close', 
    'Access-Control-Allow-Origin': '*', 
    'Date': 'Wed, 11 Nov 2015 08:28:20 GMT', 
    'x-log-requestid': '5FC4490F5D13436DA713CBEE'
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|Project ProjectName does not exist.|Project不存在。|
|404|LogStoreNotExist|logstore logstoreName does not exist.|LogStore不存在。|
|400|LogStoreAlreadyExist|logstore logstoreName already exists.|LogStore已存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|
|400|ParameterInvalid|invalid shard count,you can only modify shardCount by split& merge shard.|无效Shard个数，您只能通过SplitShard或MergeShards接口修改分区个数（shardCount）属性。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

