# GetProjectLogs

Queries logs in a project by executing an SQL statement.

## Description

-   The query statement that is used in this operation is a standard SQL statement.
-   You must specify a project in the domain name of the request.
-   You must specify a Logstore in the FROM clause of the query statement. A Logstore can be regarded as an SQL table.
-   You must specify a time range in the SQL statement by using the \_\_date\_\_ parameter \(timestamp\) or \_\_time\_\_ parameter \(integer\). The \_\_time\_\_ parameter is measured in seconds.

## Request syntax

```
GET /logs query=SELECT avg(latency) as avg_latency FROM  where __date__ >'2017-09-01 00:00:00' and __date__ < '2017-09-02 00:00:00'
Authorization: LOG yourAccessKeyId:yourSignature
Date: Wed, 3 Sept. 2014 08:33:46 GMT
Host: Projectname.endpoint
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The GetProjectLogs operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |---------|----|--------|-------|-----------|
    |projectName|String|Yes|big-game|The name of the project.|
    |query|String|Yes|\* \| SELECT \* FROM logStoreName where \_\_line\_\_ = 'error' and \_\_date\_\_ \>'2014-09-01 00:00:00' and \_\_date\_\_ < '2014-09-01 22:00:00|The SQL statement. The time range is from September 1, 2014, 00:00:00 to September 1, 2014, 22:00:00 and the keyword is "error".|


## Response parameters

-   Response headers

    For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md). The following table describes the response headers that are specific to this operation.

    |Parameter|Type|Example|Description|
    |---------|----|-------|-----------|
    |x-log-progress|String|Complete|The status of the query results. Valid values: Complete and Incomplete.    -   Complete: The query succeeded and the query results are complete.
    -   Incomplete: The query succeeded but the query results are incomplete. You must repeat the request to obtain complete query results. |
    |x-log-count|Integer|10000|The number of log entries in the query results.|
    |x-log-processed-rows|Integer|10000|The number of rows that are processed in the query.|
    |x-log-elapsed-millisecond|Integer|5|The number of milliseconds that are consumed by the query.|

-   Response elements

    The response body of the GetProjectLogs operation is an array that contains the information of each log entry. The following table describes the parameters of the response body.

    |Parameter|Type|Example|Description|
    |---------|----|-------|-----------|
    |\_\_time\_\_|Integer|1409529660|The log timestamp. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|
    |\_\_source\_\_|String|192.168.1.100|The log source, which is specified when logs are written.|
    |content|Array|"Key3": "error", "Key4": "Value4"|The original content of logs.|


## Sample code

In this example, the logs whose topic is groupA are queried in a Logstore named app\_log. The Logstore belongs to a project named big-game in the China \(Hangzhou\) region. The time range is from September 1, 2014, 00:00:00 to September 1, 2014, 22:00:00 and the keyword is "error".

-   Sample requests

    ```
    GET /logs/? query=SELECT * FROM logStoreName where __line__ = 'error' and __date__ >'2014-09-01 00:00:00' and __date__ < '2014-09-01 22:00:00'&line=20&offset=0 HTTP/1.1
    Authorization: LOG yourAccessKeyId:yourSignature
    Date: Wed, 3 Sept. 2014 08:33:46 GMT
    Host: big-game.cn-hangzhou.log.aliyuncs.com
    x-log-bodyrawsize: 0
    x-log-apiversion: 0.4.0
    x-log-signaturemethod: hmac-sha1
    ```

-   Sample responses

    ```
    HTTP/1.1 200 OK
    Content-MD5: 36F9F7F0339BEAF571581AF1B0AAAFB5
    Content-Type: application/json
    Content-Length: 269
    Date: Wed, 3 Sept. 2014 08:33:47 GMT
    x-log-requestid: efag01234-12341-15432f
    x-log-progress : Complete
    x-log-count : 10000
    x-log-processed-rows: 10000
    x-log-elapsed-millisecond:5
    {
        "progress": "Complete",
        "count": 2,
        "logs": [
            {
                "__time__": 1409529660,
                "__source__": "192.168.1.100",
                "Key1": "error",
                "Key2": "Value2"
            },
            {
                "__time__": 1409529680,
                "__source__": "192.168.1.100",
                "Key3": "error",
                "Key4": "Value4"
            }
        ]
    }
    ```

    In the sample response, the value of the x-log-progress parameter is Complete. This value indicates that the query succeeded and the returned results are complete. In this query, two log entries that meet the query conditions are found. If the value of the x-log-progress parameter is Incomplete, you must repeat the request to obtain complete query results.


## Error codes

|HTTP status code|Error code|Error message|Description|
|----------------|----------|-------------|-----------|
|400|ParameterInvalid|Parameter is invalid.|The error message returned because the specified parameter is invalid.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

