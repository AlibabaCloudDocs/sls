# Process data

Logtail allows you to configure plug-ins for data processing. Each plug-in defines a processing method. You can configure one or more processing methods for a data source. Then, Logtail executes the processing methods in sequence. This topic describes the available data processing methods and describes how to configure the methods.

## Limits

-   Performance limits

    When a plug-in is used for data processing, Logtail consumes more resources such as CPU. You can modify the Logtail parameter settings based on your business requirements. For more information, see [t13060.dita\#concept\_sdg\_czb\_wdb](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md). If raw data is generated at a rate higher than 5 MB/s, we recommend that you do not use complex plug-ins to process the data. You can use Logtail plug-ins to perform simple data processing, and then use the [data transformation](/intl.en-US/Data Transformation/Overview.md) feature to further process the data.

-   Limits on text logs

    Log Service allows you to process text logs in basic modes such as the regular expression, NGINX, or JSON mode. Log Service also allows you to use Logtail plug-ins to process text logs.However, Logtail plug-ins have the following limits on text logs:

    -   If you enable the plug-in processing feature, some advanced features of the specified mode become unavailable. For example, you cannot configure the filter, upload raw logs, configure system time zone, drop logs that fail to be parsed, or upload incomplete log entries \(delimiter mode\).
    -   Plug-ins use the line mode to process text logs. In this mode, file-level metadata such as \_\_tag\_\_:\_\_path\_\_ and \_\_topic\_\_ is stored in each log entry. If you use Logtail plug-ins to process data, the following limits apply to tag-related features:
        -   You cannot use the context query and LiveTail features because these features depend on fields such as \_\_tag\_\_:\_\_path\_\_.
        -   The name of the \_\_topic\_\_ field is renamed to \_\_log\_topic\_\_.
        -   Fields such as \_\_tag\_\_:\_\_path\_\_ no longer have original field indexes. You must configure indexes for these fields.

## Usage notes

When you configure data processing methods, you must set the key in the configuration file to processors and set the value to an array of JSON objects. Each object of the array contains the details of a processing method.

Each object contains the type and detail fields. The type field specifies the type of the processing method and the detail field contains configuration details.

```
"processors" : [
        {
            "type" : "processor_anchor",
            "detail" : {
                ...
            }
        },
        {
            "type" : "processor_regex",
            "detail" : {
                ...
            }
        }
    ]
```

## Processing methods

You can configure one or more of the following processing methods for collected data:

-   [Regular expression-based extraction](#section_bjz_2wp_rdb)
-   [Anchor-based extraction](#section_x2y_fwp_rdb)
-   [Single-character delimiter-based extraction](#section_bwg_gwp_rdb)
-   [Multi-character delimiter-based extraction](#section_az5_hwp_rdb)
-   [GeoIP conversion](#section_e1c_3wp_rdb)
-   [Regular expression-based filter](#section_d5t_3wp_rdb)
-   [Field enrichment](#section_eol_frs_8pr)
-   [Field dropping](#section_2vf_x91_xol)
-   [Log time extraction \(Go\)](#section_skm_79v_5qx)
-   [JSON expansion](#section_9hv_3tz_yxc)
-   [Field encapsulation](#section_cj6_lus_1u0)
-   [Field renaming](#section_n7e_4sm_utl)
-   [Log time extraction \(Strptime\)](#section_act_wz5_t1c)

## Regular expression-based extraction

This processing method extracts fields by using a regular expression to match the specified fields.

The type of the plug-in is processor\_regex. The following table describes the parameters of the processing method.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|SourceKey|string|Yes|The name of the source field.|
|Regex|string|Yes|The regular expression. The field values to be extracted are enclosed in parentheses \(\).|
|Keys|String array|Yes|The array of fields to be extracted, for example, \["key1", "key2" ...\].|
|NoKeyError|bool|No|Specifies whether to report an error if a field is not matched. Default value: false. This value indicates that no error is reported if a field is not matched.|
|NoMatchError|bool|No|Indicates whether to report an error if the regular expression does not match the value of a specified field. Default value: false. This value indicates that no error is reported if a field is not matched.|
|KeepSource|bool|No|Specifies whether to retain the raw data. Default value: false. This value indicates that the raw data is not retained.|
|FullMatch|bool|No|Default value: true. This value indicates that exact match is implemented when the regular expression specified in the Regex parameter attempts to match field values. If you set the value to false, partial match is implemented when the regular expression attempts to match field values.|

The following example shows how to configure the processing method to process access logs.

-   Raw data

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


## Anchor-based extraction

This processing method uses the start and stop keywords to anchor strings and extract fields. If an anchored substring is a JSON string, you can expand the substring and then extract the fields.

The type of the plug-in is processor\_anchor. The following table describes the parameters of the processing method.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|SourceKey|string|Yes|The name of the source field.|
|Anchors|Anchor array|Yes|The list of the parameters that are set to anchor strings.|
|NoAnchorError|bool|No|Specifies whether to report an error if no keyword is found. Default value: false. This value indicates that no error is reported if no keyword is found.|
|NoKeyError|bool|No|Specifies whether to report an error if a field is not matched. Default value: false. This value indicates that no error is reported if a field is not matched.|
|KeepSource|bool|No|Specifies whether to retain the raw data. Default value: false. This value indicates that the raw data is not retained.|

The following table describes the parameters of the Anchors parameter.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|Start|string|Yes|The keyword that anchors the start of a substring in a string. If the parameter is not specified, the start of the string is matched.|
|Stop|string|Yes|The keyword that anchors the end of a substring in a string. If the parameter is not specified, the end of the string is matched.|
|FieldName|string|Yes|The name of the field to be extracted.|
|FieldType|string|Yes|The type of the field to be extracted. Valid values: string and json.|
|ExpondJson|bool|No|Specifies whether to expand an anchored JSON substring. Default value: false. This value indicates that an anchored JSON substring is not expanded. This parameter is available only if the value of the FieldType parameter is set to json.|
|ExpondConnecter|string|No|The character that is used to connect expanded keys. Default value: \_.|
|MaxExpondDepth|int|No|The maximum depth of JSON expansion. Default value: 0. This value indicates that the depth of JSON expansion is unlimited.|

The following example shows how to configure the processing method to process logs of mixed field types.

-   Raw data

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


## Single-character delimiter-based extraction

This processing method uses a specified single-character delimiter to delimit fields. You can specify a quote to enclose the delimiter.

The type of the plug-in is processor\_split\_char. The following table describes the parameters of the processing method.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|SourceKey|string|Yes|The name of the source field.|
|SplitSep|string|Yes|The single-character delimiter. You can specify a non-printable character as a single-character delimiter, for example, \\u0001.|
|SplitKeys|String array|Yes|The names of the delimited fields, for example, \["key1", "key2"...\].|
|QuoteFlag|bool|No|Specifies whether to use a quote to enclose the specified delimiter. Default value: false. This value indicates that a quote is not used.|
|Quote|string|No|The quote. The quote must be a single character. You can specify a non-printable character as a quote, for example, \\u0001. This parameter is available only if the value of QuoteFlag is set to true.|
|NoKeyError|bool|No|Specifies whether to report an error if a field is not matched. Default value: false. This value indicates that no error is reported if a field is not matched.|
|NoMatchError|bool|No|Specifies whether to report an error if a delimiter is not matched. Default value: false. This value indicates that no error is reported if a delimiter is not matched.|
|KeepSource|bool|No|Specifies whether to retain the raw data. Default value: false. This value indicates that the raw data is not retained.|

The following example shows how to configure the processing method to process delimited logs.

-   Raw data

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


## Multi-character delimiter-based extraction

This processing method uses a specified multi-character delimiter to delimit fields. You cannot specify a quote to enclose the delimiter.

The type of the plug-in is processor\_split\_string. The following table describes the parameters of the processing method.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|SourceKey|string|Yes|The name of the source field to be extracted.|
|SplitSep|string|Yes|The multi-character delimiter. You can specify multiple non-printable characters as a multi-character delimiter, for example, \\u0001\\u0002.|
|SplitKeys|String array|Yes|The names of the delimited fields, for example, \["key1", "key2"...\].|
|PreserveOthers|bool|No|Specifies whether to retain the excess fields if the number of fields is greater than the number of fields specified by the SplitKeys parameter. Default value: false. This value indicates that the excess fields are not retained.|
|ExpandOthers|bool|No|Specifies whether to parse the excess fields. Default value: false. This value indicates that the excess fields are not parsed.|
|ExpandKeyPrefix|string|No|The prefix of the names of excess fields. For example, if you specify expand\_ for the parameter, the first two excess fields are named expand\_1 and expand\_2.|
|NoKeyError|bool|No|Specifies whether to report an error if a field is not matched. Default value: false. This value indicates that no error is reported if a field is not matched.|
|NoMatchError|bool|No|Specifies whether to report an error if a delimiter is not matched. Default value: false. This value indicates that no error is reported if a delimiter is not matched.|
|KeepSource|bool|No|Specifies whether to retain the raw data. Default value: false. This value indicates that the raw data is not retained.|

The following example shows how to configure the processing method to process delimited logs.

-   Raw data

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


## GeoIP conversion

This processing method transforms IP addresses in logs into geo locations. The geo locations include the country, province, city, longitude, and latitude.

**Note:** GeoIP databases are not included in the Logtail installation package. You must download and configure a GeoIP database on a local server. We recommend that you download a database that provides the city information of an IP address.

The type of the plug-in is processor\_geoip. The following table describes the parameters of the processing method.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|SourceKey|string|Yes|The name of the source field to be converted.|
|DBPath|string|Yes|The absolute path of the GeoIP database. The database is in the MMDB format, for example, /user/data/GeoLite2-City\_20180102/GeoLite2-City.mmdb.|
|NoKeyError|bool|No|Specifies whether to report an error if a field is not matched. Default value: false. This value indicates that no error is reported if a field is not matched.|
|NoMatchError|bool|No|Specifies whether to report an error if an IP address is invalid or is not matched in the database. Default value: false. This value indicates that no error is reported if an IP address is invalid or is not matched in the database.|
|KeepSource|bool|No|Specifies whether to retain the raw data. Default value: true. This value indicates that the raw data is retained.|
|Language|string|No|The language of the GeoIP database. Default value: zh-CN. Make sure that your GeoIP database can be displayed in a language that is suitable for your business.|

The following example shows how to configure the processing method to process IP addresses in logs.

-   Raw data

    ```
    "source_ip" : "**. **. **. **"
    ```

    Download a GeoIP database and install the database on the server where Logtail resides. We recommend that you download a city database of [MaxMind GeoLite2](https://dev.maxmind.com/geoip/geoip2/geolite2/).

    **Note:** Make sure that the database format is MMDB.

-   Logtail configurations for data processing

    ```
    {
       "type": "processor_geoip",
        "detail": {
             "SourceKey": "ip",
             "NoKeyError": true,
             "NoMatchError": true,
             "KeepSource": true,
             "DBPath" : "/user/local/data/GeoLite2-City_20180102/GeoLite2-City.mmdb"
        }
    }
    ```

-   Result

    ```
    "source_ip_city_" : "**. **. **.**"
    "source_ip_province_" : "Zhejiang"
    "source_ip_city_" : "Hangzhou"
    "source_ip_province_code_" : "ZJ"
    "source_ip_country_code_" : "CN"
    "source_ip_longitude_" : "120.********"
    "source_ip_latitude_" : "30. ********"
    ```


## Regular expression-based filter

This processing method uses regular expressions to filter logs.

The type of the plug-in is processor\_filter\_regex. The following table describes the parameters of the processing method.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|Include|The keys that are specified in this parameter are strings. The values of these keys are JSON objects.|No|A key is the name of a log field. A value is specified by a regular expression. If a key-value pair matches one of the fields of a log entry, the log entry is collected.|
|Exclude|The keys that are specified in this parameter are strings. The values of these keys are JSON objects.|No|A key is the name of a log field. A value is specified by a regular expression. If a key-value pair matches one of the fields of a log entry, the log entry is dropped.|

**Note:** A log entry is collected only when it exactly matches the regular expression specified in the Include parameter and does not match the regular expression specified in the Exclude parameter.

The following example shows how to configure the processing method to process logs.

-   Raw data
    -   Log entry 1

        ```
        "ip" : "10. **. **.**"
        "method" : "POST"
        ...
        "browser" : "aliyun-sdk-java"
        ```

    -   Log entry 2

        ```
        "ip" : "10. **. **.**"
        "method" : "POST"
        ...
        "browser" : "chrome"
        ```

    -   Log entry 3

        ```
        "ip" : "192.168. *.*"
        "method" : "POST"
        ...
        "browser" : "ali-sls-ilogtail"
        ```

-   Logtail configurations for data processing

    ```
    {
       "type" : "processor_filter_regex",
        "detail" : {
             "Include" : {
                "ip" : "10\\..*",
                "method" : "POST"
             },
             "Exclude" : {
                "browser" : "aliyun. *"
             }
        }
    }
    ```

-   Result

    |Log entry|Collected|Cause|
    |:--------|:--------|:----|
    |Log entry 1|No|The value of the browser field matches the regular expression specified in the Exclude parameter.|
    |Log entry 2|Yes|N/A|
    |Log entry 3|No|The value of the ip field does not match the regular expression specified in the Include parameter.|


## Field enrichment

This processing method adds fields to a log entry.

The type of the plug-in is processor\_add\_fields. The following table describes the parameters of the processing method.

**Note:** Only Logtail 0.16.28 or later supports the plug-in.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|Fields|map|No|The JSON object that includes key-value pairs. You can specify multiple key-value pairs in the parameter.|
|IgnoreIfExist|bool|No|Specifies whether to retain key-value pairs that have the same key. Default value: false. This value indicates that a key-value pair is not retained if the key is the same as another specified key.|

The following example shows how to add the aaa2 and aaa3 fields to a log entry.

-   Raw data

    ```
    "aaa1":"value1"
    ```

-   Logtail configurations for data processing

    ```
    {
      "processors":[
        {
          "type":"processor_add_fields",
          "detail": {
            "Fields": {
              "aaa2": "value2",
              "aaa3": "value3"
            }
          }
        }
      ]
    }
    ```

-   Result

    ```
    "aaa1":"value1"
    "aaa2":"value2"
    "aaa3":"value3"
    ```


## Field dropping

This processing method drops specified fields from a log entry.

The type of the plug-in is processor\_drop. The following table describes the parameters of the processing method.

**Note:** Only Logtail 0.16.28 or later supports the plug-in.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|DropKeys|array|No|The array that includes a set of strings. You can drop one or more fields of a log entry.|

The following example shows how to drop the aaa1 and aaa2 fields from a log entry.

-   Raw data

    ```
    "aaa1":"value1"
    "aaa2":"value2"
    "aaa3":"value3"
    ```

-   Logtail configurations for data processing

    ```
    {
      "processors":[
        {
          "type":"processor_drop",
          "detail": {
            "DropKeys": ["aaa1","aaa2"]
          }
        }
      ]
    }
    ```

-   Result

    ```
    "aaa3":"value3"
    ```


## Log time extraction \(Go\)

This processing method extracts the time information from a specified field, converts the time format, and then includes the time information in a destination field. For more information about how to extract and convert time information, see [Golang Time Format](https://golang.org/pkg/time/)。

The type of the plug-in is processor\_gotime. The following table describes the parameters of the processing method.

**Note:** Only Logtail 0.16.28 or later supports the plug-in.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|SourceKey|string|Yes|The name of the source field from which time information is extracted. If the parameter is not specified, the parameter does not take effect.|
|SourceFormat|string|Yes|The format of the time information in the source field.|
|SourceLocation|int|Yes|The source time zone. If the parameter is not specified, the current time zone of the server where Logtail is installed takes effect.|
|DestKey|string|Yes|The name of the destination field. If the parameter is not specified, the parameter does not take effect.|
|DestFormat|string|Yes|The format of the time information in the destination field. The time is in the Go time format.|
|DestLocation|int|No|The destination time zone. If the parameter is not specified, the current time zone of the server where Logtail is installed takes effect.|
|SetTime|bool|No|Specifies whether to set the time field. Default value: true. This value indicates that the time field is set.|
|KeepSource|bool|No|Specifies whether to retain the source field. Default value: true. This value indicates that the source field is retained.|
|NoKeyError|bool|No|Specifies whether to report an error if a field is not matched. Default value: true. This value indicates that an error is reported if the source field is not matched.|
|AlarmIfFail|bool|No|Specifies whether to trigger an alert if the time information fails to be extracted. Default value: true. This value indicates that an alert is triggered if the time information fails to be extracted.|

The following example shows how to configure the processing method to process time information. The time information 2006-01-02 15:04:05 \(UTC+8\) is extracted from the s\_key field, converted into 2006/01/02 15:04:05 \(UTC+9\), and then included in the d\_key field.

-   Raw data

    ```
    "s_key":"2019-07-05 19:28:01"
    ```

-   Logtail configurations for data processing

    ```
    {
      "processors":[
        {
          "type":"processor_gotime",
          "detail": {
            "SourceKey": "s_key",
            "SourceFormat":"2006-01-02 15:04:05",
            "SourceLocation":8,
            "DestKey":"d_key",
            "DestFormat":"2006/01/02 15:04:05",
            "DestLocation":9,
            "SetTime": true,
            "KeepSource": true,
            "NoKeyError": true,
            "AlarmIfFail": true
          }
        }
      ]
    }
    ```

-   Result

    ```
    "s_key":"2019-07-05 19:28:01"
    "d_key":"2019/07/05 20:28:01"
    ```


## JSON expansion

This processing method expands specified fields from JSON objects.

The type of the plug-in is processor\_json.

**Note:** Only Logtail 0.16.28 or later supports the plug-in.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|SourceKey|string|Yes|The source field to be expanded from a JSON object.|
|NoKeyError|bool|No|Specifies whether to report an error if the source field is not matched. Default value: true. This value indicates that an error is reported if the source field is not matched.|
|ExpandDepth|int|No|The depth of JSON expansion. Default value: 0. This value indicates the depth of JSON expansion is unlimited. If the value is n, the number of depths to expand is n.|
|ExpandConnector|string|No|The character that is used to connect expanded keys. Default value: \_. The null value is supported.|
|Prefix|string|No|The prefix that is appended to the expanded keys.|
|KeepSource|bool|No|Specifies whether to retain the source field. Default value: true. This value indicates that the source field is retained.|
|UseSourceKeyAsPrefix|bool|No|Specifies whether to append the source field name as a prefix to all expanded keys. Default value: false. This value indicates that the source field name is not appended.|

The following example shows how to expand the s\_key field from a JSON object.

-   Raw data

    ```
    "s_key":"{\"k1\":{\"k2\":{\"k3\":{\"k4\":{\"k51\":\"51\",\"k52\":\"52\"},\"k41\":\"41\"}}})"
    ```

-   Logtail configurations for data processing

    ```
    {
      "processors":[
        {
          "type":"processor_json",
          "detail": {
            "SourceKey": "s_key",
            "NoKeyError":true,
            "ExpandDepth":0,
            "ExpandConnector":"-",
            "Prefix":"j",
            "KeepSource": false,
            "UseSourceKeyAsPrefix": true
          }
        }
      ]
    }
    ```

-   Result

    ```
    "s_key":"{\"k1\":{\"k2\":{\"k3\":{\"k4\":{\"k51\":\"51\",\"k52\":\"52\"},\"k41\":\"41\"}}})"
    "js_key-k1-k2-k3-k4-k51":"51"
    "js_key-k1-k2-k3-k4-k52":"52"
    "js_key-k1-k2-k3-k41":"41"
    ```


## Field encapsulation

This processing method encapsulates one or more fields into JSON-formatted fields.

The type of the plug-in is processor\_packjson.

**Note:** Only Logtail 0.16.28 or later supports the plug-in.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|SourceKeys|array|Yes|The string array-formatted field to be encapsulated.|
|DestKey|string|No|The destination JSON-formatted field.|
|KeepSource|bool|No|Specifies whether to retain the source field. Default value: true. This value indicates that the source field is retained.|
|AlarmIfIncomplete|bool|No|Specifies whether to trigger an alert if the source field is not found. Default value: true. This value indicates that an error is reported if the source field is not found.|

The following example shows how to encapsulate the a and b fields into the d\_key field.

-   Raw data

    ```
    "a":"1"
    "b":"2"
    ```

-   Logtail configurations for data processing

    ```
    {
      "processors":[
        {
          "type":"processor_packjson",
          "detail": {
            "SourceKeys": ["a","b"],
            "DestKey":"d_key",
            "KeepSource":true,
            "AlarmIfEmpty":true
          }
        }
      ]
    }
    ```

-   Result

    ```
    "a":"1"
    "b":"2"
    "d_key":"{\"a\":\"1\",\"b\":\"2\"}"
    ```


## Field renaming

This processing method renames one or more specified fields.

The type of the plug-in is processor\_rename.

**Note:** Only Logtail 0.16.28 or later supports the plug-in.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|NoKeyError|bool|Yes|Specifies whether to report an error if a field to be renamed is not matched. Default value: false. This value indicates that no error is reported if a field to be renamed is not matched.|
|SourceKeys|array|Yes|The array of source fields to be renamed.|
|DestKeys|array|Yes|The array of renamed fields.|

The following example shows how to rename the aaa1 field into bbb1 and the aaa2 field into bbb2.

-   Raw data

    ```
    "aaa1":"value1"
    "aaa2":"value2"
    "aaa3":"value3"
    ```

-   Logtail configurations for data processing

    ```
    {
      "processors":[
        {
          "type":"processor_rename",
          "detail": {
            "SourceKeys": ["aaa1","aaa2"],
            "DestKeys": ["bbb1","bbb2"],
            "NoKeyError": true
          }
        }
      ]
    }
    ```

-   Result

    ```
    "bbb1":"value1"
    "bbb2":"value2"
    "aaa3":"value3"
    ```


## Log time extraction \(Strptime\)

This processing method extracts time from a field and parses the time by using the [Linux strptime\(\)](http://man7.org/linux/man-pages/man3/strptime.3.html) function.

The type of the plug-in is processor\_strptime.

**Note:** Only Logtail 0.16.28 or later supports the plug-in.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|SourceKey|string|Yes|The name of the field from which time is extracted.|
|Format|string|Yes|The time format that is used to parse the extracted time.|
|AdjustUTCOffset|bool|No|Specifies whether to modify the time zone. Default value: false. This value indicates that the time zone is not modified.|
|UTCOffset|int|No|The offset that is used to modify the time zone. Unit: seconds. For example, the value 28800 indicates that the time zone is modified to UTC+8.|
|AlarmIfFail|bool|No|Specifies whether to trigger an alert if the time information fails to be extracted. Default value: true. This value indicates that an alert is triggered if the time information fails to be extracted.|
|KeepSource|bool|No|Specifies whether to retain the source field. Default value: true. This value indicates that the source field is retained.|

The following examples show how to parse the value of the log\_time filed into the `%Y/%m/%d %H:%M:%S` format.

-   Example 1: The time zone is UTC+8.
    -   Raw data

        ```
        "log_time":"2016/01/02 12:59:59"
        ```

    -   Logtail configurations for data processing

        ```
        {
          "processors":[
            {
              "type":"processor_strptime",
              "detail": {
                "SourceKey": "log_time",
                "Format": "%Y/%m/%d %H:%M:%S"
              }
            }
          ]
        }
        ```

    -   Result

        ```
        "log_time":"2016/01/02 12:59:59"
        Log.Time = 1451710799
        ```

-   Example 2: The time zone is UTC+7.
    -   Raw data

        ```
        "log_time":"2016/01/02 12:59:59"
        ```

    -   Logtail configurations for data processing

        ```
        {
          "processors":[
            {
              "type":"processor_strptime",
              "detail": {
                "SourceKey": "log_time",
                "Format": "%Y/%m/%d %H:%M:%S",
                "AdjustUTCOffset": true,
                "UTCOffset": 25200
              }
            }
          ]
        }
        ```

    -   Result

        ```
        "log_time":"2016/01/02 12:59:59"
        Log.Time = 1451714399
        ```


## Custom methods

You can use multiple processing methods to process logs. The following example shows how to use a single-character delimiter to delimit a log entry into several fields and then specify anchor points to extract fields from the `detail` field.

-   Raw data

    ```
    "content" : 
    "ACCESS|QAS|11. **. **.**|1508729889935|52460dbed4d540b88a973cf5452b1447|1238|appKey=ba,env=pub,requestTime=1508729889913,latency=22ms,
    request={appKey:ba,optional:{\\domains\\:\\daily\\,\\version\\:\\v2\\},rawQuery:{\\query\\:\\The route to Location A\\,\\domain\\:\\Navigation\\,\\intent\\:\\navigate\\,\\slots\\:\\to_geo:level3=Location A\\,\\location\\:\\Location B\\},
    requestId:52460dbed4d540b88a973cf5452b1447},
    response={answers:[],status:SUCCESS}|"
    ```

-   Logtail configurations for data processing

    ```
    "processors" : [
          {
              "type" : "processor_split_char",
              "detail" : {"SourceKey" : "content",
                  "SplitSep" : "|",
                  "SplitKeys" : ["method", "type", "ip", "time", "req_id", "size", "detail"]
              }
          },
          {
              "type" : "processor_anchor",
              "detail" : "SourceKey" : "detail",
                  "Anchors" : [
                      {
                              "Start" : "appKey=",
                          "Stop" : ",env=",
                          "FieldName" : "appKey",
                          "FieldType" : "string"
                      },
                      {
                          "Start" : ",env",
                          "Stop" : ",requestTime=",
                          "FieldName" : "env",
                          "FieldType" : "string"
                      },
                      {
                          "Start" : ",requestTime=",
                          "Stop" : ",latency",
                          "FieldName" : "requestTime",
                          "FieldType" : "string"
                      },
                      {
                          "Start" : ",latency=",
                          "Stop" : ",request=",
                          "FieldName" : "latency",
                          "FieldType" : "string"
                      },
                      {
                          "Start" : ",request=",
                          "Stop" : ",response=",
                          "FieldName" : "request",
                          "FieldType" : "string"
                      },
                      {
                          "Start" : ",response=",
                          "Stop" : "",
                          "FieldName" : "response",
                          "FieldType" : "json"
                      }
                  ]
              }
          }
      ]
    ```

-   Result

    ```
    "method" : "ACCESS"
    "type" : "QAS"
    "ip" : "**. **. **.**"
    "time" : "1508729889935"
    "req_id" : "52460dbed4d540b88a973cf5452b1447"
    "size" : "1238"
    "appKey" : "ba"  
    "env" : "pub"
    "requestTime" : "1508729889913"
    "latency" : "22ms"
    "request" : "{appKey:nui-banma,optional:{\\domains\\:\\daily-faq\\,\\version\\:\\v2\\},rawQuery:{\\query\\:\\\345\216\273\344\271\220\345\261\261\347\232\204\350\267\257\347\272\277\\,\\domain\\:\\\345\257\274\350\210\252\\,\\intent\\:\\navigate\\,\\slots\\:\\to_geo:level3=\344\271\220\345\261\261\\,\\location\\:\\\345\214\227\344\272\254\\},requestId:52460dbed4d540b88a973cf5452b1447}"  
    "response_answers" : "[]"
    "response_status" : "SUCCESS"
    ```


