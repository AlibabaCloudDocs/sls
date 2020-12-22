# CreateLogstore

调用CreateLogstore接口创建Logstore。

## 请求语法

```
POST /logstores HTTP/1.1
x-log-bodyrawsize: 0
Content-Type: application/json
Content-Length: 140
Content-MD5: F3EFCA28442BEEC487451FAD30D78650
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: ProjectName.Endpoint
Date: GMT Date
Authorization: LOG yourAccessKeyId:yourSignature,
x-log-date: Fri, 27 Nov 2020 08:25:10 GMT
{
    "logstoreName" : logStoreName,
    "ttl": ttl,
    "shardCount": shardCount,
    "enable_tracking": enable\_tracking,
    "autoSplit": autoSplit,
    "maxSplitShard": maxSplitShard,
    "appendMeta": appendMeta
}
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    CreateLogstore接口无特有请求头，关于Log Service API的公共请求头，请参见[公共请求头](/intl.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |logstoreName|String|是|test-logstore|Logstore名称。其命名规则如下：    -   同一个Project下，Logstore名称不可重复。
    -   只能包括小写字母、数字、短划线（-）和下划线（\_）。
    -   必须以小写字母或者数字开头和结尾。
    -   长度为3-63字符。 |
    |ttl|Integer|是|30|数据的保存时间，单位为天。取值范围为1~3650。如果配置为3650，表示永久保存。|
    |shardCount|Integer|是|2|Shard个数。取值范围为1~10。|
    |enable\_tracking|Boolean|否|false|是否开启WebTracking功能。默认值为false。    -   true：开启WebTracking。
    -   false：不开启WebTracking。 |
    |autoSplit|Boolean|否|true|是否自动分裂Shard功能。默认值为false，表示不开启。    -   true：自动分裂Shard。
    -   false：不自动分裂Shard。 |
    |maxSplitShard|Integer|否|6|自动分裂Shard时的最大分裂数。取值范围为1~64。当autoSplit参数为true时必须设置。|
    |appendMeta|Boolean|否|false|是否开启记录外网IP地址功能。默认值为false。    -   true：开启记录外网IP地址。
    -   false：不开启记录外围IP地址。 |


## 返回数据

-   响应头

    CreateLogstore接口无特有响应头，关于Log Service API的公共响应头，请参见[公共响应头](/intl.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，表示创建Logstore成功。


## 示例

-   请求示例

    ```
    POST /logstores HTTP/1.1
    Header :
    {
        'x-log-bodyrawsize': '0',
        'Content-Type': 'application/json',
        'Content-Length': '143',
        'Content-MD5': '2ABE4729F2FE8031328CFA4B9D8A34FB',
        'x-log-apiversion': '0.6.0',
        'x-log-signaturemethod': 'hmac-sha1',
        'Host': 'ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com,
        'Date' : 'Wed, 11 Nov 2015 07:35:00 GMT',
        'Authorization': 'LOG yourAccessKeyId:yourSignature,
        'x-log-date': 'Wed, 11 Nov 2015 07:35:00 GMT'
    
    }
    Body : 
    {
        "logstoreName": "test-logstore",
        "ttl": 30,
        "shardCount": 2,
        "enable_tracking": false,
        "autoSplit": true,
        "maxSplitShard": 6,
        "appendMeta": false
    }
    ```

-   返回示例

    ```
    HTTP/1.1 200 OK
    Header:
    {
        'Server': 'Tengine',
        'Content-Length': '0',
        'Connection': 'close', 
        'Access-Control-Allow-Origin': '*', 
        'Date': 'Wed, 11 Nov 2015 07:35:00 GMT', 
        'x-log-requestid': '5FC0B8115F2EBE6550063F90'
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|400|LogstoreAlreadyExist|logstore logstoreName already exists.|Logstore已存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|
|400|LogStoreInfoInvalid|store name logstoreName is invalid|Logstore信息无效。|
|400|ProjectQuotaExceed|Project Quota Exceed.|超过项目限额。|

更多错误码，请参见[通用错误码](/intl.zh-CN/开发指南/API 参考/通用错误码.md)。

