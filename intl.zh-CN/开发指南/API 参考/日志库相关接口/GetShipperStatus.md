# GetShipperStatus

调用GetShipperStatus接口查询日志投递任务状态。

## 接口说明

该接口只支持查询最近48小时之内的投递任务状态。

## 请求语法

```
GET /logstores/logstoreName/shipper/shipperName/tasks HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    GetShipperStatus接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/intl.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-test-project|Project名称。|
    |logstoreName|String|是|test-logstore|Logstore名称。|
    |shipperName|String|是|test-shipper|日志投递名称。|
    |startTime|Integer|是|1448748198|日志投递任务创建开始时间。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|
    |endTime|Integer|是|1448948198|日志投递任务创建结束时间。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|
    |statusType|String|否|success|默认为空，表示返回所有状态的任务，支持success、fail和running状态。|
    |offset|Integer|否|0|查询开始行，默认值为0。|
    |size|Integer|否|100|分页查询时，设置的每页行数。默认值为100，最大值为500。|


## 返回数据

-   响应头

    GetShipperStatus接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/intl.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功，其响应Body会包括指定的日志投递任务列表，具体如下：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |count|Integer|10|当前页返回的任务个数。|
    |total|Integer|20|任务总数。|
    |statistics|Json|无|任务汇总状态，具体请参见下表。|
    |tasks|Array|无|投递任务详情，具体请参见下表。|

    statistics中每个元素，具体如下：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |running|Integer|0|状态为running的任务个数。|
    |success|Integer|20|状态为success的任务个数。|
    |fail|Integer|0|状态为fail的任务个数。|

    tasks中每个元素，具体如下：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |id|String|abcdefghijk|投递任务ID。|
    |taskStatus|String|success|投递任务状态，为running、success和fail中的一种。|
    |taskMessage|String|无|投递任务失败时的具体错误信息。|
    |taskCreateTime|Integer|1448925013|投递任务创建时间。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|
    |taskLastDataReceiveTime|Integer|1448915013|投递任务中的最近一条日志到达服务端时间（非日志时间，是服务端接收时间）。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|
    |taskFinishTime|Integer|1448926013|投递任务结束时间。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|


## 示例

-   请求示例

    ```
    GET /logstores/test-logstore/shipper/test-shipper/tasks?from=1448748198&to=1448948198&status=success&offset=0&size=100 HTTP/1.1
    Header:
    {
        "x-log-apiversion" : 0.6.0, 
        "Authorization" : "LOG yourAccessKeyId:yourSignature", 
        "Host" : "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com", 
        "Date" : "Wed, 11 Nov 2015 08:28:19 GMT", 
        "Content-Length" : 55, 
        "x-log-signaturemethod" : "hmac-sha1", 
        "Content-MD5" : "757C60FC41CC7D3F60B88E0D916D051E", 
        "User-Agent" : "sls-java-sdk-v-0.6.0", 
        "Content-Type" : "application/json"
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header:
    {
        "Date" : "Wed, 11 Nov 2015 08:28:20 GMT", 
        "Content-Length" : 0, 
        "x-log-requestid" : "5642FC2399248C8F7B0145FD", 
        "Connection" : "close", 
        "Server" : "nginx/1.6.1"
    }
    Body:
    {
        "count" : 10,
        "total" : 20,
        "statistics" : {
            "running" : 0,
            "success" : 20,
            "fail" : 0 
        }
        "tasks" : [
            {
                "id" : "abcdefghijk",
                "taskStatus" : "success",
                "taskMessage" : "",
                "taskCreateTime" : 1448925013,
                "taskLastDataReceiveTime" : 1448915013,
                "taskFinishTime" : 1448926013
            }
        ]
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|Project ProjectName does not exist.|Project不存在。|
|404|LogStoreNotExist|Logstore logstoreName does not exist.|Logstore不存在。|
|400|ShipperNotExist|Shipper shipperName does not exist.|Shipper不存在。|
|500|InternalServerError|Internal server error.|内部服务调用错误。|
|400|ParameterInvalid|Start time must be earlier than end time.|开始时间必须早于结束时间。|
|400|ParameterInvalid|Only support query last 48 hours task status.|只支持查询最近48小时的任务状态。|
|400|ParameterInvalid|Status only contains success/running/fail.|状态只包含success、running和fail。|

更多错误码，请参见[通用错误码](/intl.zh-CN/开发指南/API 参考/通用错误码.md)。

