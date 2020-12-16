# GetAppliedConfigs

调用GetAppliedConfigs接口获取目标机器组上已经被应用的Logtail配置列表。

## 请求语法

```
GET /machinegroups/groupName/configs HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    GetAppliedConfigs接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-test-project|Project名称。|
    |groupName|String|是|test-machine-group|机器组名称。|


## 返回数据

-   响应头

    GetAppliedConfigs接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功，其响应Body会包括指定机器组下的所有Logtail配置列表，具体格式如下：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |count|Integer|3|Logtail配置数量。|
    |configs|Array|\[ "two", "three", "test\_logstore" \]|Logtail配置名称列表。|


## 示例

-   请求示例

    ```
    GET /machinegroups/test-machine-group/configs HTTP/1.1
    Header :
    {
    x-log-apiversion: 0.6.0
    Authorization: LOG yourAccessKeyId:yourSignature
    Host: ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com
    Date: Tue, 10 Nov 2015 19:45:48 GMT
    Content-Length: 0
    x-log-signaturemethod: hmac-sha1
    User-Agent: sls-java-sdk-v-0.6.0
    Content-Type: application/x-protobuf
    x-log-bodyrawsize: 0
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header :
    {
    Date: Tue, 10 Nov 2015 19:45:48 GMT
    Content-Length: 53
    x-log-requestid: 5642496C99248C8C7B00173F
    Connection: close
    Content-Type: application/json
    Server: nginx/1.6.1
    }
    Body :
    {
        "configs":     [
            "two",
            "three",
            "test_logstore"
        ],
        "count": 3
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|The Project does not exist : projectName|Project不存在。|
|404|MachineGroupNotExist|MachineGroup groupName does not exist.|机器组不存在。|
|500|InternalServerError|Internal server error.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

