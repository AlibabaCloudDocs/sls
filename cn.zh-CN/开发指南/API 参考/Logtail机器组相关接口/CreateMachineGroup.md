# CreateMachineGroup

调用CreateMachineGroup接口创建一个机器组。

## 请求语法

```
POST /machinegroups HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
Content-Type: application/json
Content-Length: Content Length
Content-MD5: Content MD5
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
{
    "groupName" : "testgroup",
    "groupType" : "",
    "groupAttribute" : {
        "externalName" : "testgroup",
        "groupTopic": "testgrouptopic"
    },
    "machineIdentifyType" : "ip",
    "machineList" : [
        "192.168.2.1",
        "192.168.2.2"
    ]
}
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    CreateMachineGroup接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-test-project|Project名称。|
    |groupName|String|是|test-machine-group|机器组名称。|
    |machineIdentifyType|String|是|ip|机器标识类型。    -   ip：IP地址机器组。
    -   userdefined：用户自定义标识机器组。 |
    |groupAttribute|Json|是|不涉及|机器组的属性。详细请参考下表groupAttribute参数说明。|
    |machineList|Array|是|\[ "192.168.2.1", "192.168.2.2" \]|机器组的标识信息。    -   如果machineIdentifyType配置为ip，则此处填写服务器的IP地址。
    -   如果machineIdentifyType配置为userdefined，则此处填写自定义的标识。 |
    |groupType|String|否|Armory|机器组类型，可选值为空或者Armory。|

    groupAttribute参数说明如下表所示：

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |groupTopic|String|否|testtopic|机器组的日志主题。|
    |externalName|String|否|testgroup|机器组所依赖的外部管理系统（Armory）标识。|


## 返回数据

-   响应头

    CreateMachineGroup接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功。该接口调用成功后无任何响应元素。


## 示例

-   请求示例

    ```
    POST /machinegroups HTTP/1.1
    Header :
    {
        "x-log-apiversion": "0.6.0",
        "Authorization": "LOG yourAccessKeyId:yourSignature",
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
        "Date": "Tue, 10 Nov 2015 17:57:33 GMT",
        "Content-Length": "187",
        "x-log-signaturemethod": "hmac-sha1",
        "Content-MD5": "82033D507DEAAD72067BB58DFDCB590D",
        "User-Agent": "sls-java-sdk-v-0.6.0",
        "Content-Type": "application/json",
        "x-log-bodyrawsize": "0"
    }
    Body :
    {
        "groupName": "test-machine-group",
        "groupType": "",
        "machineIdentifyType": "ip",
        "groupAttribute":     {
            "groupTopic": "testtopic",
            "externalName": "testgroup"
        },
        "machineList":     [
            "192.168.2.1",
            "192.168.2.2"
        ]
    }
    ```

-   返回示例

    ```
    HTTP/1.1 200 OK
    Header :
    {
        "Date": "Tue, 10 Nov 2015 17:57:33 GMT",
        "Content-Length": "0",
        "x-log-requestid": "5642300D99248CB76D005D36",
        "Connection": "close",
        "Server": "nginx/1.6.1"
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|Project projectName does not exist.|Project不存在。|
|400|MachineGroupAlreadyExist|group groupName already exists.|机器组已存在。|
|400|InvalidParameter|invalid group resource json|无效机器组参数。|
|500|InternalServerError|Internal server error|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

