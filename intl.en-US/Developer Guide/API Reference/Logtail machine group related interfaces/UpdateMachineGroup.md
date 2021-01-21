# UpdateMachineGroup

Modifies the configurations of a machine group.

## Request syntax

```
PUT /machinegroups/groupName HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature 
Content-Type:application/json
Date: GMT Date
Host: ProjectName.Endpoint
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
{
    "groupName": "test-machine-group",
    "groupType" : "",
    "groupAttribute" : {
        "externalName" : "testgroup",
        "groupTopic": "testgrouptopic"
    },
    "machineIdentifyType" : "ip",
    "machineList" : [
        "192.168.3.1",
        "192.168.3.2"
    ]
}
```

## Request parameters

-   Request headers

    The UpdateMachineGroup API operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |---------|----|--------|-------|-----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |groupName|String|Yes|test-machine-group|The name of the machine group.|
    |groupType|String|No|N/A|The type of the machine group. Default value: null.|
    |machineIdentifyType|String|Yes|userdefined|The type of the machine group identifier. Valid values:    -   ip: The machine group uses an IP address as an identifier.
    -   userdefined: The machine group uses a user-defined identifier. |
    |groupAttribute|Json|Yes|\{"externalName" : "testgroup", "groupTopic": "testgrouptopic"\}|The attribute of the machine group. Default value: null. The following table describes the parameters in the groupAttribute parameter.|
    |machineList|Array|Yes|uu\_id\_1, uu\_id\_2|The list of machine group identifiers.    -   If you set machineIdentifyType to ip, enter the IP address of the machine.
    -   If you set machineIdentifyType to userdefined, enter a custom identifier. |

    The groupAttribute parameter consists of the following parameters.

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |groupTopic|String|No|testtopic2|The topic of the machine group. Default value: null.|
    |externalName|String|No|testgroup2|The external management identifier on which the machine group depends. Default value: null.|


## Response parameters

-   Response headers

    The UpdateMachineGroup API operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the request succeeds, the HTTP status code 200 is returned. If the request is successful, no other elements are returned.


## Examples

-   Sample requests

    ```
    PUT /machinegroups/test-machine-group HTTP/1.1
    Header :
    {
        "x-log-apiversion": "0.6.0",
        "Authorization": "LOG yourAccessKeyId:yourSignature",
        "Host": "ali-test-project.cn-hangzhou-devcommon-intranet.sls.aliyuncs.com",
        "Date": "Tue, 10 Nov 2015 18:41:43 GMT",
        "Content-Length": "194",
        "x-log-signaturemethod": "hmac-sha1",
        "Content-MD5": "2CEBAEBE53C078891527CB70A855BAF4",
        "User-Agent": "sls-java-sdk-v-0.6.0",
        "Content-Type": "application/json",
        "x-log-bodyrawsize": "0"
    }
    Body :
    {
        "groupName": "test-machine-group",
        "groupType": "",
        "machineIdentifyType": "userdefined",
        "groupAttribute":     {
            "groupTopic": "testtopic2",
            "externalName": "testgroup2"
        },
        "machineList":     [
            "uu_id_1",
            "uu_id_2"
        ]
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header :
    {
        "Date": "Tue, 10 Nov 2015 18:41:43 GMT",
        "Content-Length": "0",
        "x-log-requestid": "56423A6799248CA57B00035C",
        "Connection": "close",
        "Server": "nginx/1.6.1"
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|Project projectName does not exist.|The error message returned because the specified project does not exist.|
|404|GroupNotExist|group groupName does not exist.|The error message returned because the specified machine group does not exist.|
|400|InvalidParameter|invalid group resource json.|The error message returned because a parameter value is invalid.|
|500|InternalServerError|Internal server error|The error message returned because an internal server error has occurred.|

For more information about the error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

