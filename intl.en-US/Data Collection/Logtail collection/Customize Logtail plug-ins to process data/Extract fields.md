# Extract fields

You can extract log fields by using regular expressions, start and stop keywords, single-character delimiters, or multi-character delimiters. You can also extract log fields by splitting key-value pairs. This topic describes the parameters of these modes. This topic also provides examples to show how to configure the modes.

## Extract log fields by using a regular expression

You can extract log fields by using a regular expression.

-   Parameters

    The following table describes the parameters that you can specify in the detail parameter when you set the type parameter to processor\_regex.

    |Parameter|Type|Required|Description|
    |:--------|:---|:-------|:----------|
    |SourceKey|String|Yes|The name of the source field.|
    |Regex|String|Yes|The regular expression. The fields to be extracted are enclosed in parentheses `()`.|
    |Keys|String array|Yes|The array of fields that are extracted, for example, \["ip", "time", "method"\].|
    |NoKeyError|Boolean|No|Specifies whether to report an error if a field is not matched. Default value: false. This value indicates that no error is reported if a field is not matched.|
    |NoMatchError|Boolean|No|Specifies whether to report an error if the regular expression does not match the value of a specified field. Default value: false. This value indicates that no error is reported if a field is not matched.|
    |KeepSource|Boolean|No|Specifies whether to retain the source field. Default value: false. This value indicates that the source field is not retained.|
    |FullMatch|Boolean|No|Default value: true. This value indicates that exact match is implemented when the regular expression specified in the Regex parameter attempts to match field values. If you set the value to false, partial match is implemented when the regular expression attempts to match field values.|

-   Configuration example

    The following example shows how to extract the value of the content field. Then, you can set the names of the destination fields to ip, time, method, url, request\_time, request\_length, status, length, ref\_url, and browser.

    -   Raw log entry

        ```
        "content" : "10.200. **. ** - - [10/Aug/2017:14:57:51 +0800] \"POST /PutData?
        Category=YunOsAccountOpLog&AccessKeyId=<yourAccessKeyId>&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=<yourSignature> HTTP/1.1\" 0.024 18204 200 37 \"-\" \"aliyun-sdk-java"
        ```

    -   Logtail configurations for data processing

        ```
        {
            "type" : "processor_regex",
            "detail" : {"SourceKey" : "content",
                 "Regex" : "([\\d\\.] +) \\S+ \\S+ \\[(\\S+) \\S+\\] \"(\\w+) ([^\\\"]*)\" ([\\d\\.]+) (\\d+) (\\d+) (\\d+|-) \"([^\\\"]*)\" \"([^\\\"]*)\" (\\d+)",
                 "Keys"   : ["ip", "time", "method", "url", "request_time", "request_length", "status", "length", "ref_url", "browser"],
                 "NoKeyError" : true,
                 "NoMatchError" : true,
                 "KeepSource" : false
            }
        }
        ```

    -   Result

        ```
        "ip" : "10.200. **.**"
        "time" : "10/Aug/2017:14:57:51"
        "method" : "POST"
        "url" : "/PutData? Category=YunOsAccountOpLog&AccessKeyId=<yourAccessKeyId>&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=<yourSignature>"
        "request_time" : "0.024"
        "request_length" : "18204"
        "status" : "200"
        "length" : "27"
        "ref_url" : "-"
        "browser" : "aliyun-sdk-java"
        ```


## Extract log fields by using start and stop keywords

You can use start and stop keywords to anchor strings and extract log fields. If an anchored substring is a JSON string, you can expand the substring and then extract the fields.

-   Parameters

    The following table describes the parameters that you can specify in the detail parameter when you set the type parameter to processor\_anchor.

    |Parameter|Type|Required|Description|
    |:--------|:---|:-------|:----------|
    |SourceKey|String|Yes|The name of the source field.|
    |Anchors|Anchor array|Yes|The list of the parameters that are set to anchor strings.|
    |NoAnchorError|Boolean|No|Specifies whether to report an error if no keyword is found. Default value: false. This value indicates that no error is reported if no keyword is found.|
    |NoKeyError|Boolean|No|Specifies whether to report an error if a field is not matched. Default value: false. This value indicates that no error is reported if a field is not matched.|
    |KeepSource|Boolean|No|Specifies whether to retain the source field. Default value: false. This value indicates that the source field is not retained.|

    The following table describes the parameters of the Anchors parameter.

    |Parameter|Type|Required|Description|
    |:--------|:---|:-------|:----------|
    |Start|String|Yes|The keyword that anchors the start of a substring in a string. If the parameter is not specified, the start of the string is matched.|
    |Stop|String|Yes|The keyword that anchors the end of a substring in a string. If the parameter is not specified, the end of the string is matched.|
    |FieldName|String|Yes|The name of the field to be extracted.|
    |FieldType|String|Yes|The type of the field to be extracted. Valid values: string and json.|
    |ExpondJson|Boolean|No|Specifies whether to expand an anchored JSON substring. Default value: false. This value indicates that an anchored JSON substring is not expanded.This parameter is available only if the value of the FieldType parameter is set to json. |
    |ExpondConnecter|String|No|The character that is used to connect expanded keys. Default value: \_.|
    |MaxExpondDepth|Int|No|The maximum depth of JSON expansion. Default value: 0. This value indicates that the depth of JSON expansion is unlimited.|

-   Configuration example

    The following example shows how to extract the value of the content field. Then, you can set the names of the destination fields to time, val\_key1, val\_key2, val\_key3, value\_key4\_inner1, and value\_key4\_inner2.

    -   Raw log entry

        ```
        "content" : "time:2017.09.12 20:55:36\tjson:{\"key1\" : \"xx\", \"key2\": false, \"key3\":123.456, \"key4\" : { \"inner1\" : 1, \"inner2\" : false}}"
        ```

    -   Logtail configurations for data processing

        ```
        {
           "type" : "processor_anchor",
           "detail" : {"SourceKey" : "content",
              "Anchors" : [
                  {
                      "Start" : "time",
                      "Stop" : "\t",
                      "FieldName" : "time",
                      "FieldType" : "string",
                      "ExpondJson" : false
                  },
                  {
                      "Start" : "json:",
                      "Stop" : "",
                      "FieldName" : "val",
                      "FieldType" : "json",
                      "ExpondJson" : true 
                  }
              ]
          }
        }
        ```

    -   Result

        ```
        "time" : "2017.09.12 20:55:36"
        "val_key1" : "xx"
        "val_key2" : "false"
        "val_key3" : "123.456"
        "value_key4_inner1" : "1"
        "value_key4_inner2" : "false"
        ```


## Extract log fields by using a single-character delimiter

You can use a specified single-character delimiter to delimit fields. This processing method allows you to specify a quote to enclose the delimiter.

-   Parameters

    The following table describes the parameters that you can specify in the detail parameter when you set the type parameter to processor\_split\_char.

    |Parameter|Type|Required|Description|
    |:--------|:---|:-------|:----------|
    |SourceKey|String|Yes|The name of the source field.|
    |SplitSep|String|Yes|The single-character delimiter. The single-character delimiter. You can specify a non-printable character as a single-character delimiter, for example, \\u0001.|
    |SplitKeys|String array|Yes|The names of the delimited fields, for example, \["ip", "time", "method"\].|
    |QuoteFlag|Boolean|No|Specifies whether to use a quote to enclose the specified delimiter. Default value: false. This value indicates that a quote is not used.|
    |Quote|String|No|The quote. The quote must be a single character. You can specify a non-printable character as a quote, for example, \\u0001.This parameter is available only if the value of QuoteFlag is set to true. |
    |NoKeyError|Boolean|No|Specifies whether to report an error if a field is not matched. Default value: false. This value indicates that no error is reported if a field is not matched.|
    |NoMatchError|Boolean|No|Specifies whether to report an error if a delimiter is not matched. Default value: false. This value indicates that no error is reported if a delimiter is not matched.|
    |KeepSource|Boolean|No|Specifies whether to retain the source field. Default value: false. This value indicates that the source field is not retained.|

-   Configuration example

    The following example shows how to extract the value of the content field by using a delimiter \(\|\). Then, you can set the names of the destination fields to ip, time, method, url, request\_time, request\_length, status, length, ref\_url, and browser.

    -   Raw log entry

        ```
        "content" : "10. **. **. **|10/Aug/2017:14:57:51 +0800|POST|PutData?
        Category=YunOsAccountOpLog&AccessKeyId=<yourAccessKeyId>&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=<yourSignature>|0.024|18204|200|37|-|
        aliyun-sdk-java"
        ```

    -   Logtail configurations for data processing

        ```
        {
           "type" : "processor_split_char",
           "detail" : {"SourceKey" : "content",
              "SplitSep" : "|",
              "SplitKeys" : ["ip", "time", "method", "url", "request_time", "request_length", "status", "length", "ref_url", "browser"]     
          }
        }
        ```

    -   Result

        ```
        "ip" : "10. **. **.**"
        "time" : "10/Aug/2017:14:57:51 +0800"
        "method" : "POST"
        "url" : "/PutData? Category=YunOsAccountOpLog&AccessKeyId=<yourAccessKeyId>&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=<yourSignature>"
        "request_time" : "0.024"
        "request_length" : "18204"
        "status" : "200"
        "length" : "27"
        "ref_url" : "-"
        "browser" : "aliyun-sdk-java"
        ```


## Extract log fields by using a multi-character delimiter

You can use a specified multi-character delimiter to delimit fields. You cannot specify a quote to enclose the delimiter.

-   Parameters

    The following table describes the parameters that you can specify in the detail parameter when you set the type parameter to processor\_split\_string.

    |Parameter|Type|Required|Description|
    |:--------|:---|:-------|:----------|
    |SourceKey|String|Yes|The name of the source field.|
    |SplitSep|String|Yes|The multi-character delimiter. The multi-character delimiter. You can specify multiple non-printable characters as a multi-character delimiter, for example, \\u0001\\u0002.|
    |SplitKeys|String array|Yes|The names of the delimited fields, for example, \["key1", "key2"...\].|
    |PreserveOthers|Boolean|No|Specifies whether to retain excess fields if the number of fields is greater than the number of fields specified by the SplitKeys parameter. Default value: false. This value indicates that excess fields are not retained.|
    |ExpandOthers|Boolean|No|Specifies whether to parse excess fields. Default value: false. This value indicates that excess fields are not parsed.|
    |ExpandKeyPrefix|String|No|The name prefix of excess fields. For example, if you specify expand\_ for the parameter, the first two excess fields are named expand\_1 and expand\_2.|
    |NoKeyError|Boolean|No|Specifies whether to report an error if a field is not matched. Default value: false. This value indicates that no error is reported if a field is not matched.|
    |NoMatchError|Boolean|No|Specifies whether to report an error if a delimiter is not matched. Default value: false. This value indicates that no error is reported if a delimiter is not matched.|
    |KeepSource|Boolean|No|Specifies whether to retain the source field. This value indicates that the source field is not retained.|

-   Configuration example

    The following example shows how to extract the value of the content field by using a delimiter \(\|\#\|\). Then, you can set the names of the destination fields to ip, time, method, url, request\_time, request\_length, status, expand\_1, expand\_2, and expand\_3.

    -   Raw log entry

        ```
        "content" : "10. **. **. **|#|10/Aug/2017:14:57:51 +0800|#|POST|#|PutData?
        Category=YunOsAccountOpLog&AccessKeyId=<yourAccessKeyId>&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=<yourSignature>|#|0.024|#|18204|#|200|#|27|#|-|#|
        aliyun-sdk-java"
        ```

    -   Logtail configurations for data processing

        ```
        {
           "type" : "processor_split_string",
           "detail" : {"SourceKey" : "content",
              "SplitSep" : "|#|",
              "SplitKeys" : ["ip", "time", "method", "url", "request_time", "request_length", "status"],
              "PreserveOthers" : true,
              "ExpandOthers" : true,
              "ExpandKeyPrefix" : "expand_"
          }
        }
        ```

    -   Result

        ```
        "ip" : "10. **. **.**"
        "time" : "10/Aug/2017:14:57:51 +0800"
        "method" : "POST"
        "url" : "/PutData? Category=YunOsAccountOpLog&AccessKeyId=<yourAccessKeyId>&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=<yourSignature>"
        "request_time" : "0.024"
        "request_length" : "18204"
        "status" : "200"
        "expand_1" : "27"
        "expand_2" : "-"
        "expand_3" : "aliyun-sdk-java"
        ```


## Extract log fields by splitting key-value pairs

You can split key-value pairs to extract log fields.

-   Parameters

    The following table describes the parameters that you can specify in the detail parameter when you set the type parameter to processor\_split\_key\_value.

    **Note:** Only Logtail V0.16.26 or later supports the plug-in.

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |SourceKey|string|Yes|The name of the source field.|
    |Delimiter|string|No|The delimiter between key-value pairs. Default value: `\t`.|
    |Separator|string|No|The delimiter used to separate the key and the value in a single key-value pair. Default value: :.|
    |KeepSource|Boolean|No|Specifies whether to retain the source field. Default value: true. This value indicates that the source field is retained.|
    |ErrIfSourceKeyNotFound|Boolean|No|Specifies whether to trigger an alert if a field is not matched. Default value: true. This value indicates that an alert is triggered if a field is not matched.|
    |DiscardWhenSeparatorNotFound|Boolean|No|Specifies whether to drop the key-value pair if a field is not matched. Default value: false. This value indicates that the key-value pair is not dropped if a field is not matched.|
    |ErrIfSeparatorNotFound|Boolean|No|Specifies whether to trigger an alert if the specified delimiter \(Separator\) does not exist. Default value: true. This value indicates that an alert is triggered if the specified delimiter does not exist.|

-   Configuration example

    The following example shows how to split the key-value pair of the content field. The delimiter used to separate key-value pairs is a tab character \(`/t`\). The delimiter used to separate the key and the value in a single key-value pair is a colon \(:\).

    -   Raw log entry

        ```
        "content": "class:main\tuserid:123456\tmethod:get\tmessage:\"wrong user\""
        ```

    -   Logtail configurations for data processing

        ```
        {
          "processors":[
            {
              "type":"processor_split_key_value",
              "detail": {
                "SourceKey": "content",
                "Delimiter": "\t",
                "Separator": ":",
                "KeepSource": true
              }
            }
          ]
        }
        ```

    -   Result

        ```
        "content": "class:main\tuserid:123456\tmethod:get\tmessage:\"wrong user\""
        "class": "main"
        "userid": "123456"
        "method": "get"
        "message": "\"wrong user\""
        ```


