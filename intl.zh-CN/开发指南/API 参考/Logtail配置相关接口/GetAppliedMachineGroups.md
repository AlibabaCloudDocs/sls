# GetAppliedMachineGroups

调用GetAppliedMachineGroups接口获取已绑定指定Logtail配置的机器组列表。

## 请求语法

```
GET /configs/configName/machinegroups HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
Date: GMT Date
Host: Projectname.Endpoint              
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    GetAppliedMachineGroups接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/intl.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-test-project|Project名称。|
    |configName|String|是|logtail-config-sample|Logtail配置名称。|


## 返回数据

-   响应头

    GetAppliedMachineGroups接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/intl.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，表示请求成功。请求成功后，其响应Body中包含机器组信息，如下所示：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |count|Integer|1|返回的机器组数量。|
    |machinegroups|Array|\[ "sample-group1","sample-group2" \]|返回的机器组名称列表。|


## 示例

-   请求示例

    ```
    GET /configs/logtail-config-sample/machinegroups
    Header:
    {
        "Content-Length": 0, 
        "x-log-signaturemethod": "hmac-sha1", 
        "x-log-bodyrawsize": 0, 
        "User-Agent": "log-python-sdk-v-0.6.0", 
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",   
        "Date": "Mon, 09 Nov 2015 09:51:38 GMT", 
        "x-log-apiversion": "0.6.0", 
        "Authorization": "LOG yourAccessKeyId:yourSignature"
    }
    ```

-   返回示例

    ```
    HTTP/1.1 200 OK
    Header : 
    {
        "content-length": "44", 
        "server": "nginx/1.6.1", 
        "connection": "close", 
        "date": "Mon, 09 Nov 2015 09:51:38 GMT", 
        "content-type": "application/json", 
        "x-log-requestid": "56406CAA99248CAA230BE828"
    }
    Body:
    {
        "count": 1, 
        "machinegroups": 
        [
            "sample-group1",
            "sample-group2"
        ]
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|The Project does not exist : projectName|Project不存在。|
|404|ConfigNotExist|Config confiName does not exist.|Logtail配置不存在。|
|500|InternalServerError|internal server error.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/intl.zh-CN/开发指南/API 参考/通用错误码.md)。

