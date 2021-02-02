# ListMachines

调用ListMachines接口列出目标机器组中与日志服务连接正常的机器列表。

## 请求语法

```
GET /machinegroups/groupName/machines HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    ListMachines接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/intl.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |groupName|String|是|test-machine-group-5|机器组名称。|
    |offset|Integer|否|0|查询开始行。默认值为0。|
    |size|Integer|否|3|分页查询时，设置的每页行数。最大值为500。|


## 返回数据

-   响应头

    ListMachines接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/intl.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，表示请求成功。响应Body中包括列表信息，如下所示：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |count|Integer|3|当前页返回的机器数目。|
    |total|Integer|8|机器总数。|
    |machines|Json array|\[\{"ip": "10.\*\*\*.\*\*\*.\*\*\*", "machine-uniqueid": "", "userdefined-id": "", "lastHeartbeatTime": 1447182247\},\{"ip": "10.\*\*\*.\*\*\*.\*\*\*", "machine-uniqueid": "","userdefined-id": "","lastHeartbeatTime": 1447182246\}\]|返回的机器信息列表。详细解释请参考下表machines说明。|

    machines中每个元素，具体如下：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |ip|String|10.\*\*\*.\*\*\*.\*\*\*|机器的IP地址。|
    |machine-uniqueid|String|无|机器的唯一标识。|
    |userdefined-id|String|无|机器的用户自定义标识。|
    |lastHeartbeatTime|Integer|1447182247|最后一次心跳时间。Unix时间戳格式，表示从1970-1-1 00:00:00 UTC计算起的秒数。|


## 示例

-   请求示例

    ```
    GET /machinegroups/test-machine-group-5/machines?offset=0&size=3 HTTP/1.1
    Header :
    {
        "x-log-apiversion": "0.6.0",
        "Authorization": "LOG yourAccessKeyId:yourSignature",
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
        "Date": "Tue, 10 Nov 2015 19:04:57 GMT",
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
        "Date": "Tue, 10 Nov 2015 19:04:58 GMT",
        "Content-Length": "324",
        "x-log-requestid": "56423FD999248C827B000A57",
        "Connection": "close",
        "Content-Type": "application/json",
        "Server": "nginx/1.6.1"
    }
    Body :
    {
        "machines":     [
            {
                "ip": "10.***.***.***",
                "machine-uniqueid": "",
                "userdefined-id": "",
                "lastHeartbeatTime": 1447182247
            },
            {
                "ip": "10.***.***.***",
                "machine-uniqueid": "",
                "userdefined-id": "",
                "lastHeartbeatTime": 1447182246
            },
            {
                "ip": "10.***.***.***",
                "machine-uniqueid": "",
                "userdefined-id": "",
                "lastHeartbeatTime": 1447182248
            }
        ],
        "count": 3,
        "total": 8
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|Project ProjectName does not exist.|Project不存在。|
|404|GroupNotExist|group groupName does not exist.|机器组不存在。|
|500|InternalServerError|internal server error.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/intl.zh-CN/开发指南/API 参考/通用错误码.md)。

