# GetCursor

调用GetCursor接口可以根据时间获取对应的游标（Cursor）。

## 接口说明

通过游标（Cursor）可以获取特定日志对应的位置。关于Cursor与Project、Logstore、Shard的关系如下图所示。

![游标](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0578498951/p6710.png)

-   Project下有多个Logstore。
-   每个Logstore会有多个Shard。
-   通过Cursor可以获得特定日志对应的位置。

## 请求语法

```
GET /logstores/logstoreName/shards/shardID HTTP/1.1
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

    GetCursor接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-test-project|Project名称。|
    |logstoreName|String|是|sls-test-logstore|Logstore名称。|
    |shardID|String|是|0|Shard ID。|
    |type|String|是|cursor|请求类型。固定取值为cursor。|
    |from|String|是|begin|时间点（Unix时间戳）或者字符串`begin`、`end`。|

    通过from可以在Shard中定位生命周期内的日志，假设Logstore的生命周期为\[begin\_time,end\_time\)，from=from\_time，那么：

    -   当`from_time ≤ begin_time or from_time = "begin"`时：返回时间点为begin\_time对应的Cursor位置。
    -   当`from_time ≥ end_time or from_time = "end"`时：返回当前时间点下一条将被写入的Cursor位置（当前该Cursor位置上无数据）。
    -   当`from_time > begin_time and from_time < end_time`时：返回第一个服务端接收时间大于等于from\_time的数据包对应的Cursor。
    **说明：** Logstore生命周期由属性中TTL字段指定。例如，当前时间为2018-11-11 09:00:00，TTL=5。则每个Shard中可以消费的数据时间段为 `[2018-11-05 09:00:00,2018-11-11 09:00:00)`，这里的时间指的是服务端时间。


## 返回数据

-   响应头

    GetCursor接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功，其响应Body会包括对应游标信息，具体如下：

    |参数名称|数据类型|示例值|描述|
    |----|----|---|--|
    |cursor|String|MTQ0NzI5OTYwNjg5NjYzMjM1Ng==|Cursor值。|


## 示例

-   请求示例

    ```
    GET /logstores/sls-test-logstore/shards/0?type=cursor&from=begin
    Header:
    {
        "Content-Length": 0, 
        "x-log-signaturemethod": "hmac-sha1", 
        "x-log-bodyrawsize": 0, 
        "User-Agent": "log-python-sdk-v-0.6.0", 
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com", 
        "Date": "Thu, 12 Nov 2015 03:56:57 GMT", 
        "x-log-apiversion": "0.6.0", 
        "Content-Type": "application/json", 
        "Authorization": "LOG yourAccessKeyId:yourSignature"
    }
    ```

-   正常返回示例

    ```
    Header:
    {
        "content-length": "41", 
        "server": "nginx/1.6.1", 
        "connection": "close", 
        "date": "Thu, 12 Nov 2015 03:56:57 GMT", 
        "content-type": "application/json", 
        "x-log-requestid": "56440E0999248C070600C6AA"
    }
    Body:
    {
        "cursor": "MTQ0NzI5OTYwNjg5NjYzMjM1Ng=="
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|LogStoreNotExist|Logstore logstoreName does not exist.|Logstore不存在。|
|400|ParameterInvalid|Parameter From is not valid.|无效参数。|
|400|ShardNotExist|Shard ShardID does not exist.|Shard不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|
|400|LogStoreWithoutShard|The logstore has no shard.|Logstore没有Shard。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

