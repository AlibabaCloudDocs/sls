# GetLogs

调用GetLogs接口查询指定Project下某个Logstore中的日志数据。

## 接口说明

-   该接口中由请求参数from和to定义的时间区间遵循左闭右开原则，即该时间区间包括区间开始时间点，但不包括区间结束时间点。如果from和to的值相同，则为无效区间，函数直接返回错误。
-   当查询涉及的日志数量变化非常大时，日志服务API无法预测需要调用多少次该接口来获取完整结果。所以需要您查看每次请求返回结果中的x-log-progress状态值，根据状态值来确定是否需要重复调用该接口来获取最终完整结果。每次重复调用该接口都会重新消耗相同数量的查询CU。
-   当日志写入到Logstore中，日志服务的查询接口（GetHistograms和GetLogs）能够查到该日志的延时因写入日志类型不同而异。日志服务按日志时间戳把日志分为如下两类：

    -   实时数据：日志中时间点为服务器当前时间点（-180秒，900秒\]。例如，日志时间为UTC 2014-09-25 12:03:00，服务器收到时为UTC 2014-09-25 12:05:00，则该日志被作为实时数据处理，一般出现在正常场景下。
    -   历史数据：日志中时间点为服务器当前时间点\[-7x86400秒，-180秒）。例如，日志时间为UTC 2014-09-25 12:00:00，服务器收到时为UTC 2014-09-25 12:05:00，则该日志被作为历史数据处理，一般出现在补数据场景下。
    其中，实时数据写入至可查询的最大延时为3秒，99.9%情况下1秒内即可查询完毕。


## 请求语法

```
GET /logstores/logstoreName?type=log&topic=topic&from=from&to=to&query=query&line=line&offset=offset&reverse=reverse HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
Date: GMT Date
Host: ProjectName.Endpoint
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    GetLogs接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|big-game|Project名称。|
    |logstoreName|String|是|app\_log|Logstore名称。|
    |type|String|是|log|查询Logstore数据的类型。在该接口中固定取值为log。|
    |from|Integer|是|1409529600|查询开始时间点。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|
    |to|Integer|是|1409608800|查询结束时间点。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|
    |topic|String|否|groupA|日志主题。|
    |query|String|否|error|查询分析语法。关于查询分析的详细语法，请参见[实时分析简介](/cn.zh-CN/查询与分析/实时分析简介.md)。|
    |line|Integer|否|20|请求返回的最大日志条数。最小值为0，最大值为100，默认值为100。|
    |offset|Integer|否|0|查询开始行。默认值为0。|
    |reverse|Boolean|否|false|是否按日志时间戳逆序返回日志，精确到分钟级别。默认值为false。    -   true：按照逆序返回日志。
    -   false：按照顺序返回日志。 |


## 返回数据

-   响应头

    GetLogs接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

    响应头中有专门成员表示请求返回结果是否完整。具体响应元素格式如下：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |x-log-progress|String|Complete|查询结果的状态，包括：    -   Complete：查询已经完成，返回结果为完整结果。
    -   Incomplete：查询已经完成，但由于返回结果数据量大且结果时间区间跨度大，导致返回结果为不完整结果。需要重复请求以获得完整结果。 |
    |x-log-count|Integer|10000|当前查询扫描的日志总数。|

-   响应元素

    返回HTTP状态码200，则表示请求成功，其响应Body会包括查询命中的日志数据。当需要查询的日志数据量非常大（T级别）时，该接口的响应结果可能并不完整，GetLogs的响应body是一个数组，数组中每个元素是一条日志结果。数组中的每个元素结构如下：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |\_\_time\_\_|Integer|1409529660|日志的时间戳。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|
    |\_\_source\_\_|String|192.168.1.100|日志的来源，写入日志时指定。例如产生该日志的机器的IP地址。|
    |contents|List|\["Key1": "error","Key2": "Value2"\]|日志原始内容。|


## 示例

以杭州地域内名为big-game的Project为例，查询该Project内名为app\_log的Logstore中，主题为groupA的日志数据。查询区间为2014-09-01 00:00:00到2014-09-01 22:00:00，查询关键字为error，且从时间区间头开始查询，最多返回20条日志数据。

-   请求示例：

    ```
    GET /logstores/app_log?type=log&topic=groupA&from=1409529600&to=1409608800&query=error&line=20&offset=0 HTTP/1.1
    Header :
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    Date: Wed, 3 Sept. 2014 08:33:46 GMT
    Host: big-game.cn-hangzhou.log.aliyuncs.com
    x-log-bodyrawsize: 0
    x-log-apiversion: 0.4.0
    x-log-signaturemethod: hmac-sha1
    }
    ```

-   正常返回示例：

    ```
    HTTP/1.1 200 OK
    Header :
    {
    Content-MD5: 36F9F7F0339BEAF571581AF1B0AAAFB5
    Content-Type: application/json
    Content-Length: 269
    Date: Wed, 3 Sept. 2014 08:33:47 GMT
    x-log-requestid: efag01234-12341-15432f
    x-log-progress : Complete
    x-log-count : 10000
    x-log-processed-rows: 10000
    x-log-elapsed-millisecond:5
    }
    Body :
    {
        "progress": "Complete",
        "count": 2,
        "logs": [
            {
                "__time__": 1409529660,
                "__source__": "192.168.1.100",
                "contents": ["Key1": "error","Key2": "Value2"]
            },
            {
                "__time__": 1409529680,
                "__source__": "192.168.1.100",
                "contents": ["Key3": "error","Key4": "Value4"]
            }
        ]
    }
    ```


在这个响应示例中，x-log-progress成员的状态为Complete，表明整个日志查询已经完成，返回结果为完整结果。在这次请求中共查询到2条符合条件的日志，且日志数据在logs成员中。如果响应结果中的x-log-progress成员的状态为Incomplete，则表示查询结果不完整，需要重复相同请求以获得完整结果。

## 错误码

|HTTP状态码|错误码|错误消息|描述|
|:------|:--|:---|:-|
|404|ProjectNotExist|Project ProjectName does not exist.|Project不存在。|
|404|LogStoreNotExist|logstore logstoreName does not exist.|Logstore不存在。|
|400|InvalidTimeRange|Request time range is invalid.|请求的时间区间无效。|
|400|InvalidQueryString|Query string is invalid.|请求的查询字符串无效。|
|400|InvalidOffset|Offset is invalid.|请求的offset参数无效。|
|400|InvalidLine|Line is invalid.|请求的line参数无效。|
|400|InvalidReverse|Reverse value is invalid.|Reverse参数的值无效。|
|400|IndexConfigNotExist|Logstore without index config.|Logstore未开启索引。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

