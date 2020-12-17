# UpdateMachineGroup

调用UpdateMachineGroup接口修改机器组配置信息。

## 请求语法

```
PUT /machinegroups/groupName HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
Content-Type:application/json
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
{
    "groupName": "test-machine-group",
    "groupType" : "",
    "groupAttribute" : {
        "externalName" : "testgroup",
        "groupTopic": "testgrouptopic"
    },
    "machineIdentifyType" : "ip",
    "machineList" : [
        "192.168.3.1",
        "192.168.3.2"
    ]
}
```

## 请求参数

-   请求头

    UpdateMachineGroup接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |----|----|----|---|--|
    |projectName|String|是|ali-test-project|Project名称。|
    |groupName|String|是|test-machine-group|机器组名称。|
    |groupType|String|否|无|机器组类型，默认为空。|
    |machineIdentifyType|String|是|userdefined|机器组标识类型。    -   ip：IP地址机器组。
    -   userdefined：用户自定义标识机器组。 |
    |groupAttribute|Json|是|\{"externalName" : "testgroup", "groupTopic": "testgrouptopic"\}|机器组的属性，默认为空。详细请参考下表groupAttribute参数说明。|
    |machineList|Array|是|\[uu\_id\_1，uu\_id\_2\]|机器组的标识信息。    -   如果machineIdentifyType配置为ip，则此处填写服务器的IP地址。
    -   如果machineIdentifyType配置为userdefined，则此处填写自定义的标识。 |

    groupAttribute参数说明如下表所示：

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |groupTopic|String|否|testtopic2|机器组的日志主题。默认为空。|
    |externalName|String|否|testgroup2|机器组所依赖的外部管理系统（Armory）标识。默认为空。|


## 返回数据

-   响应头

    UpdateMachineGroup接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功。该接口调用成功后无任何响应元素。


## 示例

-   请求示例

    ```
    PUT /machinegroups/test-machine-group HTTP/1.1
    Header :
    {
        "x-log-apiversion": "0.6.0",
        "Authorization": "LOG yourAccessKeyId:yourSignature",
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
        "Date": "Tue, 10 Nov 2015 18:41:43 GMT",
        "Content-Length": "194",
        "x-log-signaturemethod": "hmac-sha1",
        "Content-MD5": "2CEBAEBE53C078891527CB70A855BAF4",
        "User-Agent": "sls-java-sdk-v-0.6.0",
        "Content-Type": "application/json",
        "x-log-bodyrawsize": "0"
    }
    Body :
    {
        "groupName": "test-machine-group",
        "groupType": "",
        "machineIdentifyType": "userdefined",
        "groupAttribute":     {
            "groupTopic": "testtopic2",
            "externalName": "testgroup2"
        },
        "machineList":     [
            "uu_id_1",
            "uu_id_2"
        ]
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header :
    {
        "Date": "Tue, 10 Nov 2015 18:41:43 GMT",
        "Content-Length": "0",
        "x-log-requestid": "56423A6799248CA57B00035C",
        "Connection": "close",
        "Server": "nginx/1.6.1"
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|Project projectName does not exist.|Project不存在。|
|404|GroupNotExist|group groupName does not exist.|机器组不存在。|
|400|InvalidParameter|invalid group resource json|无效机器组参数。|
|500|InternalServerError|Internal server error|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

