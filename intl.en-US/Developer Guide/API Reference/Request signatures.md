# Request signatures

Each API request that is sent to Log Service is signed and verified to ensure data security.

## Verification process

The symmetric-key algorithm is used to verify API requests based on AccessKey pairs of Alibaba Cloud. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).

The following picture describes the procedure.

![全验证流程](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3360140061/p5911.png)

The following procedure describes how an API request is verified:

1.  The client generates a string to be signed based on the HTTP header and body of an API request.

2.  The client signs the string that is generated in [Step 1](#step_wlu_46z_6ma) by using an AccessKey pair. This generates the digital signature of the API request.

3.  The client sends the signed API request to the server.

4.  The server receives the request and calculates the digital signature of the request. The same process that is described in Step 1 and Step 2 is used.

    **Note:** The server retrieves the AccessKey pair that is used by this request from the backend.

5.  The server compares the calculated digital signature with the digital signature that is sent from the client. If the two signatures are the same, the verification passes.


![Full verification process](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3360140061/p5911.png)

The verification process provides the following benefits:

-   Identify the user that sends the API request.

    A user must specify an AccessKey pair to generate the digital signature for an API request. When an AccessKey pair is specified, Log Service uses the AccessKey pair to identify the user and implement access control.

-   Check whether an API request is compromised during transmission.

    The server calculates the digital signature of an API request and compares the digital signature with the signature calculated by the client. If the API request is compromised during transmission, the two signatures are different and the verification fails.


## Sign an API request

The client calculates the digital signature of an API request, includes the signature in the HTTP Authorization request header, and sends the request to the server. The following example is a sample Authorization request header field:

```
Authorization = "LOG" + AccessKeyId + ":" + Signature
```

The value of the Authorization request header field contains an AccessKey ID and the digital signature that is calculated based on the AccessKey ID and the corresponding AccessKey secret. The following procedure describes how to calculate a digital signature:

1.  Obtain an Alibaba Cloud AccessKey pair.

    For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md#). You can use an existing AccessKey pair or create an AccessKey pair. The AccessKey pair must be in the **Active** state.

2.  Generate a string to be signed for the API request.

    The string contains the information about the HTTP request method, HTTP request header, and request body. The string is in the following format:

    ```
    SignString = VERB + "\n"
                 + CONTENT-MD5 + "\n"
                 + CONTENT-TYPE + "\n"
                 + DATE + "\n"
                 + CanonicalizedLOGHeaders + "\n"
                 + CanonicalizedResource
    ```

    In the preceding formula, `\n` is a newline character and the plus sign \(`+`\) is a string concatenation operator. The following table describes the other elements.

    |Parameter|Description|Example|
    |:--------|:----------|:------|
    |`VERB`|The HTTP request method.|PUT, GET, and POST|
    |`CONTENT-MD5`|The MD5 value of the HTTP request body. The value must be an uppercase string.|875264590688CA6171F6228AF5BBB3D2|
    |`CONTENT-TYPE`|The type of the HTTP request body.|application/x-protobuf|
    |`DATE`|The standard timestamp header field of the HTTP request. This timestamp header field follows the RFC 1123 specification and is in GMT.|Mon,3 Jan 2010 08:33:47 GMT|
    |`CanonicalizedLOGHeaders`|The string that is constructed from the custom fields in the HTTP request. The custom fields are prefixed with `x-log` and `x-acs`.|x-log-apiversion:0.6.0\\nx-log-bodyrawsize:50\\nx-log-signaturemethod:hmac-sha1|
    |`CanonicalizedResource`|The string that is constructed from the HTTP response.|/logstores/app\_log|

    If an HTTP request does not have a request body, the CONTENT-MD5 and CONTENT-TYPE fields are empty strings. The string to be signed is in the following format:

    ```
    SignString = VERB + "\n"
                 + "\n"
                 + "\n"
                 + DATE + "\n"
                 + CanonicalizedLOGHeaders + "\n"
                 + CanonicalizedResource
    ```

    The `x-log-date`field is a custom HTTP request header field. For more information, see[Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md). If you specify this field in an API request, the value of this field will replace the value of the DATE header field in signature calculation.

    -   The following procedure describes how to construct the CanonicalizedLOGHeaders string:
        1.  Convert the names of all HTTP request fields that are prefixed with `x-log` and `x-acs` into lowercase letters.
        2.  Sort all custom request fields that are obtained in the previous step in lexicographical order.
        3.  Delete all spaces on either side of a delimiter between the request header and content.
        4.  Separate all headers and contents with the `\n` delimiter to obtain the CanonicalizedLOGHeader string.
    -   The following procedure describes how to construct the CanonicalizedResource string:

        1.  Set the CanonicalizedResource string to an empty string `""`.
        2.  Enter the Log Service resources that you want to access, for example, /logstores/logstorename. If no `Logstore` is created in a project, this string is left blank.
        3.  If the request contains a query string `QUERY_STRING`, add a question mark \(`?`\) followed by the query string at the end of the CanonicalizedResource string.
        `QUERY_STRING` is the lexicographic string of the request parameters in the URL. Equal signs \(`=`\) are used between the names and values of parameters. The parameter name-value pairs are then sorted in lexicographical order and connected with ampersands \(`&`\) to form a string. The following formula shows how to construct a query string:

        ```
        QUERY_STRING = "KEY1=VALUE1" + "&" + "KEY2=VALUE2"
        ```

3.  Generate a digital signature for the request.

    Use the digital signature algorithm `hmac-sha1` to calculate the digital signature of an API request. Log Service supports only this algorithm for the calculation of digital signatures. Use the algorithm in the following format:

    ```
    Signature = base64(hmac-sha1(UTF8-Encoding-Of(SignString), AccessKeySecret))
    ```

    -   The HMAC-SHA1 algorithm is defined in [RFC 2104](http://www.ietf.org/rfc/rfc2104.txt?spm=5176.doc29012.2.3.2I6hJy&file=rfc2104.txt). The AccessKey secret in the preceding formula must match the AccessKey ID in the Authorization field. Otherwise, the request cannot be verified by Log Service.
    -   After the digital signature is generated, enter the signature into the Authorization header field. Then, enter the header field in the HTTP request and send the request.
    -   A signature string must be encoded in `UTF-8`.
    -   The `CONTENT-MD5` and `CONTENT-TYPE` fields are optional. If the values of these fields are null, replace the null values with line breaks `\n` instead.

## Examples

The following two examples demonstrate how to sign an API request. The following AccessKey ID and AccessKey secret are used in an API request:

```
AccessKeyId = "bq2sjzesjmo86kq*********"
AccessKeySecret = "4fdO2fTDDnZPU/L7CHNd********"
```

-   Example 1

    To list all Logstores of the project named ali-test-project, send the following GET request:

    ```
    GET /logstores?logstoreName=&offset=0&size=1000 HTTP 1.1
    Mon, 09 Nov 2015 06:11:16 GMT
    Host: ali-test-project.regionid.example.com
    x-log-apiversion: 0.6.0
    x-log-signaturemethod: hmac-sha1
    ```

    The following is the string to be signed that is generated for the API request:

    ```
    GET\n\n\nMon, 09 Nov 2015 06:11:16 GMT\nx-log-apiversion:0.6.0\nx-log-signaturemethod:hmac-sha1\n/logstores?logstoreName=&offset=0&size=1000
    ```

    A GET request does not have an HTTP body. The values of the CONTENT-MD5 and CONTENT-TYPE fields are empty strings. The following string is the digital signature calculated by using the specified AccessKey secret:

    ```
    jEYOTCJs2e88o+y5F4/S5IsnBJQ=
    ```

    The following is the signed API request:

    ```
    GET /logstores?logstoreName=&offset=0&size=1000 HTTP 1.1
    Mon, 09 Nov 2015 06:11:16 GMT
    Host: ali-test-project.regionid.example.com
    x-log-apiversion: 0.6.0
    x-log-signaturemethod: hmac-sha1
    Authorization: LOG bq2sjzesjmo86kq35behupbq:jEYOTCJs2e88o+y5F4/S5IsnBJQ=
    ```

-   Example 2

    You want to write the following log entry to the Logstore named test-logstore that belongs to the project named ali-test-project:

    ```
    topic=""
    time=1447048976
    source="10.10.10.1"
    "TestKey": "TestContent"
    ```

    To do this, send the following POST request:

    ```
    POST /logstores/test-logstore HTTP/1.1
    Date: Mon, 09 Nov 2015 06:03:03 GMT
    Host: test-project.regionid.example.com
    x-log-apiversion: 0.6.0
    x-log-signaturemethod: hmac-sha1
    Content-MD5: 1DD45FA4A70A9300CC9FE7305AF2C494
    Content-Length: 52
    x-log-apiversion:0.6.0
    x-log-bodyrawsize:50
    x-log-compresstype:lz4
    x-log-signaturemethod:hmac-sha1
    <The log entry is serialized to byte streams in the ProtoBuffer format.>
    ```

    The log entry that you want to write to Log Service is serialized to byte streams in the ProtoBuffer format and then used as the HTTP request body. For more information about the ProtoBuffer format, see [Data encoding](/intl.en-US/Developer Guide/API Reference/Common resources/Data encoding.md). Therefore, the value of the Content-Type header field is application/x-protobuf. The value of the Content-MD5 header field is the MD5 value of the HTTP request body. The following string is the string to be signed that is generated for the API request:

    ```
    POST\n1DD45FA4A70A9300CC9FE7305AF2C494\napplication/x-protobuf\nMon, 09 Nov 2015 06:03:03 GMT\nx-log-apiversion:0.6.0\nx-log-bodyrawsize:50\nx-log-compresstype:lz4\nx-log-signaturemethod:hmac-sha1\n/logstores/test-logstore
    ```

    The following is the digital signature calculated by using the specified AccessKey secret:

    ```
    XWLGYHGg2F2hcfxWxMLiNkGki6g=
    ```

    The following request is the signed API request:

    ```
    POST /logstores/test-logstore HTTP/1.1
    Date: Mon, 09 Nov 2015 06:03:03 GMT
    Host: test-project.regionid.example.com
    x-log-apiversion: 0.6.0
    x-log-signaturemethod: hmac-sha1
    Content-MD5: 1DD45FA4A70A9300CC9FE7305AF2C494
    Content-Length: 52
    x-log-apiversion:0.6.0
    x-log-bodyrawsize:50
    x-log-compresstype:lz4
    x-log-signaturemethod:hmac-sha1
    Authorization: LOG bq2sjzesjmo86kq35behupbq:XWLGYHGg2F2hcfxWxMLiNkGki6g=
    <The log is serialized to byte streams in the ProtoBuffer format.>
    ```


