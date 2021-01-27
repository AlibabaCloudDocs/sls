# GetIndex

Queries the indexes of a specified Logstore.

## Request syntax

```
GET /logstores/logstoreName/index HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
Host: ProjectName.Endpoint
x-log-signaturemethod: hmac-sha1
Date: GMT Date
Content-Type: application/x-protobuf
Connection: Keep-Alive
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The GetIndex operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|my-project|The name of the project.|
    |logstoreName|String|Yes|logstore-4|The name of the Logstore.|


## Response parameters

-   Response headers

    The GetIndex operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the GetIndex operation succeeds, the response body contains the indexes of the specified project and Logstore. The following table describes the parameters of the response body.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |index\_mode|String|v2|The type of the index.|
    |keys|Object|None|The field search configuration. It contains one or more key-value indexes, where the key is the field name and the value is the field value.|
    |line|Object|None|The full-text index configuration.|
    |storage|String|pg|The type of the store. Default value: pg.|
    |ttl|Integer|30|The time to live \(TTL\) of the index file. Valid values: 7, 30, and 90. Unit: days.|
    |lastModifyTime|Integer|1524155379|The timestamp when the index configuration was last modified. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|

    -   The following table describes the full-text index configuration.

|Parameter|Type|Example|Description|
|:--------|:---|-------|:----------|
|caseSensitive|Boolean|false|Specifies whether the field is case-sensitive.-   If you set the value to true, the field is case-sensitive.
-   If you set the value to false, the field is not case-sensitive. |
|chn|Boolean|false|Specifies whether the field contains Chinese characters.-   If you set the value to true, the field contains Chinese characters.
-   If you set the value to false, the field does not contain Chinese characters. |
|token|Array|\["\\n","\\t","\\r"\]|The list of delimiters.|
|include\_keys|Array|None|The list of included fields.|
|exclude\_keys|Array|None|The list of excluded fields.|

    -   The following table describes the field search parameters.

|Parameter|Type|Example|Description|
|:--------|:---|-------|:----------|
|type|String|text|The type of the field.|
|alias|String|None|The alias of the field.|
|chn|Boolean|false|Specifies whether the field contains Chinese characters. This parameter must be specified if the type parameter is set to text.-   If you set the value to true, the field contains Chinese characters.
-   If you set the value to false, the field does not contain Chinese characters. |
|token|Array|\["\\n","\\t","\\r"\]|The list of delimiters. This parameter must be specified if the type parameter is set to text.|
|caseSensitive|Boolean|false|Specifies whether the field is case-sensitive. This parameter must be specified if the type parameter is set to text.-   If you set the value to true, the field is case-sensitive.
-   If you set the value to false, the field is not case-sensitive. |
|doc\_value|Boolean|true|Specifies whether to enable the analytics feature for the field.-   If you set the value to true, the analytics feature is enabled for the field.
-   If you set the value to false, the analytics feature is disabled for the field. |


## Examples

-   Sample requests

    ```
    GET /logstores/logstore-4/index HTTP/1.1
    Header :
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: my-project.cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Sun, 06 May 2018 13:08:42 GMT
    Content-Type: application/x-protobuf
    Connection: Keep-Alive
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header :
    {
    Server: nginx/1.12.1
    Content-Type: application/json
    Content-Length: 712
    Connection: close
    Access-Control-Allow-Origin: *
    Date: Sun, 06 May 2018 13:08:42 GMT
    x-log-requestid: 5AEEFE5A8B8AEB5E6C82B395
    }
    Body :
    {
      "index_mode": "v2",
      "keys": {
        "agent": {
          "alias": "",
          "caseSensitive": false,
          "chn": false,
          "doc_value": true,
          "token": [
            ",",
            " ",
            "'",
            "\"",
            ";",
            "=",
            "(",
            ")",
            "[",
            "]",
            "{",
            "}",
            "?",
            "@",
            "&",
            "<",
            ">",
            "/",
            ":",
            "\n",
            "\t",
            "\r"
          ],
          "type": "text"
        },
        "bytes": {
          "alias": "",
          "doc_value": true,
          "type": "long"
        },
        "remote_ip": {
          "alias": "",
          "caseSensitive": false,
          "chn": false,
          "doc_value": true,
          "token": [
            ",",
            " ",
            "'",
            "\"",
            ";",
            "=",
            "(",
            ")",
            "[",
            "]",
            "{",
            "}",
            "?",
            "@",
            "&",
            "<",
            ">",
            "/",
            ":",
            "\n",
            "\t",
            "\r"
          ],
          "type": "text"
        },
        "response": {
          "alias": "",
          "doc_value": true,
          "type": "long"
        }
      },
      "line": {
        "caseSensitive": false,
        "chn": false,
        "token": [
          ",",
          " ",
          "'",
          "\"",
          ";",
          "=",
          "(",
          ")",
          "[",
          "]",
          "{",
          "}",
          "?",
          "@",
          "&",
          "<",
          ">",
          "/",
          ":",
          "\n",
          "\t",
          "\r"
        ]
      },
      "storage": "pg",
      "ttl": 30,
      "lastModifyTime": 1524155379
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|400|IndexConfigNotExist|index config doesn't exist.|The error message returned because the specified index does not exist.|
|404|ProjectNotExist|The Project does not exist : projectName.|The error message returned because the specified project does not exist.|
|404|LogStoreNotExist|logstore logstoreName dose not exist.|The error message returned because the specified Logstore does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

