# GetHistograms

Queries the log distribution of a Logstore in a specified project. You can also query the distribution of logs that meet specified conditions.

## Description

-   The time ranges that are used in this operation are left-closed and right-open intervals. Each interval contains the specified start time but does not contain the specified end time. The intervals include the time ranges that are defined by the from and to parameters in the request and the subintervals in the response. If you specify the from and to parameters to the same value, the time range is invalid and an error message is returned.
-   The time range is evenly divided into subintervals in the response. If the time range that is specified in the request remains unchanged, the subintervals in the response also remain unchanged.
-   If the number of logs in the Logstore greatly changes, Log Service cannot predict how many times this API operation needs to be called to obtain complete results. In this case, you must check the value of the progress parameter in the returned results of each request. Based on the value, you can decide whether to call this API operation again to obtain complete results. Each time you call this operation, the same number of charge units \(CUs\) are consumed.
-   After a log is written to a Logstore, you can call the GetHistograms and GetLogs operations to query the log. However, the query has a latency that varies with the type of the log. Based on the log timestamp, Log Service classifies logs into the following two types:
    -   Real-time data: Logs of this type are generated within the interval of \(-180 seconds, 900 seconds\] based on the current server time. For example, if a log was generated at September 25, 2014, 12:00:00 UTC and the server received the log at September 25, 2014, 12:05:00 UTC, the server processes the log as real-time data. Such logs are generated in standard query scenarios. After real-time data is written to a Logstore, the data can be queried with a latency of three seconds.
    -   Historical data: Logs of this type are generated within the interval of \[-604,800 seconds, -180 seconds\) based on the current server time. For example, if a log was generated at September 25, 2014, 12:00:00 UTC and the server received the log at September 25, 2014, 12:05:00 UTC, the server processes the log as historical data. Such logs are generated in data supplement scenarios.

## Request syntax

```
GET /logstores/logstoreName? type=histogram&topic=topic&from=from&to=to&query=query HTTP/1.1
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

    The GetHistograms operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|big-game|The name of the project.|
    |logstoreName|String|Yes|app\_log|The name of the Logstore.|
    |type|String|Yes|histogram|The type of data in the Logstore. This parameter must be set to histogram in the GetHistograms operation.|
    |from|Integer|Yes|1409529600|The start time of the time range that is specified in the request. The start time is a timestamp. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|
    |to|Integer|Yes|1409608800|The end time of the time range that is specified in the request. The time is a timestamp. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|
    |topic|String|No|groupA|The topic of the logs.|
    |query|String|No|error|The query statement. For more information about the query syntax, see [Search syntax](/intl.en-US/Index and query/Query/Search syntax.md).|


## Response parameters

-   Response headers

    The GetHistograms operation does not have operation-specific response headers. For information about common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. The response body contains the distribution of the logs along the time axis. Log Service evenly divides the time range into several subintervals and returns the number of logs in each subinterval. The following table describes the parameters of the response body.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |progress|String|Complete|The status of the query results. Valid values: Complete and incomplete.    -   Complete: The query succeeded and the query results are complete.
    -   Incomplete: The query succeeded but the query results are incomplete. You must repeat the request to obtain complete query results. |
    |count|Integer|2|The number of log entries in the query results.|
    |histograms|Array|None|The distribution of the query results in each subinterval.|

    The following table describes the parameters of each element in the histograms array.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |from|Integer|1409529600|The start time of the subinterval. The time is a timestamp. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|
    |to|Integer|1409569200|The end time of the subinterval. The time is a timestamp. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|
    |count|Integer|2|The number of log entries in the subinterval.|
    |progress|Sting|Complete|Indicates whether the query results in the subinterval are complete. Valid values: Complete and Incomplete.    -   Complete: The query succeeded and the query results are complete.
    -   Incomplete: The query succeeded but the query results are incomplete. You must repeat the request to obtain complete query results. |


## Examples

In this example, the distribution of logs whose topic is groupA is queried in a Logstore named app\_log. The Logstore belongs to a project named big-game in the China \(Hangzhou\) region. The time range is from September 1, 2014, 00:00:00 to September 1, 2014, 22:00:00 and the keyword is "error".

-   Sample requests

    ```
    GET /logstores/app_log? type=histogram&topic=groupA&from=1409529600&to=1409608800&query=error HTTP/1.1
    Header :
    {
    Authorization: LOG <yourAccessKeyId>:<yourSignature>
    Date: Wed, 3 Sept. 2014 08:33:46 GMT
    Host: big-game.cn-hangzhou.log.aliyuncs.com
    x-log-bodyrawsize: 0
    x-log-apiversion: 0.6.0
    x-log-signaturemethod: hmac-sha1
    }
    ```

-   Sample responses

    ```
    HTTP/1.1 200 OK
    Header :
    {
    Content-Type: application/json
    Content-MD5: E6AD9C21204868C2DE84EE3808AAA8C8
    Content-Type: application/json
    Date: Wed, 3 Sept. 2014 08:33:47 GMT
    Content-Length: 232
    x-log-requestid: efag01234-12341-15432f
    }
    Body :
    {
            {
                "from": 1409529600,
                "to": 1409569200,
                "count": 2,
                "progress": "Complete"
            },
            {
                "from": 1409569200,
                "to": 1409608800,
                "count": 2,
                "progress": "Complete"
            }
    }
    ```

    In this sample response, the time range is evenly divided into two subintervals. The first subinterval is from September 1, 2014, 00:00:00 to September 1, 2014, 11:00:00. The second subinterval is from September 1, 2014, 11:00:00 to September 1, 2014, 22:00:00. The results of the first query are incomplete because the amount of data exceeds the limit. The response results indicate that three log entries meet the query conditions, but the results are incomplete. Two log entries are in the first subinterval and the query results are complete. One log entry is in the second subinterval, but the query results are incomplete. To obtain the complete results, you must repeat the sample request until the value of the progress parameter in the response becomes Complete. Sample responses:

    ```
    HTTP/1.1 200 OK
    Header :
    {
    Content-Type: application/json
    Content-MD5: E6AD9C21204868C2DE84EE3808AAA8C8
    Content-Type: application/json
    Date: Wed, 3 Sept. 2014 08:33:48 GMT
    Content-Length: 232
    x-log-requestid: afag01322-1e241-25432e
    }
    Body :
    {
            {
                "from": 1409529600,
                "to": 1409569200,
                "count": 2,
                "progress": "Complete"
            },
            {
                "from": 1409569200,
                "to": 1409608800,
                "count": 1,
                "progress": "Incomplete"
            }
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|:----------|
|404|ProjectNotExist|Project ProjectName does not exist.|The error message returned because the specified project does not exist.|
|404|LogStoreNotExist|logstore logstoreName does not exist.|The error message returned because the specified Logstore does not exist.|
|400|InvalidTimeRange|request time range is invalid.|The error message returned because the specified time range is invalid.|
|400|InvalidQueryString|query string is invalid.|The error message returned because the specified query string in the request is invalid.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

