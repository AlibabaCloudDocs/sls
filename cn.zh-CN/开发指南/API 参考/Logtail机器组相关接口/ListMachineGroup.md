# ListMachineGroup

调用ListMachineGroup接口列出目标Project下的机器组。

## 请求语法

```
GET /machinegroups?offset=1&size=100 HTTP/1.1
Authorization: AuthorizationString
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    ListMachineGroup接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |offset|Integer|否|1|查询开始行。默认值为0。|
    |size|Integer|否|10|分页查询时，设置的每页行数。最大值为500。|
    |groupName|String|否|test-machine-group|机器组名称。用于过滤机器组，支持部分匹配。|


## 返回数据

-   响应头

    ListMachineGroup接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，表示请求成功。请求成功后，其响应Body中包含目标Project下的机器组列表，如下所示：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |count|Integer|2|当前页返回的机器组数。|
    |total|Integer|2|符合查询条件的机器组总数。|
    |machinegroups|Json Array|\[ "test-machine-group-1", "test-machine-group-2" \]|符合查询条件的机器组列表。|


## 示例

-   请求示例

    ```
    GET /machinegroups?groupName=test-machine-group&offset=0&size=3 HTTP/1.1
    Header :
    {
        "x-log-apiversion": "0.6.0",
        "Authorization": "LOG yourAccessKeyId:yourSignature",
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
        "Date": "Tue, 10 Nov 2015 18:34:44 GMT",
        "Content-Length": "0",
        "x-log-signaturemethod": "hmac-sha1",
        "User-Agent": "sls-java-sdk-v-0.6.0",
        "Content-Type": "application/x-protobuf",
        "x-log-bodyrawsize": "0"
    }
    ```

-   返回示例

    ```
    HTTP/1.1 200 OK
    Header :
    {
        "Date": "Tue, 10 Nov 2015 18:34:44 GMT",
        "Content-Length": "83",
        "x-log-requestid": "564238C499248C8F7B0001DE",
        "Connection": "close",
        "Content-Type": "application/json",
        "Server": "nginx/1.6.1"
    }
    Body :
    {
        "machinegroups":     [
            "test-machine-group-1",
            "test-machine-group-2"
        ],
        "count": 2,
        "total": 2
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|500|InternalServerError|internal server error|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

