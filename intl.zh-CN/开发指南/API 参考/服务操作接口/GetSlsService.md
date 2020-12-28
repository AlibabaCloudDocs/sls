# GetSlsService

调用GetSlsService接口获取日志服务的开通状态。

## 请求语法

```
GET https://sls.aliyuncs.com/
?AccessKeyId=yourAccessKeyId
&Action=GetSlsService
&Format=Format
&SignatureMethod=HMAC-SHA1
&SignatureNonce=SignatureNonce
&SignatureVersion=SignatureVersion
&Timestamp=Timestamp
&Version=Version
&Signature=Signature
```

## 请求参数

-   请求头

    GetSlsService接口无特有请求头。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |Action|String|是|GetSlsService|API名称。该接口中固定配置为GetSlsService。|
    |AccessKeyId|String|是|test-key|阿里云账号的AccessKey ID。|
    |Signature|String|是|xxxxxx|您的签名，详情请参见[签名机制](/intl.zh-CN/开发指南/API 参考/服务操作接口/OpenSlsService.md)。|
    |SignatureMethod|String|是|HMAC-SHA1|签名方式。|
    |SignatureVersion|String|是|1.0|签名算法版本。|
    |SignatureNonce|String|是|dt712rl9d|签名唯一的随机数。用于防止网络重放攻击，建议您每一次请求都使用不同的随机数。|
    |Timestamp|String|是|2018-01-01T12:00:00Z|请求的时间戳。根据[ISO8601](http://iso.org/iso-8601-date-and-time-format.html)标准表示时间（UTC格式），格式为yyyy-MM-ddTHH:mm:ssZ。例如2018-01-01T12:00:00Z表示北京时间2018年01月01日20点00分00秒。|
    |Version|String|是|2019-10-23|API版本号，格式为YYYY-MM-DD。|
    |Format|String|否|JSON|返回参数的语言类型。可选值：JSON、XML。默认值：XML。|


## 返回数据

-   响应头

    GetSlsService接口无特有响应头。

-   响应元素

    返回HTTP状态码200，则表示请求成功。

    |参数名称|数据类型|示例值|描述|
    |----|----|---|--|
    |RequestId|String|5ACE2FA0-1C36-412A-BA9F-3B0E17A146EA|当次请求的Request ID。|
    |Success|Boolean|true|是否请求成功。    -   true：请求成功。
    -   false：请求失败。 |
    |Code|String|200|返回状态码。|
    |Message|String|successful|返回响应描述。|
    |Enabled|Boolean|true|是否开通服务。    -   true：已开通服务。
    -   false：未开通服务。 |


## 示例

-   请求示例

    ```
    https://sls.aliyuncs.com/
    ?AccessKeyId=test-key
    &Action=GetSlsService
    &Format=JSON
    &SignatureMethod=HMAC-SHA1
    &SignatureNonce=222856
    &SignatureVersion=1.0
    &Timestamp=2020-09-15T13%3A01%3A26Z
    &Version=2019-10-23
    &Signature=xxxxxxx
    ```

-   正常返回示例

    ```
    {
      "RequestId": "5ACE2FA0-1C36-412A-BA9F-3B0E17A146EA",
      "Message": "successful",
      "Enabled": true,
      "Code": "200",
      "Success": true
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|400|PermissionDenied|No permission to get SLS service status.|没有查询日志服务开通状态的权限。|
|500|DescribeUserBusinessStatusFailed|Failed to describe user business status.|查询用户业务状态失败。|

更多错误码，请参见[通用错误码](/intl.zh-CN/开发指南/API 参考/通用错误码.md)。

