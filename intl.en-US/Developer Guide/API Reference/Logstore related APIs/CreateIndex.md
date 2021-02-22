# CreateIndex

Creates indexes for a specified Logstore.

## Request syntax

```
POST /logstores/logstoreName/index HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
Host: ProjectName.Endpoint
x-log-signaturemethod: hmac-sha1
Date: GMT Date
{
  "line": line,
  "keys": keys
}
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The CreateIndex operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|my-project|The name of the project.|
    |logstoreName|String|Yes|my-logstore|The name of the Logstore.|
    |keys|Object|No|None|The field search configuration. It contains one or more key-value indexes, where the key is the field name and the value is the field value. You must specify this parameter, the line parameter, or both. For more information, see [Examples](#section_sdw_lwz_c2b).|
    |line|Object|No|None|The full-text index configuration. You must specify this parameter, the keys parameter, or both. For more information, see [Examples](#section_sdw_lwz_c2b).|

    -   The following table describes the full-text index configuration.

|Parameter|Type|Required|Example|Description|
|:--------|:---|:-------|-------|:----------|
|caseSensitive|Boolean|No|true|Specifies whether the field is case-sensitive.-   If you set the value to true, the field is case-sensitive.
-   If you set the value to false, the field is not case-sensitive. |
|chn|Boolean|No|true|Specifies whether the field contains Chinese characters.-   If you set the value to true, the field contains Chinese characters.
-   If you set the value to false, the field does not contain Chinese characters. |
|token|Array|Yes|\["\\n","\\t","\\r"\]|The list of delimiters. A delimiter can be set to specify the method in which a value is delimited. For more information, see [Examples](#section_sdw_lwz_c2b).|
|include\_keys|Array|No|None|The list of included fields. This parameter and the exclude\_keys parameter cannot be specified at the same time.|
|exclude\_keys|Array|No|None|The list of excluded fields. This parameter and the include\_keys parameter cannot be specified at the same time.|

    -   The following table describes the field search parameters.

|Parameter|Type|Required|Example|Description|
|:--------|:---|:-------|-------|:----------|
|type|String|Yes|index|The type of the field.|
|alias|String|No|agent\_alias|The alias of the field.|
|chn|Boolean|No|true|Specifies whether the field contains Chinese characters. This parameter takes effect only when the value of the type parameter is text.-   If you set the value to true, the field contains Chinese characters.
-   If you set the value to false, the field does not contain Chinese characters. |
|token|Array|No|\["\\n","\\t","\\r"\]|The list of delimiters. This parameter takes effect only when the value of the type parameter is text.|
|caseSensitive|Boolean|No|true|Specifies whether the field is case-sensitive. This parameter takes effect only when the value of the type parameter is text.-   If you set the value to true, the field is case-sensitive.
-   If you set the value to false, the field is not case-sensitive. |
|doc\_value|Boolean|No|true|Specifies whether to enable the analytics feature for the field.-   If you set the value to true, the analytics feature is enabled for the field.
-   If you set the value to false, the analytics feature is disabled for the field. |


## Response parameters

-   Response headers

    The CreateIndex operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. If the request is successful, no other elements are returned.


## Examples

-   Sample requests

    ```
    POST /logstores/my-logstore/index HTTP/1.1
    Header :
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    x-log-bodyrawsize: 0
    User-Agent: sls-java-sdk-v-0.6.1
    x-log-apiversion: 0.6.0
    Host: my-project.cn-shanghai.log.aliyuncs.com
    x-log-signaturemethod: hmac-sha1
    Date: Mon, 07 May 2018 09:47:20 GMT
    Content-Type: application/json
    Content-MD5: 1860B805A6AA5288B97B32CF3B519112
    Content-Length: 316
    Connection: Keep-Alive
    }
    Body :
    {
      "line": {
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
      "keys": {
        "agent": {
          "doc_value": true,
          "caseSensitive": true,
          "alias": "agent_alias",
          "type": "text",
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
        }
      }
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200
    Server: nginx/1.12.1
    Content-Length: 0
    Connection: close
    Access-Control-Allow-Origin: *
    Date: Mon, 07 May 2018 09:47:21 GMT
    x-log-requestid: 5AF020A98CBAA2EF52AB66C9
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|400|IndexInfoInvalid|Required field token is lacking or of error format.|The error message returned because the required field tags are not specified or their formats are invalid.|
|400|IndexAlreadyExist|Logstore index is already created.|The error message returned because the specified Logstore index already exists.|
|404|ProjectNotExist|The Project does not exist : projectName.|The error message returned because the specified project does not exist.|
|404|LogStoreNotExist|Logstore logstoreName dose not exist.|The error message returned because the specified Logstore does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

