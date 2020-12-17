# GetLogstore

调用GetLogstore接口查看Logstore的详细信息。

## 请求语法

```
GET /logstores/logstoreName HTTP/1.1
Content-Length: 0
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: Projectname.Endpoint
Date: GMT Date
Authorization: LOG yourAccessKeyId:yourSignature
x-log-date: Wed, 11 Nov 2015 07:53:29 GMT
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    GetLogstore接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |logstoreName|String|是|test-logstore|日志库名称。|


## 返回数据

-   响应头

    GetLogstore接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，表示请求成功，其响应Body包括具体Logstore属性信息。具体如下：

    |参数名称|数据类型|示例值|描述|
    |----|----|---|--|
    |logstoreName|String|test-logstore|日志库名称。|
    |ttl|Integer|1|数据的保存时间，单位为天。|
    |shardCount|Integer|2|Shard个数。|
    |enable\_tracking|Boolean|false|是否开启WebTracking。WebTracking即网络跟踪功能，支持采集HTML、H5、iOS和Android平台的日志。    -   true：开启WebTracking。
    -   false：不开启WebTracking。 |
    |autoSplit|Boolean|true|是否自动分裂Shard。    -   true：自动分裂Shard。
    -   false：不自动分裂Shard。 |
    |maxSplitShard|Integer|64|自动分裂时最大的Shard个数，最小值是1，最大值是64。|
    |appendMeta|Boolean|false|是否开启记录外网IP地址。    -   true：开启记录外网IP地址。
    -   false：不开启记录外围IP地址。 |
    |createTime|Integer|1447833064|日志库创建时间。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|
    |lastModifyTime|Integer|1447833064|日志库最后一次更新时间。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|


## 示例

-   请求示例

    ```
    GET /logstores/test-logstore HTTP/1.1
    Header :
    {
    'Content-Length': '0',
    'x-log-bodyrawsize': '0',
    'x-log-apiversion': '0.6.0',
    'x-log-signaturemethod': 'hmac-sha1',
    'Host': 'ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com',
    'Date': 'Wed, 11 Nov 2015 07:53:29 GMT',
    'Authorization': 'LOG yourAccessKeyId:yourSignature',
    'x-log-date': 'Wed, 11 Nov 2015 07:53:29 GMT'
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header :
    {
    'Server': 'Tengine',
    'Content-Type': 'application/json',
    'Content-Length': '274',
    'Connection': 'close',
    'Access-Control-Allow-Origin': '*',
    'Date': 'Mon, 30 Nov 2020 02:27:58 GMT',
    'x-log-requestid': '5FC458AE54F7D19F960DB515'
    }
    Body :
    {
        "logstoreName": test-logstore,
        "ttl": 1,
        "shardCount": 2,
        "enable_tracking": False,
        "autoSplit": True,
        "maxSplitShard": 64,
        "createTime": 1447833064,
        "lastModifyTime": 1447833064,
        "appendMeta': False,
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|Project ProjectName does not exist.|Project不存在。|
|404|LogstoreNotExist|logstore logstoreName does not exist.|Logstore不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

