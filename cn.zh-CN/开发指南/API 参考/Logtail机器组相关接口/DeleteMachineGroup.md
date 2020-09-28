# DeleteMachineGroup

调用DeleteMachineGroup接口删除机器组，如果机器组已应用Logtail采集配置，则删除机器组后，会解绑对应的Logtail配置。

## 请求语法

```
DELETE /machinegroups/{groupName} HTTP/1.1
Authorization: <AuthorizationString> 
Date: <GMT Date>
Host: <Project Endpoint>
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

## 请求参数

-   请求头

    DeleteMachineGroup接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|类型|是否必须|示例值|描述|
    |:---|:-|:---|---|:-|
    |groupName|string|是|test-machine-group-4|机器组名称。|


## 返回数据

-   响应头

    DeleteMachineGroup接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   相应元素

    HTTP状态码返回200。


## 示例

-   请求示例

    ```
    DELETE /machinegroups/test-machine-group-4 HTTP/1.1
    Header :
    {
        "x-log-apiversion": "0.6.0",
        "Authorization": "LOG <yourAccessKeyId>:<yourSignature>",
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
        "Date": "Tue, 10 Nov 2015 19:13:28 GMT",
        "Content-Length": "0",
        "x-log-signaturemethod": "hmac-sha1",
        "User-Agent": "sls-java-sdk-v-0.6.0",
        "Content-Type": "application/x-protobuf",
        "x-log-bodyrawsize": "0"
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header :
    {
        "Date": "Tue, 10 Nov 2015 19:13:28 GMT",
        "Content-Length": "0",
        "x-log-requestid": "564241D899248C827B000CFE",
        "Connection": "close",
        "Server": "nginx/1.6.1"
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|-------|---|----|--|
|404|GroupNotExist|group \{GroupName\} does not exist.|机器组不存在。|
|500|InternalServerError|internal server error.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

