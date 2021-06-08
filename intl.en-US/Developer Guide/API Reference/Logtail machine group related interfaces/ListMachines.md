# ListMachines

Lists the machines that are connected to Log Service in a machine group.

## Request syntax

```
GET /machinegroups/groupName/machines HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The ListMachines operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |groupName|String|Yes|test-machine-group-5|The name of the machine group.|
    |offset|Integer|No|0|The start row of the query. Default value: 0.|
    |size|Integer|No|3|The maximum number of entries to return on each page. Default value: 500.|


## Response parameters

-   Response headers

    The ListMachines API operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the request succeeds, the HTTP status code 200 is returned. The response body contains the information of the machine list. The following table lists the parameters in the response body.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |count|Integer|3|The number of machines that are returned on the current page.|
    |total|Integer|8|The total number of machines.|
    |machines|Json array|\[\{"ip": "10. \*\*\*. \*\*\*.\*\*\*", "machine-uniqueid": "", "userdefined-id": "", "lastHeartbeatTime": 1447182247\},\{"ip": "10. \*\*\*. \*\*\*.\*\*\*", "machine-uniqueid": "","userdefined-id": "","lastHeartbeatTime": 1447182246\}\]|The machine list that is returned. The following table describes the parameters in the machines parameter.|

    The machines parameter consists of the following parameters.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |ip|String|10.\*\*\*. \*\*\*. \*\*\*|The IP address of the machine.|
    |machine-uniqueid|String|N/A|The unique identifier of the machine.|
    |userdefined-id|String|N/A|The user-defined identifier of the machine.|
    |lastHeartbeatTime|Integer|1447182247|The last heartbeat time. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00, January 1, 1970.|


## Examples

-   Sample requests

    ```
    GET /machinegroups/test-machine-group-5/machines? offset=0&size=3 HTTP/1.1
    Header :
    {
        "x-log-apiversion": "0.6.0",
        "Authorization": "LOG yourAccessKeyId:yourSignature",
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
        "Date": "Tue, 10 Nov 2015 19:04:57 GMT",
        "Content-Length": "0",
        "x-log-signaturemethod": "hmac-sha1",
        "User-Agent": "sls-java-sdk-v-0.6.0",
        "Content-Type": "application/x-protobuf",
        "x-log-bodyrawsize": "0"
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header :
    {
        "Date": "Tue, 10 Nov 2015 19:04:58 GMT",
        "Content-Length": "324",
        "x-log-requestid": "56423FD999248C827B000A57",
        "Connection": "close",
        "Content-Type": "application/json",
        "Server": "nginx/1.6.1"
    }
    Body :
    {
        "machines":     [
            {
                "ip": "10. ***. ***.***",
                "machine-uniqueid": "",
                "userdefined-id": "",
                "lastHeartbeatTime": 1447182247
            },
            {
                "ip": "10. ***. ***.***",
                "machine-uniqueid": "",
                "userdefined-id": "",
                "lastHeartbeatTime": 1447182246
            },
            {
                "ip": "10. ***. ***.***",
                "machine-uniqueid": "",
                "userdefined-id": "",
                "lastHeartbeatTime": 1447182248
            }
        ],
        "count": 3,
        "total": 8
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|Project ProjectName does not exist.|The error message returned because the specified project does not exist.|
|404|GroupNotExist|Group groupName does not exist.|The error message returned because the specified machine group does not exist.|
|500|InternalServerError|Internal server error.|The error message returned because an internal server error has occurred.|

For more information about the error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

