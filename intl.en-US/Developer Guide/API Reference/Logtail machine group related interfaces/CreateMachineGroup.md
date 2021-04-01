# CreateMachineGroup

Creates a machine group.

## Request syntax

```
POST /machinegroups HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
Content-Type: application/json
Content-Length: Content Length
Content-MD5: Content MD5
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
{
    "groupName" : "testgroup",
    "groupType" : "",
    "groupAttribute" : {
        "externalName" : "testgroup",
        "groupTopic": "testgrouptopic"
    },
    "machineIdentifyType" : "ip",
    "machineList" : [
        "192.168.2.1",
        "192.168.2.2"
    ]
}
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The CreateMachineGroup API operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |groupName|String|Yes|test-machine-group|The name of the machine group.|
    |machineIdentifyType|String|Yes|ip|The type of the machine group identifier. Valid values:    -   ip: The machine group uses an IP address as an identifier.
    -   userdefined: The machine group uses a user-defined identifier. |
    |groupAttribute|Json|Yes|N/A|The attribute of the machine group. The following table describes the parameters in the groupAttribute parameter.|
    |machineList|Array|Yes|\[ "192.168.2.1", "192.168.2.2" \]|The list of machine group identifiers.    -   If you set machineIdentifyType to ip, enter the IP address of the machine.
    -   If you set machineIdentifyType to userdefined, enter a custom identifier. |
    |groupType|String|No|Armory|The type of the machine group. Valid values: null and Armory.|

    The groupAttribute parameter consists of the following parameters.

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |groupTopic|String|No|testtopic|The topic of the machine group.|
    |externalName|String|No|testgroup|The external management identifier on which the machine group depends.|


## Response parameters

-   Response headers

    The CreateMachineGroup API operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the request succeeds, the HTTP status code 200 is returned. If the request is successful, no other elements are returned.


## Examples

-   Sample requests

    ```
    POST /machinegroups HTTP/1.1
    Header :
    {
        "x-log-apiversion": "0.6.0",
        "Authorization": "LOG yourAccessKeyId:yourSignature",
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
        "Date": "Tue, 10 Nov 2015 17:57:33 GMT",
        "Content-Length": "187",
        "x-log-signaturemethod": "hmac-sha1",
        "Content-MD5": "82033D507DEAAD72067BB58DFDCB590D",
        "User-Agent": "sls-java-sdk-v-0.6.0",
        "Content-Type": "application/json",
        "x-log-bodyrawsize": "0"
    }
    Body :
    {
        "groupName": "test-machine-group",
        "groupType": "",
        "machineIdentifyType": "ip",
        "groupAttribute":     {
            "groupTopic": "testtopic",
            "externalName": "testgroup"
        },
        "machineList":     [
            "192.168.2.1",
            "192.168.2.2"
        ]
    }
    ```

-   Sample responses

    ```
    HTTP/1.1 200 OK
    Header :
    {
        "Date": "Tue, 10 Nov 2015 17:57:33 GMT",
        "Content-Length": "0",
        "x-log-requestid": "5642300D99248CB76D005D36",
        "Connection": "close",
        "Server": "nginx/1.6.1"
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|Project projectName does not exist.|The error message returned because the specified project does not exist.|
|400|MachineGroupAlreadyExist|group groupName already exists.|The error message returned because the machine group already exists.|
|400|InvalidParameter|invalid group resource json|The error message returned because a parameter value is invalid.|
|500|InternalServerError|Internal server error|The error message returned because an internal server error has occurred.|

For more information about the error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

