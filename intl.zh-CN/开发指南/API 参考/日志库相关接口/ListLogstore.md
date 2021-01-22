# ListLogstore

调用ListLogstore接口查询指定Project下的Logstore。支持查询指定Project下所有Logstore列表，也支持查询指定Project下具体Logstore。

## 请求语法

```
GET /logstores HTTP/1.1
Content-Length: 0
x-log-bodyrawsize': 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: ProjectName.Endpoint
Date: GMT Date
Authorization: LOG yourAccessKeyId:yourSignature
x-log-date: Mon, 30 Nov 2020 06:22:17 GMT         
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    ListLogstore接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/intl.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |ProjectName|String|是|test-project|Project名称。|
    |logstoreName|String|否|test-logstore|Logstore名称。支持模糊匹配，例如输入test，则返回名称包含test的Logsotre列表。|
    |offset|Integer|否|1|查询开始行。默认值为0。|
    |size|Integer|否|10|分页查询时，设置的每页行数。最大值为500。|


## 返回数据

-   响应头

    ListLogstore接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/intl.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，表示请求成功。响应Body中包括Logstore信息，如下所示：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |count|Integer|1|当前页返回的Logstore数目。|
    |total|Integer|1|符合查询条件的Logstore总数。|
    |logstores|Array|\["test-logstore"\]|返回的Logstore名称列表。|


## 示例

-   请求示例

    ```
    GET /logstores HTTP/1.1
    Header: 
    {
    'Content-Length': '0',
    'x-log-bodyrawsize': '0',
    'x-log-apiversion': '0.6.0',
    'x-log-signaturemethod': 'hmac-sha1',
    'Host': 'ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com',
    'Date': 'Mon, 30 Nov 2020 06:22:17 GMT',
    'Authorization': 'LOG yourAccessKeyId:yourSignature',
    'x-log-date': 'Mon, 30 Nov 2020 06:22:17 GMT'
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header: 
    {
    'Server': 'Tengine',
    'Content-Type': 'application/json',
    'Content-Length': '85',
    'Connection': 'close',
    'Access-Control-Allow-Origin': '*',
    'Date': 'Mon, 30 Nov 2020 06:23:03 GMT',
    'x-log-requestid': '5FC48FC72068AF63D11472FC'
    }
    Body:
    {
    'count':1,
    'total':1,
    'logstores':["test-logstore"]
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|Project ProjectName does not exist.|Project不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|
|400|ParameterInvalid|Invalid parameter size, \(0.6.0\].|无效参数。|
|400|InvalidLogStoreQuery|logstore Query is invalid.|无效查询。|

更多错误码，请参见[通用错误码](/intl.zh-CN/开发指南/API 参考/通用错误码.md)。

