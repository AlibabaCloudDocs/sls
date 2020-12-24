# UpdateExternalStore

调用UpdateExternalStore接口修改外部存储数据的配置信息。

## 接口说明

目前支持OSS数据源和VPC下的RDS MySQL数据库作为外部存储数据。本文以RDS MySQL为例，OSS数据源作为外部存储数据时其参数配置请参见[关联OSS数据源](/cn.zh-CN/查询与分析/关联外部数据源/关联OSS数据源.md)。

## 请求语法

```
PUT /externalstores/externalStoreName HTTP/1.1
x-log-bodyrawsize: 0
Content-Type: application/json
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
Host: ProjectName.Endpoint
Date: GMT Date 
Authorization: LOG yourAccessKeyId:yourSignature 
{"externalStoreName": "externalStoreName",
  "storeType": "rds-vpc", 
  "parameter": {
               "vpc-id": "vpc-id", 
               "instance-id": "instance-id", 
               "host": "host", 
               "port": "port", 
               "username": "username", 
               "password": "password", 
               "db": "db", 
               "table": "table", 
               "region": "region"
               }
}
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    UpdateExternalStore接口无特有请求头，关于Log Service API的公共请求头请参见[公共请求头](/cn.zh-CN/开发指南/API 参考/公共请求头.md)。

-   请求参数

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-test-project|Project名称。|
    |externalStoreName|String|是|rds\_store|外部存储名称。|
    |storeType|String|是|rds-vpc|存储类型。固定取值为rds-vpc，表示VPC下的RDS MySQL数据库。|
    |vpc-id|String|否|vpc-bp1aevy8sofi8mh1q\*\*\*\*|RDS MySQL实例所属的VPC ID。|
    |intance-id|String|否|i-bp1b6c719dfa08exf\*\*\*\*|RDS MySQL实例ID。|
    |host|String|否|192.168.XX.XX|RDS MySQL实例的内网地址或外网地址。|
    |port|String|是|3306|RDS MySQL实例的内网或者外网端口。|
    |username|String|是|root|RDS MySQL实例中的账号名称。|
    |password|String|是|sfdsfldsfksfl\*\*\*\*|RDS MySQL实例中账号对应的密码。|
    |db|String|是|meta|RDS MySQL实例的数据库名称。|
    |table|String|是|join\_meta|RDS MySQL实例的数据库表名称。|
    |region|String|是|cn-qingdao|RDS MySQL实例所在地域，目前仅支持cn-qingdao、cn-beijing、cn-hangzhou。|


## 返回数据

-   响应头

    UpdateExternalStore接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/cn.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功。该接口调用成功后无任何响应元素。


## 示例

-   请求示例

    ```
    PUT /externalstores/rds_store  HTTP/1.1
    Header :
    {
    x-log-bodyrawsize: 0
    Content-Type: application/json 
    Content-Length: 307 
    Content-MD5: 7C1D14659C0BBBA7C7BFF9E5A1A46705 
    x-log-apiversion: 0.6.0
    x-log-signaturemethod: hmac-sha1 
    Host: ali-test-project.cn-chengdu.log.aliyuncs.com 
    Date: Thu, 19 Apr 2018 02:15:41 GMT
    Authorization: LOG yourAccessKeyId:yourSignature
    }
    Body :
    {"externalStoreName": "rds_store", 
      "storeType": "rds-vpc", 
      "parameter": {
                   "vpc-id": "vpc-p1aevy8sofi8mh1q****", 
                   "instance-id": "i-bp1b6c719dfa08exf****", 
                   "host": "192.168.XX.XX", 
                   "port": "3306", 
                   "username": "root", 
                   "password": "sfdsfldsfksfl****", 
                   "db": "meta", 
                   "table": "join_meta", 
                   "region": "cn-qingdao"
                   }
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header
    {
    date: Mon, 09 Nov 2015 07:45:30 GMT
    connection: close
    x-log-requestid: 56404F1A99248CA26C002180
    content-length: 0
    server: nginx/1.6.1
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|The Project does not exist : projectName|Project不存在。|
|400|ParameterInvalid|The body is not valid json string.|外部存储配置中存在无效的参数。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/cn.zh-CN/开发指南/API 参考/通用错误码.md)。

