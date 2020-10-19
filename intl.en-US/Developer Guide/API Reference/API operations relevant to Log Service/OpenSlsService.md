# OpenSlsService

Activates Log Service. You must activate Log Service before you can use it to collect and manage logs.

## Request parameters

-   Request headers

    The OpenSlsService operation does not have operation-specific request headers.

-   Parameters

    |Request parameter|Type|Required|Example|Description|
    |:----------------|:---|:-------|-------|:----------|
    |Action|String|Yes|OpenSlsService|The operation that you want to perform. Set the value to OpenSlsService.|
    |AccessKeyId|String|Yes|test-key|The AccessKey ID provided to you by Alibaba Cloud.|
    |Signature|String|Yes|xxxxxx|The signature string of the current request. For more information about how signatures are calculated, see [Signature method](#section_8f8_hbd_y87).|
    |SignatureMethod|String|Yes|HMAC-SHA1|The encryption method of the signature string.|
    |SignatureVersion|String|Yes|1.0|The version of the signature encryption algorithm.|
    |SignatureNonce|String|Yes|dt712rl9d|A unique, random number used to prevent replay attacks. You must use different numbers for different requests.|
    |Timestamp|String|Yes|2018-01-01T12:00:00Z|The timestamp of the request. Specify the time in the [ISO 8601](http://iso.org/iso-8601-date-and-time-format.html) standard in the yyyy-MM-ddTHH:mm:ssZ format. The time must be in UTC. For example, 2018-01-01T12:00:00Z indicates 20:00:00 on January 1, 2018 \(UTC+8\).|
    |Version|String|Yes|2019-10-23|The version number of the API. The value must be in the YYYY-MM-DD format.|
    |Format|String|No|JSON|The format in which to return the response. Valid values: JSON and XML. Default value: XML.|


## Signature method

1.  Compose and encode a string-to-sign.

    1.  Create a canonicalized query string by arranging the request parameters \(including all common and operation-specific parameters except Signature\) in alphabetical order.

    2.  Encode the parameters in UTF-8. For more information, see [RFC 3986](http://tools.ietf.org/html/rfc3986).

        -   Uppercase letters, lowercase letters, digits, and some special characters such as hyphens \(-\), underscores \(\_\), periods \(.\), and tildes \(~\) do not need to be encoded.
        -   Other characters must be percent encoded in `%XY` format. `XY` represents the ASCII code of the characters in hexadecimal notation. For example, double quotation marks \("\) are encoded as `%22`.
        -   Use an equal sign \(=\) to concatenate each encoded request parameter and its value.
        -   Use an ampersand \(&\) to concatenate each encoded request parameter. The order of the parameter must be consistent with that in [Step 1.a](#step_bw2_s55_mxb).
    3.  Create a canonicalized query string.

2.  Create a string-to-sign.

    1.  Create StringToSign.

        ```
        StringToSign=
          HTTPMethod + "&" + //HTTPMethod: HTTP method used to send the request, such as GET.
          percentEncode("/") + "&" + //percentEncode("/"): Encode backslash (/) as %2F.
          percentEncode(CanonicalizedQueryString) //Encode the canonicalized query string created in [Step 1](#step_bw2_s55_mxb).
        ```

    2.  Calculate the HMAC value of the string-to-sign as defined in [RFC 2104](http://www.ietf.org/rfc/rfc2104.txt). Use the SHA1 algorithm to calculate the HMAC value of the StringToSign. The AccessKey secret is used as the key for the HMAC calculation.

        ```
        Signature = Base64( HMAC-SHA1( AccessKeySecret + "&", UTF-8-Encoding-Of(StringToSign) ) )
        ```

    3.  Encode the Signature parameter based on the [RFC 3986](https://tools.ietf.org/html/rfc3986) specification and add the encoded signature to the canonicalized query string.


The following sample code shows how to use Python to call the OpenSlsService operation:

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

## Response elements

-   Response header

    The OpenSlsService operation does not have operation-specific response headers.

-   Response element

    The HTTP status code 200 is returned.

    |Response parameter|Type|Description|
    |------------------|----|-----------|
    |RequestId|String|The ID of the request.|
    |Success|Boolean|Indicates whether the request is successful.|
    |Code|String|The error code.|
    |Message|String|The error description.|


## Examples

-   Sample request

    ```
    https://sls.aliyuncs.com/
    ? AccessKeyId=test-key
    &Action=OpenSlsService
    &Format=JSON
    &SignatureMethod=HMAC-SHA1
    &SignatureNonce=222856
    &SignatureVersion=1.0
    &Timestamp=2020-09-15T13%3A01%3A26Z
    &Version=2019-10-23
    &Signature=xxxxxxx
    ```

-   Sample success response

    ```
    {
      "RequestId": "1CCC2B8E-4FF3-4755-A96C-8CE2E4BF27DF",
      "Message": "You have activated Log Service. You can go to the Log Service console to manage logs.",
      "Code": "200",
      "Success": true
    }
    ```


## Error code

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|400|PermissionDenied|No permission to open SLS service.|The error message returned because you are not authorized to activate Log Service.|
|500|GetSpecificationsFailed|Failed to get specifications of commodity.|The error message returned because the information about Log Service has failed to be queried.|
|500|CreateOrderFailed|Failed to create an order.|The error message returned because the order has failed to be created.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

