# GetShipperStatus

Queries the status of LogShipper tasks.

## Description

You can only query the status of LogShipper tasks that were performed within the last 48 hours.

## Request syntax

```
GET /logstores/logstoreName/shipper/shipperName/tasks HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The GetShipperStatus operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |logstoreName|String|Yes|test-logstore|The name of the Logstore.|
    |shipperName|String|Yes|test-shipper|The name of the LogShipper task.|
    |startTime|Integer|Yes|1448748198|The timestamp when the LogShipper task begins. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|
    |endTime|Integer|Yes|1448948198|The timestamp when the LogShipper task ends. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|
    |statusType|String|No|success|The default value is empty. It indicates that all LogShipper tasks are returned. These tasks can be in the success, fail, or running state.|
    |offset|Integer|No|0|The line from which the query starts. Default value: 0.|
    |size|Integer|No|100|The number of lines to return on each page in a paged query. Default value: 100. Maximum value: 500.|


## Response parameters

-   Response headers

    The GetShipperStatus operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. The response body contains a list of specified LogShipper tasks. The following table describes the parameters of the response body.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |count|Integer|10|The number of returned tasks on the current page.|
    |total|Integer|20|The total number of tasks.|
    |statistics|Json|None|The statistics of task status within a specified time range. For more information, see the table that describes the elements of the statistics parameter.|
    |tasks|Array|None|The details of LogShipper tasks. For more information, see the table that describes the elements of the tasks parameter|

    The following table describes the elements of the statistics parameter.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |running|Integer|0|The number of tasks that are in the running state.|
    |success|Integer|20|The number of tasks that are in the success state.|
    |fail|Integer|0|The number of tasks that are in the fail state.|

    The following table describes the elements of the tasks parameter.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |id|String|abcdefghijk|The unique ID of the LogShipper task.|
    |taskStatus|String|success|The status of the LogShipper task. The task can be in the running, success, or fail state.|
    |taskMessage|String|None|The error message that is returned if a LogShipper task fails.|
    |taskCreateTime|Integer|1448925013|The timestamp when the LogShipper task was created. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|
    |taskLastDataReceiveTime|Integer|1448915013|The timestamp when the server receives the last log of a LogShipper task. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|
    |taskFinishTime|Integer|1448926013|The timestamp when the LogShipper task ends. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.|


## Examples

-   Sample requests

    ```
    GET /logstores/test-logstore/shipper/test-shipper/tasks? from=1448748198&to=1448948198&status=success&offset=0&size=100 HTTP/1.1
    Header:
    {
        "x-log-apiversion" : 0.6.0, 
        "Authorization" : "LOG yourAccessKeyId:yourSignature", 
        "Host" : "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com", 
        "Date" : "Wed, 11 Nov 2015 08:28:19 GMT", 
        "Content-Length" : 55, 
        "x-log-signaturemethod" : "hmac-sha1", 
        "Content-MD5" : "757C60FC41CC7D3F60B88E0D916D051E", 
        "User-Agent" : "sls-java-sdk-v-0.6.0", 
        "Content-Type" : "application/json"
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header:
    {
        "Date" : "Wed, 11 Nov 2015 08:28:20 GMT", 
        "Content-Length" : 0, 
        "x-log-requestid" : "5642FC2399248C8F7B0145FD", 
        "Connection" : "close", 
        "Server" : "nginx/1.6.1"
    }
    Body:
    {
        "count" : 10,
        "total" : 20,
        "statistics" : {
            "running" : 0,
            "success" : 20,
            "fail" : 0 
        }
        "tasks" : [
            {
                "id" : "abcdefghijk",
                "taskStatus" : "success",
                "taskMessage" : "",
                "taskCreateTime" : 1448925013,
                "taskLastDataReceiveTime" : 1448915013,
                "taskFinishTime" : 1448926013
            }
        ]
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|Project ProjectName does not exist.|The error message returned because the specified project does not exist.|
|404|LogStoreNotExist|Logstore logstoreName does not exist.|The error message returned because the specified Logstore does not exist.|
|400|ShipperNotExist|Shipper shipperName does not exist.|The error message returned because the specified LogShipper task does not exist.|
|500|InternalServerError|Internal server error.|The error message returned because an internal server error has occurred.|
|400|ParameterInvalid|Start time must be earlier than end time.|The error message returned because the start time is later than or equal to the end time.|
|400|ParameterInvalid|Only support query last 48 hours task status.|The error message returned because the specified time range is invalid. You can query the status of the tasks that were performed within only the last 48 hours.|
|400|ParameterInvalid|Status only contains success/running/fail.|The error message returned because the specified status is invalid. The task can be in the success, running, or fail state.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

