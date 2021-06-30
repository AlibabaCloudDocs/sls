# PutWebtracking

Writes multiple log entries to Log Service in one request.

## Description

-   You can call this API operation to collect logs from web pages or clients.
-   If you use web tracking to collect logs, you can send only one log entry to Log Service in each request. For more information, see [Use web tracking to collect logs](/intl.en-US/Data Collection/Other collection methods/Use web tracking to collect logs.md).
-   In scenarios that involve a large volume of log data, you can call the PutWebTracking operation to write multiple log entries to Log Service in one request.
-   Before you can call the PutWebTracking API operation to write multiple log entries to a Logstore, you must turn on the WebTracking switch for the Logstore. For more information, see [Use web tracking to collect logs](/intl.en-US/Data Collection/Other collection methods/Use web tracking to collect logs.md).
-   You cannot use this operation to write log entries of multiple topics to Log Service at the same time.

## Request syntax

```
POST ProjectName.Endpoint/logstores/logstoreName/track HTTP/1.1

x-log-apiversion: 0.6.0
x-log-bodyrawsize: 1234
x-log-compresstype: lz4

{
  "__topic__": "topic",
  "__source__": "source",
  "__logs__": [
    {
      "key1": "value1",
      "key2": "value2"
    },
    {
      "key1": "value1",
      "key2": "value2"
    }
  ],
  "__tags__": {
    "tag1": "value1",
    "tag2": "value2"
  }
}
```

## Request parameters

-   Request headers

    The following three request headers are supported. The first two request headers are required when you call the PutWebTracking operation. For more information, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

    -   `x-log-apiversion: 0.6.0`
    -   `x-log-bodyrawsize: 1234`
    -   `x-log-compresstype: lz4`
    If you do not need to send compressed data, the x-log-compresstype header is not required. If you need to send compressed data, you must set the x-log-compresstype header to lz4 or deflate. The lz4 value indicates the LZ4 algorithm and the deflate value indicates the DEFLATE algorithm. The request header that applies each of the two algorithms is in the format of `x-log-compresstype: lz4` and `x-log-compresstype: deflate`.

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |---------|----|--------|-------|-----------|
    |projectName|String|Yes|my-project|The name of the project.|
    |logstoreName|String|Yes|logstore-test|The name of the Logstore.|
    |endpoint|String|Yes|cn-hangzhou.log.aliyuncs.com|The endpoint of Log Service. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|
    |\_\_topic\_\_|String|No|topic|The topic of the log entries.|
    |\_\_source\_\_|String|No|source|The source of the log entries.|
    |\_\_logs\_\_|List|Yes|\[\{"key1": value1", "key2": "value2"\},\{"key1": "value1","key2": "value2"\}\]|The content of the log entries. Each element is a JSON object that indicates a log entry. **Note:** The time in a log entry that is collected by using the WebTracking feature is the time at which Log Service receives the log entry. You do not need to configure the \_\_time\_\_ field for each log entry. If this field exists, it is overwritten by the time at which Log Service receives the log entry. |
    |\_\_tags\_\_|Object|No|\{"tag1": "value1", "tag2": "value2" \}|The tags of the log entries.|


## Response parameters

-   Response headers

    The PutWebTracking operation does not have operation-specific response headers. For information about common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. If the request is successful, no other elements are returned.


## Examples

-   Sample requests

    ```
    POST my-project.cn-hangzhou.log.aliyuncs.com/logstores/logstore-test/track HTTP/1.1
    Header :
    {
    x-log-apiversion: 0.6.0
    x-log-bodyrawsize: 1234
    x-log-compresstype: lz4
    }
    Body :
    {
      "__topic__": "topic",
      "__source__": "source",
      "__logs__": [
        {
          "foo": "bar",
          "foo1": "bar1"
        },
        {
          "bar": "foo",
          "bar1": "foo1"
        }
      ],
      "__tags__": {
        "tag1": "value1",
        "tag2": "value2"
      }
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header:
    {
        'Date': 'Wed, 20 May 2019 07:35:00 GMT', 
        'Content-Length': '0', 
        'x-log-requestid': '5642EFA499248C827B012B39', 
        'Connection': 'close', 
        'Server': 'nginx/1.6.1'
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|:----------|
|400|PostBodyInvalid|Protobuffer content cannot be parsed.|The error message returned because the log entries in the Protocol Buffer format are invalid and cannot be parsed.|
|400|InvalidTimestamp|Invalid timestamps are in logs.|The error message returned because the log entries contain invalid timestamps.|
|400|InvalidEncoding|Non-UTF8 characters are in logs.|The error message returned because the log entries contain non-UTF-8 characters.|
|400|InvalidKey|Invalid keys are in logs.|The error message returned because the log entries contain invalid keys.|
|400|PostBodyTooLarge|Logs must be less than or equal to 3 MB and 4096 entries.|The error message returned because the size of log entries is greater than 3 MB or the number of log entries exceeds 4,096.|
|400|PostBodyUncompressError|Failed to decompress logs.|The error message returned because the log entries failed to be decompressed.|
|499|PostBodyInvalid|The post data time is out of range.|The error message returned because the log timestamp is not within the valid time range: \[-7 × 24 hours, + 15 minutes\].|
|404|LogStoreNotExist|logstore logstoreName does not exist.|The error message returned because the specified Logstore does not exist.|
|404|ProjectNotExist|The Project does not exist : projectName.|The error message returned because the specified project does not exist.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

