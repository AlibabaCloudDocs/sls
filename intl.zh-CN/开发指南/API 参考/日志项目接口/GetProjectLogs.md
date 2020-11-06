# GetProjectLogs

GetProjectLogs是Project级别的SQL查询接口。

## 接口说明

-   该接口的query是一个标准的SQL查询语句。
-   查询的Project在请求的域名中指定。
-   查询的Logstore在查询语句的from条件中指定。可以将Logstore看做是SQL中的表。
-   在查询的SQL条件中必须指定要查询的时间范围，时间范围由\_\_date\_\_（timestamp类型）或\_\_time\_\_（int类型，单位是unix\_time）来指定。
-   当查询涉及的日志数量变化非常大时，日志服务API无法预测需要调用多少次该接口来获取完整结果。所以需要您查看每次请求返回结果中的x-log-progress成员状态值，根据成员状态值来确定是否需要重复调用该接口来获取最终完整结果。需要注意的是，每次重复调用该接口都会重新消耗相同数量的查询CU。

## 请求语法

```
GET /logs query=SELECT avg(latency) as avg_latency FROM  where __date__ >'2017-09-01 00:00:00' and __date__ < '2017-09-02 00:00:00'
Authorization: <AuthorizationString>
Date: Wed, 3 Sept. 2014 08:33:46 GMT
Host: big-game.cn-hangzhou.log.aliyuncs.com
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## 请求参数

-   请求头

    GetProjectLogs接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/intl.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |属性名称|类型|是否必须|示例值|描述|
    |----|--|----|---|--|
    |query|string|是|\* \| SELECT \* FROM <logStoreName\> where \_\_line\_\_ = 'abc' and \_\_date\_\_ \>'2017-09-01 00:00:00' and \_\_date\_\_ < '2017-09-02 00:00:00'&line=20&offset=0 HTTP/1.1|SQL语句。|


## 返回数据

-   响应头

    关于Log Service API的公共响应头，请参见[公共响应头](/intl.zh-CN/开发指南/API 参考/公共响应头.md)。该接口特有的响应头如下表所示。

    |名称|类型|描述|
    |--|--|--|
    |x-log-progress|字符串|查询结果的状态。可以有Incomplete和Complete两个选值，表示本次是否完整。|
    |x-log-count|整型|当前查询结果的日志总数。|
    |x-log-processed-rows|整型|本次查询处理的行数。|
    |x-log-elapsed-millisecond|整型|本次查询消耗的毫秒时间。|

-   响应元素

    GetProjectLogs的响应body是一个数组，数组中每个元素是一条日志结果。

    |名称|类型|描述|
    |--|--|--|
    |\_\_time\_\_|整型|日志的时间戳，精度为秒，从1970-1-1 00:00:00 UTC开始计算的秒数。|
    |\_\_source\_\_|字符串|日志的来源，写入日志时指定。|
    |\[content\]|Key-Value对|日志原始内容。|


## 示例

以杭州地域内名为big-game的Project为例，查询该Project内名为app\_log的Logstore中，主题为groupA的日志数据。查询区间为2014-09-01 00:00:00到2014-09-01 22:00:00，查询关键字为error，且从时间区间头开始查询，最多返回20条日志数据。

-   请求示例

    ```
    GET /logs/?query=SELECT * FROM <logStoreName> where __line__ = 'abc' and __date__ >'2017-09-01 00:00:00' and __date__ < '2017-09-02 00:00:00'&line=20&offset=0 HTTP/1.1
    Authorization: <AuthorizationString>
    Date: Wed, 3 Sept. 2014 08:33:46 GMT
    Host: big-game.cn-hangzhou.log.aliyuncs.com   //big-game为Project名称，请根据实际情况替换。
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
                "__source__": "10.237.0.17",
                "Key1": "error",
                "Key2": "Value2"
            },
            {
                "__time__": 1409529680,
                "__source__": "10.237.0.18",
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
|400|ParameterInvalid|parameter is invalid.|请求的参数错误，详情请参见具体的错误message。|

更多错误码，请参见[通用错误码](/intl.zh-CN/开发指南/API 参考/通用错误码.md)。

