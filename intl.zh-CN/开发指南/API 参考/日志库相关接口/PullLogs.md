# PullLogs

调用PullLogs接口获取指定游标（Cursor）位置的日志数据。

## 接口说明

-   获取日志时必须指定Shard。
-   目前仅支持读取[Protocol Buffer格式](/intl.zh-CN/开发指南/API 参考/公共资源说明/数据编码方式.md)数据。

## 请求语法

```
GET /logstores/logstoreName/shards/shardID HTTP/1.1
Accept: application/x-protobuf
Accept-Encoding: lz4
Authorization: LOG yourAccessKeyId:yourSignature
Date: GMT Date
Host: Projectname.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    PullLogs接口的特有请求头如下所示：

    -   Accept：application/x-protobuf
    -   Accept-Encoding：lz4
    其中，Accept-Encoding取值包括lz4、deflate或双引号（“”）之一。

    关于Log Service API的公共请求头，请参见[公共请求头](/intl.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-test-project|Project名称。|
    |logstoreName|String|是|sls-test-logstore|Logstore名称。|
    |shardID|Integer|是|0|Shard ID。|
    |type|String|是|logs|请求类型，固定取值为logs。|
    |cursor|String|是|MTQ0NzMyOTQwMTEwMjEzMDkwNA|游标，表示从什么位置开始读取数据，相当于起点。|
    |count|Integer|否|1000|返回的Loggroup数目，最小值为1，最大值为1000。|


## 返回数据

-   响应头

    PullLogs接口的特有响应元素如下所示：

    -   x-log-cursor：当前读取数据下一条Cursor。
    -   x-log-count：当前返回数量。
    关于Log Service API的公共响应头，请参见[公共响应头](/intl.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    protobuf格式序列化后的数据（可能经过压缩）。


## 示例

-   请求示例

    ```
    GET /logstores/sls-test-logstore/shards/0?cursor=MTQ0NzMyOTQwMTEwMjEzMDkwNA==&count=1000&type=log  
    Header:
    {
        "Authorization"="LOG yourAccessKeyId:yourSignature", 
        "x-log-bodyrawsize"=0, 
        "User-Agent" : "sls-java-sdk-v-0.6.0", 
        "x-log-apiversion" : "0.6.0", 
        "Host" : "ali-test-project.cn-hangzhou-failover-intranet.sls.aliyuncs.com", 
        "x-log-signaturemethod" : "hmac-sha1", 
        "Accept-Encoding" : "lz4", 
        "Content-Length": 0,
        "Date" : "Thu, 12 Nov 2015 12:03:17 GMT",
        "Content-Type" : "application/x-protobuf", 
        "accept" : "application/x-protobuf"
    }
    ```

-   正常返回示例

    ```
    Header:
    {
        "x-log-count" : "1000", 
        "x-log-requestid" : "56447FB20351626D7C000874", 
        "Server" : "nginx/1.6.1", 
        "x-log-bodyrawsize" : "34121", 
        "Connection" : "close", 
        "Content-Length" : "4231", 
        "x-log-cursor" : "MTQ0NzMyOTQwMTEwMjEzMDkwNA==", 
        "Date" : "Thu, 12 Nov 2015 12:01:54 GMT", 
        "x-log-compresstype" : "lz4", 
        "Content-Type" : "application/x-protobuf"
    }
    Body:
    <protobuf格式loggrouplist内容>压缩后结果
    ```

-   翻页

    如果仅进行翻页，获取下一组Token，不返回数据，可以通过HTTP HEAD方式进行请求。


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|LogStoreNotExist|Logstore logstoreName does not exist.|Logstore不存在。|
|400|ParameterInvalid|Invalid cursor cursor.|游标参数无效。|
|400|ParameterInvalid|ParameterCount should be in \[0-1000\].|count参数取值范围应该为\[0-1000\]。|
|400|ShardNotExist|Shard ShardID does not exist.|Shard不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/intl.zh-CN/开发指南/API 参考/通用错误码.md)。

