# GetMachineGroup

Views the information of a machine group.

## Request syntax

```
GET /machinegroups/groupName HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

The value of the Host parameter consists of a project name and Log Service endpoint. You must specify the project in the Host parameter.

## Request parameters

-   Request headers

    The CreateMachineGroup API operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |groupName|String|Yes|test-machine-group|The name of the machine group.|


## Response parameters

-   Response headers

    The CreateMachineGroup API operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the request succeeds, the HTTP status code 200 is returned. The response body contains the information of the specified machine group. The following table lists the parameters in the response body.

    |Parameter|Type|Example|Description|
    |---------|----|-------|-----------|
    |groupName|String|test-machine-group|The name of the machine group.|
    |groupType|String|Armory|The type of the machine group. Valid values: null and Armory.|
    |machineIdentifyType|String|ip|The type of the machine group identifier. Valid values:    -   ip: indicates that the machine group uses an IP address as an identifier.
    -   userdefined: indicates that the machine group uses a user-defined identifier. |
    |groupAttribute|Json Object|\{"externalName": "testgroup", "groupTopic": "testtopic"\}|The attributes of the machine group. The following table describes the parameters in the groupAttribute parameter.|
    |machineList|Json Array|\[ "127.0.0.1", "127.0.0.2" \]|The list of machine group identifiers.    -   If you set machineIdentifyType to ip, enter the IP address of the machine.
    -   If you set machineIdentifyType to userdefined, enter a custom identifier. |
    |createTime|integer|1447178253|The time when the machine group was created. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00, January 1, 1970.|
    |lastModifyTime|integer|1447178253|The time when the machine group was last updated. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00, January 1, 1970.|

    The groupAttribute parameter consists of the following parameters.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |groupTopic|String|testtopic|The topic of the machine group.|
    |externalName|String|testgroup|The identifier of the external management system \(Armory\) on which the machine group depends.|


## Examples

-   Sample requests

    ```
    GET /machinegroups/test-machine-group HTTP/1.1
    Header :
    {
        "x-log-apiversion": "0.6.0",
        "Authorization": "LOG yourAccessKeyId:yourSignature",
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
        "Date": "Tue, 10 Nov 2015 18:15:24 GMT",
        "Content-Length": "0",
        "x-log-signaturemethod": "hmac-sha1",
        "User-Agent": "sls-java-sdk-v-0.6.0",
        "Content-Type": "application/x-protobuf",
        "x-log-bodyrawsize": "0"
    }
    ```

-   Sample responses

    ```
    HTTP/1.1 200 OK
    Header :
    {
        "Date": "Tue, 10 Nov 2015 18:15:23 GMT",
        "Content-Length": "239",
        "x-log-requestid": "5642343B99248CB36D0060B8",
        "Connection": "close",
        "Content-Type": "application/json",
        "Server": "nginx/1.6.1"
    }
    Body :
    {
        "groupName": "test-machine-group",
        "groupType": "",
        "groupAttribute":     {
            "externalName": "testgroup",
            "groupTopic": "testtopic"
        },
        "machineIdentifyType": "ip",
        "machineList":     [
            "127.0.0.1",
            "127.0.0.2"
        ],
        "createTime": 1447178253,
        "lastModifyTime": 1447178253
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|Project projectName does not exist.|The error message returned because the specified project does not exist.|
|404|GroupNotExist|group groupName does not exist.|The error message returned because the specified machine group does not exist.|
|500|InternalServerError|Internal server error|The error message returned because an internal server error has occurred.|

For more information about the error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

