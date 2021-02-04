# GetSlsService

Queries the activation status of Log Service.

## Request syntax

```
GET https://sls.aliyuncs.com/
? AccessKeyId=yourAccessKeyId
&Action=GetSlsService
&Format=Format
&SignatureMethod=HMAC-SHA1
&SignatureNonce=SignatureNonce
&SignatureVersion=SignatureVersion
&Timestamp=Timestamp
&Version=Version
&Signature=Signature
```

## Request parameters

-   Request headers

    The GetSlsService operation does not have operation-specific request headers.

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |Action|String|Yes|GetSlsService|The operation that you want to perform. Set the value to GetSlsService.|
    |AccessKeyId|String|Yes|test-key|The AccessKey ID of your Alibaba Cloud account.|
    |Signature|String|Yes|xxxxxx|The signature string of the current request. For more information about how signatures are calculated, see [Signature method](/intl.en-US/Developer Guide/API Reference/API operations relevant to Log Service/OpenSlsService.md).|
    |SignatureMethod|String|Yes|HMAC-SHA1|The encryption method of the signature string.|
    |SignatureVersion|String|Yes|1.0|The version of the signature algorithm.|
    |SignatureNonce|String|Yes|dt712rl9d|A unique, random number that is used to prevent replay attacks. You must use different numbers for different requests.|
    |Timestamp|String|Yes|2018-01-01T12:00:00Z|The timestamp of the request. Specify the time in the [ISO 8601](http://iso.org/iso-8601-date-and-time-format.html) standard in the yyyy-MM-ddTHH:mm:ssZ format. The time must be in UTC. For example, 2018-01-01T12:00:00Z indicates 20:00:00 on January 1, 2018 \(UTC+8\).|
    |Version|String|Yes|2019-10-23|The version number of the API. The value must be in the YYYY-MM-DD format.|
    |Format|String|No|JSON|The format in which to return the response. Valid values: JSON and XML. Default value: XML.|


## Response elements

-   Response headers

    The GetSlsService operation does not have operation-specific response headers.

-   Response elements

    If the HTTP status code 200 is returned, the request is successful.

    |Parameter|Type|Example|Description|
    |---------|----|-------|-----------|
    |RequestId|String|5ACE2FA0-1C36-412A-BA9F-3B0E17A146EA|The ID of the request.|
    |Success|Boolean|true|Indicates whether the request is successful. Valid values: true and false.    -   true: The request is successful.
    -   false: The request failed. |
    |Code|String|200|The returned status code.|
    |Message|String|successful|The description of the returned response.|
    |Enabled|Boolean|true|Indicates whether Log Service is activated. Valid values: true and false.    -   true: Log Service is activated.
    -   false: Log Service is not activated. |


## Examples

-   Sample requests

    ```
    https://sls.aliyuncs.com/
    ? AccessKeyId=test-key
    &Action=GetSlsService
    &Format=JSON
    &SignatureMethod=HMAC-SHA1
    &SignatureNonce=222856
    &SignatureVersion=1.0
    &Timestamp=2020-09-15T13%3A01%3A26Z
    &Version=2019-10-23
    &Signature=xxxxxxx
    ```

-   Sample success responses

    ```
    {
      "RequestId": "5ACE2FA0-1C36-412A-BA9F-3B0E17A146EA",
      "Message": "successful",
      "Enabled": true,
      "Code": "200",
      "Success": true
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|400|PermissionDenied|No permission to get SLS service status.|The error message returned because you are not authorized to query the activation status of Log Service.|
|500|DescribeUserBusinessStatusFailed|Failed to describe user business status.|The error message returned because the activation status of Log Service has failed to be queried.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

