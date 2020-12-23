# GetExternalStore

调用GetExternalStore接口获取指定外部存储数据的详细信息。

## 接口说明

目前支持OSS数据源和VPC下的RDS MySQL数据库作为外部存储数据。本文以RDS MySQL为例，OSS数据源作为外部存储数据时其参数配置请参见[关联OSS数据源](/intl.zh-CN/查询与分析/关联外部数据源/关联OSS数据源.md)。

## 请求语法

```
GET /externalstores/externalStoreName 
Content-Length: 0 
x-log-bodyrawsize: 0 
x-log-apiversion: 0.6.0 
x-log-signaturemethod: hmac-sha1
Host: ProjectName.Endpoint
Date: GMT Date
Authorization: LOG yourAccessKeyId:yourSignature
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    GetExternalStore接口无特有请求头，关于Log Service API的公共请求头请参见[公共请求头](/intl.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |projectName|String|是|ali-test-project|Project名称。|
    |externalStoreName|String|是|rds\_store|外部存储名称。|


## 返回数据

-   响应头

    GetExternalStore接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/intl.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功。响应Body中包含该Project下指定外部存储详细信息，具体如下：

    |参数名称|数据类型|示例值|描述|
    |:---|:---|---|:-|
    |storeType|String|rds-vpc|存储类型。固定取值为rds-vpc，表示VPC下的RDS MySQL数据库。|
    |vpc-id|String|vpc-bp1aevy8sofi8mh1q\*\*\*\*|RDS MySQL实例所属的VPC ID。|
    |instance-id|String|i-bp1b6c719dfa08exf\*\*\*\*|RDS MySQL的实例ID。|
    |host|String|192.\*\*\*.\*\*\*.\*\*\*|RDS MySQL实例的内网地址或外网地址。|
    |port|String|3306|RDS MySQL实例的内网或者外网端口。|
    |username|String|root|RDS MySQL实例中创建的账号名称。|
    |db|String|meta|RDS MySQL实例的数据库名称。|
    |table|String|join\_meta|RDS MySQL实例的数据库表名称。|
    |region|String|cn-qingdao|RDS MySQL实例所在地域，目前仅支持cn-qingdao、cn-beijing、cn-hangzhou。|


## 示例

-   请求示例

    ```
    GET /externalstores/rds_store
    Header :
    {
    Content-Length: 0 
    x-log-bodyrawsize: 0
    x-log-apiversion: 0.6.0
    x-log-signaturemethod: hmac-sha1
    Host: ali-test-project.cn-chengdu.log.aliyuncs.com 
    Date: Thu, 19 Apr 2018 03:26:49 GMT
    Authorization: LOG yourAccessKeyId:yourSignature
    }
    ```

-   正常返回示例

    ```
    HTTP/1.1 200 OK
    Header :
    {   
    content-length: 730
    server: nginx/1.6.1
    connection: close
    date: Mon, 09 Nov 2015 08:29:15 GMT
    content-type: application/json
    x-log-requestid: 5640595B99248CAA23004A59
    }
    Body :
    {
      'storeType': 'rds-vpc', 
      'parameter': {
                   'region': 'cn-qingdao', 
                   'vpc-id': 'vpc-p1aevy8sofi8mh1q****', 
                   'instance-id': 'i-bp1b6c719dfa08exf****', 
                   'host': '192.168.XX.XX', 
                   'port': '3306', 
                   'username': 'root', 
                   'db': 'meta', 
                   'table': 'join_meta'
                   }
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|404|ProjectNotExist|The Project does not exist : projectName|Project不存在。|
|404|LogStoreNotExist|Logstore externalStoreName does not exist|外部存储不存在。|
|500|InternalServerError|Specified Server Error Message.|内部服务调用错误。|

更多错误码，请参见[通用错误码](/intl.zh-CN/开发指南/API 参考/通用错误码.md)。

