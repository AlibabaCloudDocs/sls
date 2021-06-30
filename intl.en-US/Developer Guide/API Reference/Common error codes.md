# Common error codes

This topic describes the common error codes that are returned when API operations fail to be called.

If an error occurs when you send an API request to Log Service, Log Service returns an HTTP status code and an error message. The error message is included in the HTTP response body in the following format:

```
{
"errorCode" : <ErrorCode>,
"errorMessage" : <ErrorMessage>
}
```

The error messages returned by Log Service are applicable to most API operations. However, some error messages are specific to some API operations. The following table lists the common error codes that are returned when API operations fail to be called. Error codes that are specific to API operations are described in the corresponding API reference topic.

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|:----------|
|400|MissingContentType|Content-Type does not exist in http header when body is not empty.|The error message returned because the HTTP request body is not empty, but the Content-Type header field does not exist. Check the request header and make sure that the Content-Type field exists.|
|400|MissingBodyRawSize|x-log-bodyrawsize does not exist in header when it is necessary.|The error message returned because the required x-log-bodyrawsize field does not exist in the request header when the request fields are compressed. Check the request header and make sure that the x-log-bodyrawsize field exists.|
|400|InvalidBodyRawSize|x-log-bodyrawsize is invalid.|The error message returned because the value of the x-log-bodyrawsize header field is invalid. Check the request header and make sure that the value is a valid number.|
|400|InvalidCompressType|x-log-compresstype type is unsupported.|The error message returned because the compression type that is specified in the x-log-compresstype field is not supported. Check the request header and make sure that the value is lz4 or Deflate.|
|400|MissingHost|Host does not exist in http header.|The error message returned because the Host field does not exist in the request header. Check the request header and make sure that the host field exists.|
|400|MissingDate|Date does not exist in http header.|The error message returned because the Date field does not exist in the request header. Check the request header and make sure that the Date field exists.|
|400|InvalidDateFormat|Date date must follow RFC822.|The error message returned because the value of the Date field in the request header does not comply with RFC 822. Check the request header and make sure that the value of the Date field complies with RFC 822.|
|400|MissingAPIVersion|x-log-apiversion does not exist in http header.|The error message returned because the x-log-apiversion field does not exist in the request header. Check the request header and make sure that the x-log-apiversion field exists.|
|400|InvalidAPIVersion|x-log-apiversion version is unsupported.|The error message returned because the value of the x-log-apiversion header field is invalid. Check the request header and make sure that the specified API version is supported.|
|400|MissAccessKeyId|x-log-accesskeyid does not exist in header.|The error message returned because no AccessKey ID exists in the Authorization header field. Check the request header and make sure that an AccessKey ID exists in the Authorization header field.|
|400|MissingSignatureMethod|x-log-signaturemethod does not exist in http header.|The error message returned because the x-log-signaturemethod header field does not exist. Check the request header and make sure that the x-log-signaturemethod field exists.|
|400|InvalidSignatureMethod|signature method method is unsupported.|The error message returned because the signature method specified in the x-log-signaturemethod header field is not supported. Check the request header and make sure that the specified signature method is supported.|
|400|RequestTimeTooSkewed|Request time exceeds server time more than 15 minutes.|The error message returned because the request is initiated more than 15 minutes before or after the current server time.|
|401|SignatureNotMatch|Signature signature is not matched.|The error message returned because the digital signature calculated by the client does not match the signature calculated by the server. Try again, or replace the AccessKey ID and try again.|
|401|Unauthorized|The AccessKeyId is unauthorized.|The error message returned because the specified AccessKey ID does not have the required permissions. Make sure that your AccessKey ID has access to Log Service.|
|The security token you provided is invalid.|The error message returned because the Security Token Service \(STS\) token is invalid. Check the request header and make sure that the STS token is valid.|
|The security token you provided has expired.|The error message returned because the STS token has expired. Apply for a new STS token and initiate a request again.|
|AccessKeyId not found: AccessKey ID|The error message returned because the AccessKey ID does not exist. Replace your AccessKey ID and initiate the request again.|
|AccessKeyId is disabled: AccessKey ID|The error message returned because the AccessKey ID is disabled. Make sure your AccessKey ID is enabled and initiate the request again.|
|Your SLS service has been forbidden.|The error message returned because your Log Service resources are disabled. Make sure that your Alibaba Cloud account has no overdue payments.|
|401|InvalidAccessKeyId|The access key id you provided is invalid: AccessKey ID.|The error message returned because the AccessKey ID is invalid. Check your AccessKey ID and make sure that the AccessKey ID is valid.|
|Your SLS service has not opened.|The error message returned because Log Service is not activated. Log on to the Log Service console or call the API to activate Log Service and initiate the request again.|
|403|WriteQuotaExceed|Write quota is exceeded.|The error message returned because the size of the logs that are written to Log Service has exceeded the quota. Optimize the settings for the request to reduce the number of written logs.|
|403|ReadQuotaExceed|Read quota is exceeded.|The error message returned because the size of the logs that are read from Log Service has exceeded the quota. Optimize the settings for the request to reduce the number of read logs.|
|404|ProjectNotExists|Project name does not exist.|The error message returned because the specified project does not exist. Check the project name and make sure that the project exists.|
|411|MissingContentLength|Content-Length does not exist in http header when it is necessary.|The error message returned because the Content-Length field does not exist in the request header. Check the request header and make sure that the Content-Length field exists.|
|415|InvalidContentType|Content-Type type is unsupported.|The error message returned because the value of the Content-Type header field is invalid. Make sure that the value of the Content-Type header field is valid.|
|500|InternalServerError|Internal server error message.|The error message returned because an internal server error occurred. Try again later.|
|503|ServerBusy|The server is busy, please try again later.|The error message returned because the server is busy. Try again later.|

The italic part in an error message indicates specific error information. For example, name in the ProjectNotExists error message is replaced by a specific project name.

