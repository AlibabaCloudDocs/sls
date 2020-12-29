# OpenSlsService

调用OpenSlsService接口开通日志服务。只有开通服务后，才能进行日志服务的操作。

## 请求语法

```
POST https://sls.aliyuncs.com/
?AccessKeyId=yourAccessKeyId
&Action=OpenSlsService
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

    OpenSlsService接口无特有请求头。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |Action|String|是|OpenSlsService|API名称，该接口中固定配置为OpenSlsService。|
    |AccessKeyId|String|是|test-key|阿里云账号的AccessKey ID。|
    |Signature|String|是|xxxxxx|您的签名，详情请参见[签名机制](#section_8f8_hbd_y87)。|
    |SignatureMethod|String|是|HMAC-SHA1|签名方式。|
    |SignatureVersion|String|是|1.0|签名算法版本。|
    |SignatureNonce|String|是|dt712rl9d|签名唯一的随机数。用于防止网络重放攻击，建议您每一次请求都使用不同的随机数。|
    |Timestamp|String|是|2018-01-01T12:00:00Z|请求的时间戳。根据[ISO8601](http://iso.org/iso-8601-date-and-time-format.html)标准表示时间（UTC格式），格式为yyyy-MM-ddTHH:mm:ssZ。例如2018-01-01T12:00:00Z表示北京时间2018年01月01日20点00分00秒。|
    |Version|String|是|2019-10-23|API版本号，格式为YYYY-MM-DD。|
    |Format|String|否|JSON|返回参数的语言类型。可选值：JSON、XML。默认值：XML。|


## 签名机制

1.  构造规范化请求字符串。

    1.  将参数按照字母序进行排序，不包含Signature参数。

    2.  使用UTF-8字符集按照[RFC3986](http://tools.ietf.org/html/rfc3986)规则编码参数。

        -   A~Z、a~z、0~9、短划线（-）、下划线（\_）、英文句点（.）、和波浪线（~）不编码。
        -   其它字符编码为`%XY`格式，其中`XY`是字符对应ASCII码的十六进制数。例如半角双引号（"）对应的编码为`%22`。
        -   使用等号（=）连接编码后的请求参数和参数取值。
        -   使用and（&）连接编码后的请求参数，该参数排序按照字母序进行排序，不包含Signature参数。
    3.  获取规范化的请求字符串（CanonicalizedQueryString）。

2.  构造签名字符串。

    1.  构造待签名的字符串StringToSign。

        ```
        StringToSign=
          HTTPMethod + "&" +                      //HTTPMethod表示发送请求的HTTP方法，例如GET。
          percentEncode("/") + "&" +              //percentEncode("/")表示正斜线（/）经过UTF-8编码获得的值，即%2F。
          percentEncode(CanonicalizedQueryString) //您在[步骤1](#step_bw2_s55_mxb)中获取的规范化请求字符串。
        ```

    2.  按照[RFC2104](http://www.ietf.org/rfc/rfc2104.txt)定义，计算待签名字符串StringToSign的HMAC-SHA1值。

        ```
        Signature = Base64( HMAC-SHA1( AccessKeySecret + "&", UTF-8-Encoding-Of(StringToSign) ) )
        ```

    3.  将通过[RFC3986](https://tools.ietf.org/html/rfc3986)规则编码后的参数Signature添加到规范化请求字符串URL中。


Python代码示例如下所示：

```
import base64
import datetime
import hmac
import random
from hashlib import sha1
from urllib import parse

import pytz

url = 'https://sls.aliyuncs.com/?'
accessKeyId = 'test-key'
accessKeySecret = 'test-secret'

params = {
    'AccessKeyId': accessKeyId,
    'Action': 'OpenSlsService',
    'Format': 'JSON',
    'Version': '2019-10-23',
    'SignatureMethod': 'HMAC-SHA1',
    'SignatureNonce': str(int(random.random() * 1000000)),
    'SignatureVersion': '1.0',
    'Timestamp': parse.quote(
        datetime.datetime.now(pytz.utc).strftime('%Y-%m-%dT%H:%M:%SZ')
    )
}


def create_url():
    kvs = sorted(params.items(), key=lambda x: x[0])
    paramStr = '&'.join([k + '=' + v for k, v in kvs])
    sigStr = 'GET&%2F&' + parse.quote(paramStr, safe='')
    hashed = hmac.new((accessKeySecret + '&').encode(), sigStr.encode(), sha1)
    signature = base64.encodebytes(
        hashed.digest()).decode('utf-8').rstrip('\n')
    return url + paramStr + '&Signature=' + signature

print(create_url())
```

## 返回数据

-   响应头

    OpenSlsService接口无特有响应头。

-   响应元素

    返回HTTP状态码200，则表示请求成功。

    |参数名称|数据类型|示例值|描述|
    |----|----|---|--|
    |RequestId|String|1CCC2B8E-4FF3-4755-A96C-8CE2E4BF27DF|当次请求的Request ID。|
    |Success|Boolean|true|是否请求成功。    -   true：请求成功。
    -   false：请求失败。 |
    |Code|String|200|返回状态码。|
    |Message|String|您已开通，请前往控制台使用。|返回响应描述。|


## 示例

-   请求示例

    ```
    https://sls.aliyuncs.com/
    ?AccessKeyId=test-key
    &Action=OpenSlsService
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
      "RequestId": "1CCC2B8E-4FF3-4755-A96C-8CE2E4BF27DF",
      "Message": "您已开通，请前往控制台使用。",
      "Code": "200",
      "Success": true
    }
    ```


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|:------|:--|:---|--|
|400|PermissionDenied|No permission to open SLS service.|没有开通日志服务的权限。|
|500|GetSpecificationsFailed|Failed to get specifications of commodity.|查询商品规格失败。|
|500|CreateOrderFailed|Failed to create an order.|订单创建失败。|

更多错误码，请参见[通用错误码](/intl.zh-CN/开发指南/API 参考/通用错误码.md)。

