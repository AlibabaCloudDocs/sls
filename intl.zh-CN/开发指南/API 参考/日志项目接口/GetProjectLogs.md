# GetProjectLogs

调用GetProjectLogs接口查询目标Project下的日志，该接口是Project级别的SQL查询接口。

## 接口说明

-   该接口的query是一个标准的SQL查询语句。
-   查询的Project在请求的域名中指定。
-   查询的Logstore在查询语句的from条件中指定。可以将Logstore看做是SQL中的表。
-   在查询的SQL条件中必须指定要查询的时间范围，时间范围由\_\_date\_\_（Timestamp类型）或\_\_time\_\_（Integer类型，单位是秒）来指定。

## 请求语法

```
GET /logs query=SELECT avg(latency) as avg_latency FROM  where __date__ >'2017-09-01 00:00:00' and __date__ < '2017-09-02 00:00:00'
Authorization: LOG yourAccessKeyId:yourSignature
Date: Wed, 3 Sept. 2014 08:33:46 GMT
Host: Projectname.endpoint
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    GetProjectLogs接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/intl.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |----|----|----|---|--|
    |projectName|String|是|big-game|Project名称。|
    |query|String|是|\* \| SELECT \* FROM logStoreName where \_\_line\_\_ = 'error' and \_\_date\_\_ \>'2014-09-01 00:00:00' and \_\_date\_\_ < '2014-09-01 22:00:00|SQL语句，表示查询时间区间为2014-09-01 00:00:00到2014-09-01 22:00:00，关键字为error的日志数据。|


## 返回数据

-   响应头

    关于Log Service API的公共响应头，请参见[公共响应头](/intl.zh-CN/开发指南/API 参考/公共响应头.md)。该接口特有的响应头如下表所示。

    |参数名称|数据类型|示例值|描述|
    |----|----|---|--|
    |x-log-progress|String|Complete|查询结果的状态，包括：    -   Complete：查询已经完成，返回结果为完整结果。
    -   Incomplete：查询已经完成，返回结果为不完整结果，需要重复请求以获得完整结果。 |
    |x-log-count|Integer|10000|当前查询结果的日志总数。|
    |x-log-processed-rows|Integer|10000|本次查询处理的行数。|
    |x-log-elapsed-millisecond|Integer|5|本次查询消耗的毫秒时间。|

-   响应元素

    GetProjectLogs的响应body是一个数组，数组中每个元素是一条日志结果。

    |参数名称|数据类型|示例值|描述|
    |----|----|---|--|
    |\_\_time\_\_|Integer|1409529660|日志的时间戳，精度为秒，从1970-1-1 00:00:00 UTC开始计算的秒数。|
    |\_\_source\_\_|String|192.168.1.100|日志的来源，写入日志时指定。|
    |content|Array|"Key3": "error", "Key4": "Value4"|日志原始内容。|


## 示例

以杭州地域内名为big-game的Project为例，查询该Project内名为app\_log的Logstore中，主题为groupA的日志数据。查询区间为2014-09-01 00:00:00到2014-09-01 22:00:00，查询关键字为error的日志数据。

-   请求示例

    ```
    GET /logs/?query=SELECT * FROM logStoreName where __line__ = 'error' and __date__ >'2014-09-01 00:00:00' and __date__ < '2014-09-01 22:00:00'&line=20&offset=0 HTTP/1.1
    Authorization: LOG yourAccessKeyId:yourSignature
    Date: Wed, 3 Sept. 2014 08:33:46 GMT
    Host: big-game.cn-hangzhou.log.aliyuncs.com
    x-log-bodyrawsize: 0
    x-log-apiversion: 0.4.0
    x-log-signaturemethod: hmac-sha1
    ```

-   响应示例

    ```
    HTTP/1.1 200 OK
    Content-MD5: 36F9F7F0339BEAF571581AF1B0AAAFB5
    Content-Type: application/json
    Content-Length: 269
    Date: Wed, 3 Sept. 2014 08:33:47 GMT
    x-log-requestid: efag01234-12341-15432f
    x-log-progress : Complete
    x-log-count : 10000
    x-log-processed-rows: 10000
    x-log-elapsed-millisecond:5
    {
        "progress": "Complete",
        "count": 2,
        "logs": [
            {
                "__time__": 1409529660,
                "__source__": "192.168.1.100",
                "Key1": "error",
                "Key2": "Value2"
            },
            {
                "__time__": 1409529680,
                "__source__": "192.168.1.100",
                "Key3": "error",
                "Key4": "Value4"
            }
        ]
    }
    ```

    响应示例中，x-log-progress的值为Complete，表明整个日志查询已经完成，返回结果为完整结果。本次请求共查询到2条符合条件的日志数据。如果响应结果中的x-log-progress的值为Incomplete，则需要重复请求以获得完整结果。


## 错误码

|HTTP状态码|错误码|错误消息|描述|
|-------|---|----|--|
|400|ParameterInvalid|Parameter is invalid.|请求的参数错误。|

更多错误码，请参见[通用错误码](/intl.zh-CN/开发指南/API 参考/通用错误码.md)。

