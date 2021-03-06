# CreateIndex

调用CreateIndex接口为指定Logstore创建索引。

## 请求语法

```
POST /logstores/logstoreName/index HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
Host: ProjectName.Endpoint
x-log-signaturemethod: hmac-sha1
Date: GMT Date
{
  "line": line,
  "keys": keys
}
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    CreateIndex接口无特有请求头，关于Log Service API的公共请求头，请参见[公共请求头](/intl.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|my-project|Project名称。|
    |logstoreName|String|是|my-logstore|Logstore的名称。|
    |keys|Object|否|无|字段索引配置，key为字段名称，value为字段索引配置。该参数和line必须至少指定一个，更多示例，请参见[示例](#section_sdw_lwz_c2b)。|
    |line|Object|否|无|全文索引配置。该参数和keys必须至少指定一个，更多示例，请参见[示例](#section_sdw_lwz_c2b)。|

    -   全文索引配置包含如下信息：

|参数名称|数据类型|是否必填|示例值|描述|
|:---|:---|:---|---|:-|
|caseSensitive|Boolean|否|true|是否大小写敏感。-   true：大小写敏感。
-   false：大小写不敏感。 |
|chn|Boolean|否|true|是否包含中文。-   true：包含中文。
-   false：不包含中文。 |
|token|Array|是|\["\\n","\\t","\\r"\]|分词符列表。可以设置一个分词参数，指定这个字段按照哪一种方式分词。更多分词符，请参见[示例](#section_sdw_lwz_c2b)。|
|include\_keys|Array|否|无|包含的字段列表，不能与exclude\_keys同时指定。|
|exclude\_keys|Array|否|无|排除的字段列表，不能与include\_keys同时指定。|

    -   字段索引配置包含如下信息：

|参数名称|数据类型|是否必填|示例值|描述|
|:---|:---|:---|---|:-|
|type|String|是|index|字段类型。|
|alias|String|否|agent\_alias|字段别名。|
|chn|Boolean|否|true|是否包含中文。仅当type参数取值为text时，必须设置。-   true：包含中文。
-   false：不包含中文。 |
|token|Array|否|\["\\n","\\t","\\r"\]|分词符列表。仅当type参数取值为text时，必须设置。|
|caseSensitive|Boolean|否|true|是否大小写敏感。仅当type参数取值为text时，必须设置。-   true：大小写敏感。
-   false：大小写不敏感。 |
|doc\_value|Boolean|否|true|是否开启统计。-   true：开启统计。
-   false：不开启统计。 |


## 返回数据

-   响应头

    CreateIndex接口无特有响应头，关于Log Service API的公共响应头，请参见[公共响应头](/intl.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功。该接口调用成功后无任何响应元素。


## 示例

-   请求示例

    ```
    POST /logstores/my-logstore/index HTTP/1.1
    Header :
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: my-project.cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Mon, 07 May 2018 09:47:20 GMT
    Content-Type: application/json
    Content-MD5: 1860B805A6AA5288B97B32CF3B519112
    Content-Length: 316
    Connection: Keep-Alive
    }
    Body :
    {
      "line": {
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
      "keys": {
        "agent": {
          "doc_value": true,
          "caseSensitive": true,
          "alias": "agent_alias",
          "type": "text",
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
        }
      }
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200
    Server: nginx/1.12.1
    Content-Length: 0
    Connection: close
    Access-Control-Allow-Origin: *
    Date: Mon, 07 May 2018 09:47:21 GMT
    x-log-requestid: 5AF020A98CBAA2EF52AB66C9
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|400|IndexInfoInvalid|Required field token is lacking or of error format.|缺少必要的字段标记或格式配置错误。|
|400|IndexAlreadyExist|Logstore index is already created.|日志索引已经创建。|
|404|ProjectNotExist|The Project does not exist : projectName.|Project不存在。|
|404|LogStoreNotExist|Logstore logstoreName dose not exist.|Logstore不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/intl.zh-CN/开发指南/API 参考/通用错误码.md)。

