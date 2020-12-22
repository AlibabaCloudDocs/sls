# GetIndex

调用GetIndex接口查询指定Logstore的索引。

## 请求语法

```
GET /logstores/logstoreName/index HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
Host: ProjectName.Endpoint
x-log-signaturemethod: hmac-sha1
Date: GMT Date
Content-Type: application/x-protobuf
Connection: Keep-Alive
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    GetIndex接口无特有请求头，关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|my-project|Project名称。|
    |logstoreName|String|是|logstore-4|Logstore名称。|


## 返回数据

-   响应头

    GetIndex接口无特有响应头，关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    GetIndex请求成功，其响应Body会包括指定Project和Logstore的索引，具体如下：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |index\_mode|String|v2|索引类型。|
    |keys|Object|无|字段索引配置。key为字段名称，value为索引配置。|
    |line|Object|无|全文索引配置。|
    |storage|String|pg|存储类型，目前固定取值为pg。|
    |ttl|Integer|30|索引文件生命周期，支持7天、30天、90天。|
    |lastModifyTime|Integer|1524155379|索引最后更新时间。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|

    -   全文索引配置包含如下信息：

|参数名称|数据类型|示例值|描述|
|:---|:---|---|:-|
|caseSensitive|Boolean|false|是否大小写敏感。-   true：大小写敏感。
-   false：大小写不敏感。 |
|chn|Boolean|false|是否包含中文。-   true：包含中文。
-   false：不包含中文。 |
|token|Array|\["\\n","\\t","\\r"\]|分词符列表。|
|include\_keys|Array|无|包含的字段列表。|
|exclude\_keys|Array|无|排除的字段列表。|

    -   字段索引配置包含如下信息：

|属性名称|类型|示例值|描述|
|:---|:-|---|:-|
|type|String|text|字段类型。|
|alias|String|无|字段别名。|
|chn|Boolean|false|是否包含中文。仅当type参数取值为text时，必须设置。-   true：包含中文。
-   false：不包含中文。 |
|token|Array|\["\\n","\\t","\\r"\]|分词符列表。仅当type参数取值为text时，必须设置。|
|caseSensitive|Boolean|false|是否大小写敏感。仅当type参数取值为text时，必须设置。-   true：大小写敏感。
-   false：大小写不敏感。 |
|doc\_value|Boolean|true|是否开启字段统计。-   true：开启统计。
-   false：不开启统计。 |


## 示例

-   请求示例

    ```
    GET /logstores/logstore-4/index HTTP/1.1
    Header :
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: my-project.cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Sun, 06 May 2018 13:08:42 GMT
    Content-Type: application/x-protobuf
    Connection: Keep-Alive
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header :
    {
    Server: nginx/1.12.1
    Content-Type: application/json
    Content-Length: 712
    Connection: close
    Access-Control-Allow-Origin: *
    Date: Sun, 06 May 2018 13:08:42 GMT
    x-log-requestid: 5AEEFE5A8B8AEB5E6C82B395
    }
    Body :
    {
      "index_mode": "v2",
      "keys": {
        "agent": {
          "alias": "",
          "caseSensitive": false,
          "chn": false,
          "doc_value": true,
          "token": [
            ",",
            " ",
            "'",
            "\"",
            ";",
            "=",
            "(",
            ")",
            "[",
            "]",
            "{",
            "}",
            "?",
            "@",
            "&",
            "<",
            ">",
            "/",
            ":",
            "\n",
            "\t",
            "\r"
          ],
          "type": "text"
        },
        "bytes": {
          "alias": "",
          "doc_value": true,
          "type": "long"
        },
        "remote_ip": {
          "alias": "",
          "caseSensitive": false,
          "chn": false,
          "doc_value": true,
          "token": [
            ",",
            " ",
            "'",
            "\"",
            ";",
            "=",
            "(",
            ")",
            "[",
            "]",
            "{",
            "}",
            "?",
            "@",
            "&",
            "<",
            ">",
            "/",
            ":",
            "\n",
            "\t",
            "\r"
          ],
          "type": "text"
        },
        "response": {
          "alias": "",
          "doc_value": true,
          "type": "long"
        }
      },
      "line": {
        "caseSensitive": false,
        "chn": false,
        "token": [
          ",",
          " ",
          "'",
          "\"",
          ";",
          "=",
          "(",
          ")",
          "[",
          "]",
          "{",
          "}",
          "?",
          "@",
          "&",
          "<",
          ">",
          "/",
          ":",
          "\n",
          "\t",
          "\r"
        ]
      },
      "storage": "pg",
      "ttl": 30,
      "lastModifyTime": 1524155379
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|400|IndexConfigNotExist|index config doesn’t exist.|查询的索引不存在。|
|404|ProjectNotExist|The Project does not exist : projectName.|Project不存在。|
|404|LogStoreNotExist|logstore logstoreName dose not exist.|Logstore不存在。|
|500|InternalServerError|Specified Server Error Message.|服务器错误信息。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

