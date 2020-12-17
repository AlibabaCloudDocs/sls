# GetMachineGroup

调用GetMachineGroup接口查看目标机器组的具体信息。

## 请求语法

```
GET /machinegroups/groupName HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    GetMachineGroup接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-test-project|Project名称。|
    |groupName|String|是|test-machine-group|机器组名称。|


## 返回数据

-   响应头

    GetMachineGroup接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功。请求成功后，其响应Body中包含目标机器组的具体信息，如下所示：

    |参数名称|数据类型|示例值|描述|
    |----|----|---|--|
    |groupName|String|test-machine-group|机器组名称。|
    |groupType|String|Armory|机器组类型，值为空或者Armory。|
    |machineIdentifyType|String|ip|机器组标识的类型。    -   ip：IP地址机器组。
    -   userdefined：用户自定义标识机器组。 |
    |groupAttribute|Json Object|\{"externalName": "testgroup", "groupTopic": "testtopic"\}|机器组的属性。详细请参考下表groupAttribute参数说明。|
    |machineList|Json Array|\[ "127.0.0.1", "127.0.0.2" \]|机器组的标识信息。    -   如果machineIdentifyType配置为ip，则此处填写服务器的IP地址。
    -   如果machineIdentifyType配置为userdefined，则此处填写自定义的标识。 |
    |createTime|integer|1447178253|机器组的创建时间。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|
    |lastModifyTime|integer|1447178253|最近一次更新机器组的时间。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|

    groupAttribute参数说明如下表所示：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |groupTopic|String|testtopic|机器组的日志主题。|
    |externalName|String|testgroup|机器组所依赖的外部管理系统（Armory）标识。|


## 示例

-   请求示例

    ```
    GET /machinegroups/test-machine-group HTTP/1.1
    Header :
    {
        "x-log-apiversion": "0.6.0",
        "Authorization": "LOG yourAccessKeyId:yourSignature",
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
        "Date": "Tue, 10 Nov 2015 18:15:24 GMT",
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
        "Date": "Tue, 10 Nov 2015 18:15:23 GMT",
        "Content-Length": "239",
        "x-log-requestid": "5642343B99248CB36D0060B8",
        "Connection": "close",
        "Content-Type": "application/json",
        "Server": "nginx/1.6.1"
    }
    Body :
    {
        "groupName": "test-machine-group",
        "groupType": "",
        "groupAttribute":     {
            "externalName": "testgroup",
            "groupTopic": "testtopic"
        },
        "machineIdentifyType": "ip",
        "machineList":     [
            "127.0.0.1",
            "127.0.0.2"
        ],
        "createTime": 1447178253,
        "lastModifyTime": 1447178253
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|Project projectName does not exist.|Project不存在。|
|404|GroupNotExist|group groupName does not exist.|机器组不存在。|
|500|InternalServerError|Internal server error|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

