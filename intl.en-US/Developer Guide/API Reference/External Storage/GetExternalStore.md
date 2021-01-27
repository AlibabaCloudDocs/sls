# GetExternalStore

Queries the details of an external store.

## Description

The available external stores include Object Storage Service \(OSS\) objects and ApsaraDB RDS for MySQL databases over a virtual private cloud \(VPC\). This topic uses an ApsaraDB RDS for MySQL database as an example. For information about how to use an OSS object as a data source, see [Associate Log Service with OSS](/intl.en-US/Index and query/Associate Log Service with external data sources/Associate Log Service with OSS.md).

## Request syntax

```
GET /externalstores/externalStoreName 
Content-Length: 0 
x-log-bodyrawsize: 0 
x-log-apiversion: 0.6.0 
x-log-signaturemethod: hmac-sha1
Host: ProjectName.Endpoint
Date: GMT Date
Authorization: LOG yourAccessKeyId:yourSignature
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The GetExternalStore operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |externalStoreName|String|Yes|rds\_store|The name of the external store.|


## Response parameters

-   Response headers

    The GetExternalStore operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. The response body contains the detailed information of the specified external store in the project. The following table describes the parameters of the response body.

    |Parameter|Type|Example|Description|
    |:--------|:---|-------|:----------|
    |storeType|String|rds-vpc|The type of the store. Valid value: rds-vpc. This value indicates an ApsaraDB RDS for MySQL database over a virtual private cloud \(VPC\).|
    |vpc-id|String|vpc-bp1aevy8sofi8mh1q\*\*\*\*|The ID of the VPC to which the ApsaraDB RDS for MySQL instance belongs.|
    |instance-id|String|i-bp1b6c719dfa08exf\*\*\*\*|The ID of the ApsaraDB RDS for MySQL instance.|
    |host|String|192.\*\*\*. \*\*\*. \*\*\*|The internal or public endpoint of the ApsaraDB RDS for MySQL instance.|
    |port|String|3306|The internal or public port of the ApsaraDB RDS for MySQL instance.|
    |username|String|root|The username that is used to log on to the ApsaraDB RDS for MySQL instance.|
    |db|String|meta|The name of the database in the ApsaraDB RDS for MySQL instance.|
    |table|String|join\_meta|The name of the database table in the ApsaraDB RDS for MySQL instance.|
    |region|String|cn-qingdao|The region to which the ApsaraDB RDS for MySQL instance belongs. Valid values: cn-qingdao, cn-beijing, and cn-hangzhou.|


## Examples

-   Sample requests

    ```
    GET /externalstores/rds_store
    Header :
    {
    Content-Length: 0 
    x-log-bodyrawsize: 0
    x-log-apiversion: 0.6.0
    x-log-signaturemethod: hmac-sha1
    Host: ali-test-project.cn-chengdu.log.aliyuncs.com 
    Date: Thu, 19 Apr 2018 03:26:49 GMT
    Authorization: LOG yourAccessKeyId:yourSignature
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header :
    {   
    content-length: 730
    server: nginx/1.6.1
    connection: close
    date: Mon, 09 Nov 2015 08:29:15 GMT
    content-type: application/json
    x-log-requestid: 5640595B99248CAA23004A59
    }
    Body :
    {
      'storeType': 'rds-vpc', 
      'parameter': {
                   'region': 'cn-qingdao', 
                   'vpc-id': 'vpc-p1aevy8sofi8mh1q****', 
                   'instance-id': 'i-bp1b6c719dfa08exf****', 
                   'host': '192.168.XX.XX', 
                   'port': '3306', 
                   'username': 'root', 
                   'db': 'meta', 
                   'table': 'join_meta'
                   }
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|The Project does not exist : projectName|The error message returned because the specified project does not exist.|
|404|LogStoreNotExist|Logstore externalStoreName does not exist|The error message returned because the specified external store does not exist.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

