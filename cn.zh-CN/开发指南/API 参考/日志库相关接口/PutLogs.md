# PutLogs

调用PutLogs接口向指定的Logstore中写入日志数据。

## 接口说明

-   服务端会对每次PutLogs写入的日志数据做格式检查，只要日志数据中有任何一条日志不符合规范，则整个请求失败且无任何日志数据成功写入。
-   目前仅支持写入[PB格式](/cn.zh-CN/开发指南/API 参考/公共资源说明/数据编码方式.md)的日志数据，日志数据以LogGroup的形式展示。
-   日志数据写入时有两种模式：
    -   负载均衡模式（LoadBalance）：自动根据Logstore下所有可写的Shard进行负载均衡写入。该方法写入可用性较高（SLA：99.95%），适合写入消费数据与Shard无关的场景，例如不保序。
    -   Key路由Shard模式（KeyHash）：在URL参数中增加Key字段，用来判断数据写入哪个Shard中。该参数为可选参数，不设置时自动切换为负载均衡写入模式。例如，可以将某个生产者（例如instance）根据名称Hash固定到Shard上，这样就能保证写入与消费在该Shard上的数据是严格有序的（在合并、分裂过程中能够严格保证对于Key在一个时间点只会出现在一个Shard上，请参见[分区](/cn.zh-CN/产品简介/基本概念/分区.md)）。
-   PutLogs接口每次可以写入的日志组数据量上限为3 MB或者4096条，日志组中每条日志下的Value部分不超过1 MB大小。只要日志数据量超过这两条上限中的任意一条则整个请求失败，且无任何日志数据成功写入。

## PB数据

PB格式日志压缩数据字段描述如下。详情请参见[数据模型](/cn.zh-CN/开发指南/API 参考/公共资源说明/数据模型.md)和[数据编码方式](/cn.zh-CN/开发指南/API 参考/公共资源说明/数据编码方式.md)。

-   Log

    |参数名称|数据类型|是否必填|描述|
    |----|----|----|--|
    |Time|Integer|是|日志时间。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|
    |Contents|List|是|日志字段列表，至少包含一个元素，每个元素类型请参见[Content](#table_ghl_68w_i35)表格。|

-   Content

    |参数名称|数据类型|是否必填|描述|
    |----|----|----|--|
    |Key|String|是|自定义Key名称。|
    |Value|String|是|自定义Key对应的值。|

-   LogTag

    |参数名称|数据类型|是否必填|描述|
    |----|----|----|--|
    |Key|String|是|自定义Key名称。|
    |Value|String|是|自定义Key对应的值。|

-   LogGroup

    |参数名称|数据类型|是否必填|描述|
    |----|----|----|--|
    |Logs|List|是|日志列表，每个元素请参见[Log](#table_7r8_0fs_hm7)字段表格。|
    |Topic|String|否|日志主题，用户自定义字段，用于区分不同特征的日志数据。|
    |Source|String|否|日志的来源。例如产生该日志的机器的IP地址。|
    |LogTags|List|是|日志的标签列表，每个元素请参见[LogTag](#table_ghl_68w_i35)。|


## 请求语法

-   负载均衡模式

    ```
    POST /logstores/logstoreName/shards/lb HTTP/1.1
    Authorization: LOG yourAccessKeyId:yourSignature
    Content-Type: application/x-protobuf
    Content-Length: Content Length
    Content-MD5: Content MD5
    Date: GMT Date
    Host: ProjectName.Endpoint
    x-log-apiversion: 0.6.0
    x-log-bodyrawsize: BodyRawSize
    x-log-compresstype: lz4
    x-log-signaturemethod: hmac-sha1
    <PB 格式日志压缩数据>
    ```

-   Key路由Shard模式

    ```
    POST /logstores/logstoreName/shards/route?key=14d2f850ad6ea48e46e4547edbbb27e0
    Authorization: LOG yourAccessKeyId:yourSignature
    Content-Type: application/x-protobuf
    Content-Length: Content Length
    Content-MD5: Content MD5
    Date: GMT Date
    Host: ProjectName.Endpoint
    x-log-apiversion: 0.6.0
    x-log-bodyrawsize: BodyRawSize
    x-log-compresstype: lz4
    x-log-signaturemethod: hmac-sha1
    <PB 格式日志压缩数据>
    ```


其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   请求参数

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-test-project|Project名称。|
    |logstoreName|String|是|sample-logtail-config|Logstore名称。|


## 返回数据

-   响应头

    PutLogs接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功。该接口调用成功后无任何响应元素。


## 示例

-   请求示例

    ```
    POST /logstores/sls-test-logstore
    {
        "Content-Length": 118,
        "Content-Type":"application/x-protobuf",
        "x-log-bodyrawsize":1356,
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
        "Content-MD5":"6554BD042149C844761C2C094A8FECCE",
        "Date":"Thu, 12 Nov 2015 06:54:26 GMT",
        "x-log-apiversion": "0.6.0",
        "x-log-compresstype":"lz4"
        "x-log-signaturemethod": "hmac-sha1",
        "Authorization":"LOG yourAccessKeyId:yourSignature"
    }
    <PB格式日志使用Lz4压缩后的二进制数据>
    ```

-   正常返回示例

    ```
    Header
    {   
        "Date" : "Wed, 11 Nov 2015 08:28:20 GMT", 
        "Content-Length" : 0, 
        "x-log-requestid" : "5642FC2399248C8F7B0145FD", 
        ""Connection" : "close", 
        "Server" : "nginx/1.6.1"
    }
    ```


## 错误码

|HTTP状态码|错误码|错误消息|描述|
|:------|:--|:---|:-|
|400|PostBodyInvalid|Protobuffer content cannot be parsed.|Protobuffer内容不合法，无法解析。|
|400|InvalidTimestamp|Invalid timestamps are in logs.|日志内容中有无效的日志时间戳。|
|400|InvalidEncoding|Non-UTF8 characters are in logs.|日志内容中有非UTF8字符。|
|400|InvalidKey|Invalid keys are in logs.|日志内容中有无效的Key。|
|400|PostBodyTooLarge|Logs must be less than or equal to 3 MB and 4096 entries.|日志内容包含的日志必须小于3 MB和4096条。|
|400|PostBodyUncompressError|Failed to decompress logs.|日志内容解压失败。|
|400|PostBodyInvalid|The post data time is out of range|日志中时间范围不在\[-7\*24小时 + 15分钟\]有效范围内。|
|404|LogStoreNotExist|logstore logstoreName does not exist.|Logstore不存在。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

