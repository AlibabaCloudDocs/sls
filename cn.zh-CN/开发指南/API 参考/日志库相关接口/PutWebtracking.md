# PutWebtracking

调用PutWebTracking接口将多条日志合并进行采集。

## 接口说明

-   适用于在网页或者客户端采集日志的场景。
-   使用Web Tracking采集日志时，单个请求只能写入一条日志。更多信息，请参见[使用Web Tracking采集日志](/cn.zh-CN/数据采集/其他采集方式/使用Web Tracking采集日志.md)。
-   针对日志量较大的场景，可以调用PutWebTracking接口将多条日志合并为一次请求。
-   使用PutWebTracking接口写入日志时，需要先为Logstore打开Web Tracking开关。更多信息，请参见[使用Web Tracking采集日志](/cn.zh-CN/数据采集/其他采集方式/使用Web Tracking采集日志.md)。

## 请求语法

```
POST ProjectName.Endpoint/logstores/logstoreName/track HTTP/1.1

x-log-apiversion: 0.6.0
x-log-bodyrawsize: 1234
x-log-compresstype: lz4

{
  "__topic__": "topic",
  "__source__": "source",
  "__logs__": [
    {
      "key1": "value1",
      "key2": "value2"
    },
    {
      "key1": "value1",
      "key2": "value2"
    }
  ],
  "__tags__": {
    "tag1": "value1",
    "tag2": "value2"
  }
}
```

## 请求参数

-   请求头

    仅支持如下三个请求头，在调用PutWebTracking接口时前两个为必选，格式和含义请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

    -   `x-log-apiversion: 0.6.0`
    -   `x-log-bodyrawsize: 1234`
    -   `x-log-compresstype: lz4`
    如果发送的数据没有经过任何压缩，不需要指定x-log-compresstype。如果需要对数据压缩发送，当前仅支持lz4和Deflate算法，其分别对应的请求头为：`x-log-compresstype: lz4`或`x-log-compresstype: deflate`。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |----|----|----|---|--|
    |projectName|String|是|my-project|Project名称。|
    |logstoreName|String|是|logstore-test|Logstore名称。|
    |endpoint|String|是|cn-hangzhou.log.aliyuncs.com|日志服务的Endpoint，请参见[服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)。|
    |\_\_topic\_\_|String|否|topic|日志主题。|
    |\_\_source\_\_|String|否|source|日志来源。|
    |\_\_logs\_\_|List|是|\[\{"key1": value1", "key2": "value2"\},\{"key1": "value1","key2": "value2"\}\]|日志内容列表。每个元素为一个JSON对象，表示一条日志。 **说明：** WebTracking采集的日志时间为日志到达服务端的时间，每条日志中无需设置\_\_time\_\_字段，如果存在该字段，将被服务端使用日志到达的时间覆盖。 |
    |\_\_tags\_\_|Object|否|\{"tag1": "value1", "tag2": "value2" \}|日志标签。|


## 返回数据

-   响应头

    PutWebtracking接口无特有响应头。关于Log Service API的公共响应头，请参考[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功。该接口调用成功后无任何响应元素。


## 示例

-   请求示例

    ```
    POST my-project.cn-hangzhou.log.aliyuncs.com/logstores/logstore-test/track HTTP/1.1
    Header :
    {
    x-log-apiversion: 0.6.0
    x-log-bodyrawsize: 1234
    x-log-compresstype: lz4
    }
    Body :
    {
      "__topic__": "topic",
      "__source__": "source",
      "__logs__": [
        {
          "foo": "bar",
          "foo1": "bar1"
        },
        {
          "bar": "foo",
          "bar1": "foo1"
        }
      ],
      "__tags__": {
        "tag1": "value1",
        "tag2": "value2"
      }
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header:
    {
        'Date': 'Wed, 20 May 2019 07:35:00 GMT', 
        'Content-Length': '0', 
        'x-log-requestid': '5642EFA499248C827B012B39', 
        'Connection': 'close', 
        'Server': 'nginx/1.6.1'
    }
    ```


## 错误码

|HTTP状态码|错误码|错误消息|描述|
|:------|:--|:---|:-|
|400|PostBodyInvalid|Protobuffer content cannot be parsed.|Protobuffer内容无效，无法解析。|
|400|InvalidTimestamp|Invalid timestamps are in logs.|日志内容中有无效的日志时间戳。|
|400|InvalidEncoding|Non-UTF8 characters are in logs.|日志内容中有非UTF-8字符。|
|400|InvalidKey|Invalid keys are in logs.|日志内容中有无效的key。|
|400|PostBodyTooLarge|Logs must be less than or equal to 3 MB and 4096 entries.|日志内容过大，包含的日志必须小于3MB和4096条。|
|400|PostBodyUncompressError|Failed to decompress logs.|日志内容解压失败。|
|499|PostBodyInvalid|The post data time is out of range.|日志中时间范围不在\[-7\*24Hour,+15Min\]有效范围内。|
|404|LogStoreNotExist|logstore logstoreName does not exist.|Logstore不存在。|
|404|ProjectNotExist|The Project does not exist : projectName.|Project不存在。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

