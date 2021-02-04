# GetLogs

Queries logs in a Logstore of a specified project.

## Description

-   The time ranges that are used in this operation are left-closed and right-open intervals. Each interval contains the specified start time, but does not contain the specified end time. The intervals are defined by the from and to parameters. If you specify the from and to parameters to the same value, the time range is invalid and an error message is returned.
-   If the number of logs in the Logstore greatly changes, Log Service cannot predict how many times this API operation needs to be called to obtain complete results. In this case, you must check the value of the x-log-progress parameter in the returned results of each request. Based on the value, you can decide whether to call this operation again to obtain complete results. Each time you call this operation, the same number of charge units \(CUs\) are consumed.
-   After a log is written to a Logstore, you can call the GetHistograms and GetLogs operations to query the log. However, the query has a latency that varies with the type of the log. Based on the log timestamp, Log Service classifies logs into the following two types:

    -   Real-time data: Logs of this type are generated within the interval of \(-180 seconds, 900 seconds\] based on the current server time. For example, if a log was generated at September 25, 2014, 12:00:00 UTC and the server received the log at September 25, 2014, 12:05:00 UTC, the server processes the log as real-time data. Such logs are generated in standard query scenarios.
    -   Historical data: Logs of this type are generated within the interval of \[-604,800 seconds, -180 seconds\) based on the current server time. For example, if a log was generated at September 25, 2014, 12:00:00 UTC and the server received the log at September 25, 2014, 12:05:00 UTC, the server processes the log as historical data. Such logs are generated in data supplement scenarios.
    After real-time data is written to a Logstore, the data can be queried with a maximum latency of 3 seconds. You can query 99.9% of real-time data within 1 second.


## Request syntax

```
GET /logstores/logstoreName? type=log&topic=topic&from=from&to=to&query=query&line=line&offset=offset&reverse=reverse HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
Date: GMT Date
Host: ProjectName.Endpoint
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The GetLogs operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|big-game|The name of the project.|
    |logstoreName|String|Yes|app\_log|The name of the Logstore.|
    |type|String|Yes|log|The type of data in the Logstore. This parameter must be set to log in the GetLogs operation.|
    |from|Integer|Yes|1409529600|The start time of the time range that is specified in the request. The start time is a timestamp. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|
    |to|Integer|Yes|1409608800|The end time of the time range that is specified in the request. The end time is a timestamp. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|
    |topic|String|No|groupA|The topic of the logs.|
    |query|String|No|error|The query statement. For more information, see [Real-time log analysis](/intl.en-US/Index and query/Real-time log analysis.md).|
    |line|Integer|No|20|The maximum number of log entries to return for the request. Minimum value: 0. Maximum value: 100. Default value: 100.|
    |offset|Integer|No|0|The line from which the query starts. Default value: 0.|
    |reverse|Boolean|No|false|Specifies whether to return logs in reverse order based on the log timestamp. Unit: minutes. Valid values: true and false. Default value: false.    -   true: Logs are returned in reverse order.
    -   false: Logs are returned in regular order. |


## Response parameters

-   Response headers

    The GetLogs operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

    The response header contains specific parameters that indicate whether the query results are complete. The following table describes the specific response parameters.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |x-log-progress|String|Complete|The status of the query results. Valid values: Complete and Incomplete.    -   Complete: The query succeeded and the query results are complete.
    -   Incomplete: The query succeeded but the query results are incomplete. You must repeat the request to obtain complete query results. |
    |x-log-count|Integer|10000|The total number of log entries in the query results.|

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. The response body contains the matched logs. If a large number of log entries \(measured in TBs\) are queried, the query results may be incomplete. The response body of the GetLogs operation is an array that contains the information of each log entry. The following table describes the parameters of the array.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |\_\_time\_\_|Integer|1409529660|The timestamp of the log. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|
    |\_\_source\_\_|String|192.168.1.100|The log source, which is specified when logs are written. For example, the IP address of the machine where the log is generated.|
    |contents|List|\["Key1": "error","Key2": "Value2"\]|The original content of logs.|


## Examples

In this example, the logs whose topic is groupA are queried in a Logstore named app\_log. The Logstore belongs to a project named big-game in the China \(Hangzhou\) region. The time range is from September 1, 2014, 00:00:00 to September 1, 2014, 22:00:00 and the keyword is "error". The query starts from the beginning of the time range. A maximum of 20 log entries can be returned.

-   Sample requests

    ```
    GET /logstores/app_log? type=log&topic=groupA&from=1409529600&to=1409608800&query=error&line=20&offset=0 HTTP/1.1
    Header :
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    Date: Wed, 3 Sept. 2014 08:33:46 GMT
    Host: big-game.cn-hangzhou.log.aliyuncs.com
    x-log-bodyrawsize: 0
    x-log-apiversion: 0.4.0
    x-log-signaturemethod: hmac-sha1
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header :
    {
    Content-MD5: 36F9F7F0339BEAF571581AF1B0AAAFB5
    Content-Type: application/json
    Content-Length: 269
    Date: Wed, 3 Sept. 2014 08:33:47 GMT
    x-log-requestid: efag01234-12341-15432f
    x-log-progress : Complete
    x-log-count : 10000
    x-log-processed-rows: 10000
    x-log-elapsed-millisecond:5
    }
    Body :
    {
        "progress": "Complete",
        "count": 2,
        "logs": [
            {
                "__time__": 1409529660,
                "__source__": "192.168.1.100",
                "contents": ["Key1": "error","Key2": "Value2"]
            },
            {
                "__time__": 1409529680,
                "__source__": "192.168.1.100",
                "contents": ["Key3": "error","Key4": "Value4"]
            }
        ]
    }
    ```


In the sample response, the value of the x-log-progress parameter is Complete. This indicates that all logs are queried and the returned results are complete. Two log entries meet the specified query conditions. The log entries are displayed as the value of the logs parameter. If the value of the x-log-progress parameter is Incomplete, you must repeat the request to obtain complete query results.

## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|:----------|
|404|ProjectNotExist|Project ProjectName does not exist.|The error message returned because the specified project does not exist.|
|404|LogStoreNotExist|logstore logstoreName does not exist.|The error message returned because the specified Logstore does not exist.|
|400|InvalidTimeRange|Request time range is invalid.|The error message returned because the specified time interval of the request is invalid.|
|400|InvalidQueryString|Query string is invalid.|The error message returned because the specified query string of the request is invalid.|
|400|InvalidOffset|Offset is invalid.|The error message returned because the specified offset parameter of the request is invalid.|
|400|InvalidLine|Line is invalid.|The error message returned because the specified line parameter of the request is invalid.|
|400|InvalidReverse|Reverse value is invalid.|The error message returned because the specified reverse parameter is invalid.|
|400|IndexConfigNotExist|Logstore without index config.|The error message returned because the indexing feature is not enabled for the Logstore.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

