# GetLogs

Queries logs in a Logstore of a specified project.

## Description

-   The time ranges that are used in this operation are left-closed and right-open intervals. Each interval includes the specified start time, but does not include the specified end time. These intervals are specified by the from and to parameters in the request. If you set the from and to parameters to the same value, the time range is invalid and an error message is returned.
-   If the number of logs in a Logstore significantly changes, Log Service cannot predict the number of times you must call this API operation to obtain complete results. In this case, you must check the value of the x-log-progress parameter in the returned results of each request. You can also call this API operation again to obtain the complete results based on the value of this parameter. Each time you call this operation, the same number of charge units \(CUs\) are consumed.
-   After a log is written to a Logstore, you can call the GetHistograms and GetLogs operations to query the log. The latency of the query varies based on the type of the log. Log Service classifies logs into the following two types based on the log timestamp:

    -   Real-time data: Logs of this type are generated within the interval of \(-180 seconds, 900 seconds\] based on the current server time. For example, if a log was generated at 12:03:00 UTC, September 25, 2014 and the server received the log at 12:05:00 UTC, September 25, 2014, the server processes the log as real-time data. Logs of this type are generated in standard query scenarios.
    -   Historical data: Logs of this type are generated within the interval of \[-604,800 seconds, -180 seconds\) based on the current server time. For example, if a log was generated at 12:00:00 UTC, September 25, 2014 and the server received the log at 12:05:00 UTC, September 25, 2014, the server processes the log as historical data. Logs of this type are generated in data supplement scenarios.
    After real-time data is written to a Logstore, the data can be queried with a maximum latency of 3 seconds. You can query 99.9% of real-time data within 1 second.


## Request syntax

```
GET /logstores/logstoreName?type=log&topic=topic&from=from&to=to&query=query&line=line&offset=offset&reverse=reverse HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
Date: GMT Date
Host: ProjectName.Endpoint
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

The value of the Host parameter consists of a project name and Log Service endpoint. You must specify a project in the Host parameter.

## Request parameters

-   Request headers

    The GetLogs operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|big-game|The name of the project.|
    |logstoreName|String|Yes|app\_log|The name of the Logstore.|
    |type|String|Yes|log|The type of the data in the Logstore. This parameter must be set to log in the GetLogs operation.|
    |from|Integer|Yes|1409529600|The start time of the time range that is specified in the request. The start time is a timestamp. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 UTC, Thursday, January 1, 1970.|
    |to|Integer|Yes|1409608800|The end time of the time range that is specified in the request. The end time is a timestamp. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 UTC, Thursday, January 1, 1970.|
    |topic|String|No|groupA|The topic of the logs.|
    |query|String|No|`level:Information|select event_id as Key1,COUNT(*) as Key2 group by Key1`|The query statement. For more information, see [Log search](/intl.en-US/Index and query/Log search.md) and [Log analysis](/intl.en-US/Index and query/Log analysis.md). If you add `set session parallel_sql=true;` to the analytic statement in the query parameter, a dedicated SQL instance is used. For example, you can set the query parameter to `* | set session parallel_sql=true; select count(*) as pv`.

**Note:** If the query parameter contains an analytic statement \(SQL statement\), the line parameter and offset parameter must be set to 0, and the LIMIT clause is used to paginate the query results. For more information, see [Display analysis results on multiple pages](/intl.en-US/Index and query/Best practices/Display query and analysis results on multiple pages.md). |
    |line|Integer|No|20|The maximum number of logs to return for the request. Minimum value: 0. Maximum value: 100. Default value: 100.|
    |offset|Integer|No|0|The line from which the query starts. Default value: 0.|
    |reverse|Boolean|No|false|Specifies whether to return logs in reverse order based on the log timestamp. Unit: minutes.     -   true: Logs are returned in reverse order.
    -   false: Logs are returned in regular order. This is the default value. |
    |powerSql|Boolean|No|false|Specifies whether to use a dedicated SQL instance. For more information, see [Enable a dedicated SQL instance]().     -   true: A dedicated SQL instance is used.
    -   false: A standard SQL instance is used. This is the default value.
You can use the powerSql or query parameter to configure a dedicated SQL instance. |


## Response parameters

-   Response headers

    The GetLogs operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

    The response header contains parameters that indicate whether the query results are complete. The following table describes the perameters in the response header.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |x-log-progress|String|Complete|The status of the query results. Valid values:    -   Complete: The query is successful and the query results are complete.
    -   Incomplete: The query is successful but the query results are incomplete because the amount of returned data exceeds the limit and the specified time range is invalid. You must repeat the request to obtain complete query results. |
    |x-log-count|Integer|10000|The total number of logs in the query results.|

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. The response body contains the matched logs. If a large amount of log data that is measured in terabytes is queried, the query results may be incomplete. The response body of the GetLogs operation is an array that contains the information of each log. The following table describes the parameters of the array.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |\_\_time\_\_|Integer|1409529660|The timestamp of the log. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 UTC, Thursday, January 1, 1970.|
    |\_\_source\_\_|String|192.168.1.100|The log source that is specified when logs are written. For example, the source can be the IP address of the machine where the log is generated.|
    |contents|List|\["Key1": "error","Key2": "Value2"\]|The original content of logs.|


## Examples

In this example, the logs whose topic is groupA are queried in a Logstore named app\_log. The Logstore belongs to a project named big-game in the China \(Hangzhou\) region. The time range is from 00:00:00 UTC, September 1, 2014 to 22:00:00 UTC, September 1, 2014, and the keyword is "error". The query statement is `level:Information|select event_id as Key1,COUNT(*) as Key2 group by Key1`. The query starts from the start of the time range. A maximum of 20 logs can be returned.

-   Sample requests

    ```
    GET /logstores/app_log?type=log&topic=groupA&from=1409529600&to=1409608800&query="level:Information|select event_id as Key1,COUNT(*) as Key2 group by Key1"&line=20&offset=0 HTTP/1.1
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
    x-log-requestid: 60AC5164947E71A31F55D8D0
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


In the sample response, the value of the x-log-progress parameter is Complete. This indicates that all logs are queried and the returned results are complete. Two logs meet the specified query conditions. The logs are displayed as the value of the logs parameter. If the value of the x-log-progress parameter in the response is Incomplete, you must repeat the request to obtain complete query results.

## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|:----------|
|404|ProjectNotExist|Project ProjectName does not exist.|The error message returned because the specified project does not exist.|
|404|LogStoreNotExist|logstore logstoreName does not exist.|The error message returned because the specified Logstore does not exist.|
|400|InvalidTimeRange|Request time range is invalid.|The error message returned because the specified time range is invalid.|
|400|InvalidQueryString|Query string is invalid.|The error message returned because the specified query string in the request is invalid.|
|400|InvalidOffset|Offset is invalid.|The error message returned because the specified offset parameter in the request is invalid.|
|400|InvalidLine|Line is invalid.|The error message returned because the specified line parameter in the request is invalid.|
|400|InvalidReverse|Reverse value is invalid.|The error message returned because the specified reverse parameter is invalid.|
|400|IndexConfigNotExist|Logstore without index config.|The error message returned because the indexing feature is not enabled for the Logstore.|
|400|ParameterInvalid|ErrorType:OLSQueryParseError.ErrorMessage:offset is not available for pagination in sql query, please use limit x,y syntax for pagination.|-   If the query parameter contains an analytic statement \(SQL statement\), the line parameter and offset parameter must be set to 0, and the LIMIT clause is used to paginate the query results.
-   If the SQL statement in the query parameter is invalid, check the SQL statement and modify it as needed, and then try again. |

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

