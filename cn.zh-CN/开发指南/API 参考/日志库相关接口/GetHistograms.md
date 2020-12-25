# GetHistograms

调用GetHistograms接口查询指定Project下某个Logstore中日志的分布情况。还可以通过指定相关参数仅查询符合条件的日志分布情况。

## 接口说明

-   该接口中涉及的时间区间（无论是请求参数from和to定义的时间区间，还是返回结果中各个子时间区间）都遵循“左闭右开”原则，即该时间区间包括区间开始时间点，但不包括区间结束时间点。如果from和to的值相同，则为无效区间，函数直接返回错误。
-   该接口的响应中子区间划分方式是一致稳定的。如果您在请求查询的时间区间不变，则响应中子区间划分结果也不会改变。
-   当查询涉及的日志数量变化非常大时，日志服务API无法预测需要调用多少次该接口来获取完整结果。所以需要您查看每次请求返回结果中的progress成员状态值，根据成员状态值来确定是否需要重复调用该接口来获取最终完整结果。每次重复调用该接口都会重新消耗相同数量的查询CU。
-   从日志写入日志库到查询接口（GetHistograms和GetLogs）查到该日志，延时时长因写入日志类型不同而异。日志服务按日志时间戳把日志分为如下两类：
    -   实时数据：日志中时间点为服务器当前时间点（-180秒，900秒\]。例如，日志时间为UTC 2014-09-25 12:03:00，服务器收到时为 UTC 2014-09-25 12:05:00，则该日志被视作实时数据处理。实时数据从写入到在日志查询界面查询到该数据的延迟为3秒。
    -   历史数据：日志中时间点为服务器当前时间点\[-7 x 86400秒，-180秒）。例如，日志时间为UTC 2014-09-25 12:00:00，服务器收到时为UTC 2014-09-25 12:05:00，则该日志被作为历史数据处理，一般出现在补数据场景下。

## 请求语法

```
GET /logstores/logstoreName?type=histogram&topic=topic&from=from&to=to&query=query HTTP/1.1
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

    GetHistograms接口无特有请求头。关于Log Service API的公共请求头，请参见 [公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|big-game|Project名称。|
    |logstoreName|String|是|app\_log|Logstore名称。|
    |type|String|是|histogram|Logstore中数据的类型。该接口中固定取值为histogram。|
    |from|Integer|是|1409529600|查询开始时间点。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|
    |to|Integer|是|1409608800|查询结束时间点。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|
    |topic|String|否|groupA|日志主题。|
    |query|String|否|error|查询表达式。关于查询表达式的详细语法，请参见[查询语法](/cn.zh-CN/查询与分析/查询语法与功能/查询语法.md)。|


## 返回数据

-   响应头

    GetHistograms接口无特有响应头。关于Log Service API的公共响应头，请参见 [公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功，其响应Body会包括查询命中的日志数量在时间轴上的分布情况。响应结果会把您查询的时间范围均匀分割成若干个子时间区间，并返回每个子时间区间内命中的日志条数。具体如下：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |progress|String|Complete|查询结果的状态。    -   Complete：查询已经完成，返回结果为完整结果。
    -   Incomplete：查询已经完成，返回结果为不完整结果，需要重复请求以获得完整结果。 |
    |count|Integer|2|当前查询结果中所有日志条数。|
    |histograms|Array|无|当前查询结果在划分的子时间区间上的分布状况，具体结构见下面的描述。|

    histograms数组中的每个元素结构如下：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |from|Integer|1409529600|子时间区间的开始时间点。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|
    |to|Integer|1409569200|子时间区间的结束时间点。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|
    |count|Integer|2|该子时间区间内查询到的日志条数。|
    |progress|Sting|Complete|当前查询结果在该子时间区间内的结果是否完整。    -   Complete：查询已经完成，返回结果为完整结果。
    -   Incomplete：查询已经完成，返回结果为不完整结果，需要重复请求以获得完整结果。 |


## 示例

以杭州地域内名为big-game的Project为例，查询该Project内名为app\_log的Logstore中，主题为groupA的日志分布情况。查询区间为2014-09-01 00:00:00到2014-09-01 22:00:00，查询关键字是error。

-   请求示例

    ```
    GET /logstores/app_log?type=histogram&topic=groupA&from=1409529600&to=1409608800&query=error HTTP/1.1
    Header :
    {
    Authorization: LOG <yourAccessKeyId>:<yourSignature>
    Date: Wed, 3 Sept. 2014 08:33:46 GMT
    Host: big-game.cn-hangzhou.log.aliyuncs.com
    x-log-bodyrawsize: 0
    x-log-apiversion: 0.6.0
    x-log-signaturemethod: hmac-sha1
    }
    ```

-   响应示例

    ```
    HTTP/1.1 200 OK
    Header :
    {
    Content-Type: application/json
    Content-MD5: E6AD9C21204868C2DE84EE3808AAA8C8
    Content-Type: application/json
    Date: Wed, 3 Sept. 2014 08:33:47 GMT
    Content-Length: 232
    x-log-requestid: efag01234-12341-15432f
    }
    Body :
    {
            {
                "from": 1409529600,
                "to": 1409569200,
                "count": 2,
                "progress": "Complete"
            },
            {
                "from": 1409569200,
                "to": 1409608800,
                "count": 2,
                "progress": "Complete"
            }
    }
    ```

    在下面这个响应示例中，服务端把整个Histogram划分到两个均等的时间区间，分别为\[2014-09-01 00:00:00，2014-09-01 11:00:00）和\[2014-09-01 11:00:00，2014-09-01 22:00:00）。由于您的数据量过大，第一次查询会返回不完整数据。响应结果告知您命中日志条数为3，但整体结果不完整，仅在时间段\[2014-09-01 00:00:00，2014-09-01 11:00:00）结果完整且命中2条，而在另外一个时间段结果不完整但已经命中1条。在这个情况下，如果您希望得到完整结果，则需要重复调用上面的请求示例若干次直到整个响应中的progress成员值变成Complete。响应示例如下：

    ```
    HTTP/1.1 200 OK
    Header :
    {
    Content-Type: application/json
    Content-MD5: E6AD9C21204868C2DE84EE3808AAA8C8
    Content-Type: application/json
    Date: Wed, 3 Sept. 2014 08:33:48 GMT
    Content-Length: 232
    x-log-requestid: afag01322-1e241-25432e
    }
    Body :
    {
            {
                "from": 1409529600,
                "to": 1409569200,
                "count": 2,
                "progress": "Complete"
            },
            {
                "from": 1409569200,
                "to": 1409608800,
                "count": 1,
                "progress": "Incomplete"
            }
    }
    ```


## 错误码

|HTTP状态码|错误码|错误消息|描述|
|:------|:--|:---|:-|
|404|ProjectNotExist|Project ProjectName does not exist.|Project不存在。|
|404|LogStoreNotExist|logstore logstoreName does not exist.|Logstore不存在。|
|400|InvalidTimeRange|request time range is invalid.|请求的时间区间无效。|
|400|InvalidQueryString|query string is invalid.|请求的查询字符串无效。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

