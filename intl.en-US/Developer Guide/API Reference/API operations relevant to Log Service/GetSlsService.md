# GetSlsService

Queries the activation status of Log Service.

## Request parameters

-   Request headers

    The GetSlsService operation does not have operation-specific request headers.

-   Parameters

    |Request parameter|Type|Required|Example|Description|
    |:----------------|:---|:-------|-------|:----------|
    |Action|String|Yes|GetSlsService|The operation that you want to perform. Set the value to GetSlsService.|
    |AccessKeyId|String|Yes|test-key|The AccessKey ID provided to you by Alibaba Cloud.|
    |Signature|String|Yes|xxxxxx|The signature string of the current request. For more information about how signatures are calculated, see [Signature method]().|
    |SignatureMethod|String|Yes|HMAC-SHA1|The encryption method of the signature string.|
    |SignatureVersion|String|Yes|1.0|The version of the signature encryption algorithm.|
    |SignatureNonce|String|Yes|dt712rl9d|A unique, random number used to prevent replay attacks. You must use different numbers for different requests.|
    |Timestamp|String|Yes|2018-01-01T12:00:00Z|The timestamp of the request. Specify the time in the [ISO 8601](http://iso.org/iso-8601-date-and-time-format.html) standard in the yyyy-MM-ddTHH:mm:ssZ format. The time must be in UTC. For example, 2018-01-01T12:00:00Z indicates 20:00:00 on January 1, 2018 \(UTC+8\).|
    |Version|String|Yes|2019-10-23|The version number of the API. The value must be in the YYYY-MM-DD format.|
    |Format|String|No|JSON|The format in which to return the response. Valid values: JSON and XML. Default value: XML.|


## Response elements

-   Response header

    The GetSlsService operation does not have operation-specific response headers.

-   Response element

    The HTTP status code 200 is returned.

    |Response parameter|Type|Description|
    |------------------|----|-----------|
    |RequestId|String|The ID of the request.|
    |Success|Boolean|Indicates whether the request is successful.|
    |Code|String|The error code.|
    |Message|String|The error description.|
    |Enabled|Boolean|Indicates whether Log Service is activated.|


## Examples

-   Sample request

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

-   Sample success response

    ```
    {
      "RequestId": "5ACE2FA0-1C36-412A-BA9F-3B0E17A146EA",
      "Message": "successful",
      "Enabled": true,
      "Code": "200",
      "Success": true
    }
    ```


## Error code

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|400|PermissionDenied|No permission to get SLS service status.|The error message returned because you are not authorized to query the activation status of Log Service.|
|500|DescribeUserBusinessStatusFailed|Failed to describe user business status.|The error message returned because the activation status of Log Service has failed to be queried.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

