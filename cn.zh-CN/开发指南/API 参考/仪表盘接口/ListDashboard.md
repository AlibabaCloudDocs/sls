# ListDashboard

调用ListDashboard接口查询指定Project下的仪表盘（Dashboard）简要信息，包括仪表盘ID和仪表盘名称。支持查询指定Project下所有仪表盘列表，也支持查询指定Project下具体仪表盘信息。

## 接口说明

仪表盘ID和名称在控制台如下所示。

![pic](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7353049061/p208421.png)

|序号|描述|
|--|--|
|①|仪表盘ID|
|②|仪表盘名称|

## 请求语法

```
GET /dashboards HTTP/1.1
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

    ListDashboard接口无特有请求头，关于Log Service API的公共请求头请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-test-project|Project名称。|
    |dashboardName|String|否|dashboard-1609294922657-434834|仪表盘ID。同一个Project下，仪表盘ID唯一，不可重复。支持模糊查询，例如输入da，会查询出所有以da开头的仪表盘。|
    |displayName|String|否|data-ingest|仪表盘显示名称。|
    |offset|Integer|否|0|查询开始行。默认值为0。|
    |sizes|Integer|否|10|分页查询时，设置的每页行数。最大值为500。|


## 返回数据

-   响应头

    ListDashboard接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功。响应Body中包括仪表盘简要信息，具体格式如下：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |count|Integer|1|当前页返回的仪表盘数量。|
    |total|Integer|1|符合查询条件的仪表盘总数。|
    |dashboards|Array|\[\{"dashboardName":"dashboard-1609294922657-434834","displayName":"data-ingest"\}\]|返回的仪表盘简要信息列表。|


## 示例

-   请求示例

    ```
    GET /dashboards HTTP/1.1
    Header:
    {
        "Content-Length": "0",
        "x-log-bodyrawsize": "0",
        "x-log-apiversion": "0.6.0",
        "x-log-signaturemethod": "hmac-sha1",
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
        "Date": "Mon, 30 Nov 2020 06:22:17 GMT",
        "Authorization": "LOG yourAccessKeyId:yourSignature",
        "x-log-date": "Mon, 30 Nov 2020 06:22:17 GMT"
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header :
    {
        "content-length": "85" 
        "server": "nginx/1.6.1"
        "connection": "close"
        "date": "Mon, 09 Nov 2015 09:19:13 GMT"
        "content-type": "application/json"
        "x-log-requestid": "5640651199248CAA2300C2BA"
    }
    Body:
    {
        "count": 1, 
        "total": 1,
        "dashboards": :[{"dashboardName":"dashboard-1609294922657-434834","displayName":"data-ingest"}]
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|The Project does not exist : projectName|Project不存在。|
|400|ParameterInvalid|offset: offset is invalid.|offset参数取值无效。|
|400|ParameterInvalid|sizes: sizes is invalid.|sizes参数取值无效。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

