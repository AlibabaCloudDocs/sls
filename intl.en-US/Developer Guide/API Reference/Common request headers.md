# Common request headers

This topic describes the common request headers of Log Service API operations.

## Parameters

Log Service API operations are RESTful API operations that are based on the HTTP protocol. A set of common request headers can be used in these API operations. The following table describes the common request headers of all API requests.

|Header name|Type|Required|Description|
|:----------|:---|--------|:----------|
|Accept|String|No|The type of the value that the client expects Log Service to return. Valid values: application/json and application/x-protobuf. This header is valid only for GET requests. The value varies with API definitions.|
|Accept-Encoding|String|No|The compression algorithm that the client expects Log Service to return. Valid values: LZ4, DEFLATE, and null \(not compressed\). The header is valid only for GET requests. The value varies with API definitions.|
|Authorization|String|Yes|The content of the signature. For more information, see [Request signatures](/intl.en-US/Developer Guide/API Reference/Request signatures.md).|
|Content-Length|Numerical value|No|The length of the HTTP request body that is defined in RFC 2616. For requests without bodies, this header is not required.|
|Content-MD5|String|No|The string that is generated after the request body is calculated based on the MD5 algorithm. The value is in uppercase. For requests without bodies, this header is not required.|
|Content-Type|String|No|The type of the HTTP request body that is defined in RFC 2616. Valid values: application/x-protobuf. For requests without bodies, this header is not required. The value varies with API definitions.|
|Date|String|Yes|The time when the request is sent. Only the RFC 822 message format is supported. The time is in GMT. The string is in the following format: %a, %d%b%Y %H:%M:%S GMT, for example, Mon, 3 Jan 2010 08:33:47 GMT.|
|Host|String|Yes|The host name of the HTTP request. The value does not include protocol headers such as `http://`. For example, the host name is `big-game.cn-hangzhou.sls.aliyuncs.com`.|
|x-log-apiversion|String|Yes|The API version. The current version is 0.6.0.|
|x-log-bodyrawsize|Numeric value|No|The initial size of the request body. For requests without bodies, the value is 0. For requests with compressed bodies, the value is the size of the body before compression. Value range: 0 to 3145728. This header is required only when the body is compressed.|
|x-log-compresstype|String|No|The algorithm that is used to compress the request body. LZ4 and DEFLATE that are specified in RFC 1951 are supported. If you need to use the ZLIB compressed data format, see RFC 1950. For requests with uncompressed bodies, this header is not required.|
|x-log-date|String|No|The time when the request is sent. The format is the same as that of the Date header. If a request contains this common request header, the value of this header replaces that of the standard Date header. The value is used by Log Service to verify the request. This header is not included in signature calculation. The HTTP standard Date header must be included in the request regardless of whether the request contains an x-log-date header.|
|x-log-signaturemethod|String|Yes|The signature calculation method. Valid value: `hmac-sha1`.|
|x-acs-security-token|String|No|The Security Token Service \(STS\) temporary identity is used to send data. This request header is required only when the STS temporary identity is used.|

**Note:**

-   The maximum difference between the time in the Date header and the time when Log Service receives the request is 15 minutes. If the difference exceeds 15 minutes, Log Service rejects the request. For requests with x-log-date headers, the time difference is calculated based on the value of the x-log-date header.
-   If a compression algorithm is specified in the x-log-compresstype header of a request, the raw data must be compressed and then placed into the HTTP request body. The values of Content-Length and Content-MD5 headers are calculated based on the compressed body.
-   For HTTP requests that are sent from specific platforms, you cannot specify a Date header. The current time is sent as specified by the internal database of the platform. Therefore, the correct value of the Date header cannot be used to calculate the request signature. In this case, you must specify an x-log-date header. This value is used to calculate the request signature. After an API request is received, Log Service determines whether the request contains an x-log-date header. If the request contains an x-log-date header, Log Service uses the value of the x-log-date header to verify the signature. Otherwise, Log Service uses the value of the standard Date header to verify the signature.

## Example

```
POST / HTTP/1.1
Authorization: LOG <yourAccessKeyId>:<yourSignature>
x-log-bodyrawsize: 0
User-Agent: sls-java-sdk-v-0.6.1
x-log-apiversion: 0.6.0
Host: my-project-test.cn-shanghai.log.aliyuncs.com
x-log-signaturemethod: hmac-sha1
Date: Sun, 27 May 2018 07:43:26 GMT
Content-Type: application/json
Content-MD5: A7967D81EFF5E3CD447FB6D8DF294E20
Content-Length: 80
Connection: Keep-Alive
```

