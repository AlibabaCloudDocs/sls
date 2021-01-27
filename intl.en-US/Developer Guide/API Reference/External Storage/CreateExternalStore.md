# CreateExternalStore

Creates an external store.

## Description

The available external stores include Object Storage Service \(OSS\) objects and ApsaraDB RDS for MySQL databases over a virtual private cloud \(VPC\). This topic uses an ApsaraDB RDS for MySQL database as an example. For information about how to use an OSS object as a data source, see [Associate Log Service with OSS](/intl.en-US/Index and query/Associate Log Service with external data sources/Associate Log Service with OSS.md).

## Request syntax

```
POST /externalstores HTTP/1.1
x-log-bodyrawsize: 0
Content-Type: application/json 
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1 
Host: ProjectName.Endpoint 
Date: GMT Date
Authorization: LOG yourAccessKeyId:yourSignature 
{
	"externalStoreName": "externalStoreName",
	"storeType": "rds-vpc",
	"parameter": {
		"vpc-id": "vpc-id",
		"instance-id": "instance-id",
		"host": "host",
		"port": "port",
		"username": "username",
		"password": "password",
		"db": "db",
		"table": "table",
		"region": "region"
	}
}
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The CreateExternalStore operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |:--------|:---|:-------|-------|:----------|
    |projectName|String|Yes|ali-test-project|The name of the project.|
    |externalStoreName|String|Yes|rds\_store|The name of the external store. The name must be unique in a project. The name must also be different from the Logstore name.|
    |storeType|String|Yes|rds-vpc|The type of the external store. Valid value: rds-vpc. This value indicates an ApsaraDB RDS for MySQL database over a VPC.|
    |vpc-id|String|No|vpc-bp1aevy8sofi8mh1q\*\*\*\*|The ID of the VPC to which the ApsaraDB RDS for MySQL instance belongs.|
    |instance-id|String|No|i-bp1b6c719dfa08exf\*\*\*\*|The ID of the ApsaraDB RDS for MySQL instance.|
    |host|String|No|192.168.XX.XX|The internal or public endpoint of the ApsaraDB RDS for MySQL instance.|
    |port|String|Yes|3306|The internal or public port of the ApsaraDB RDS for MySQL instance.|
    |username|String|Yes|root|The username that is used to log on to the ApsaraDB RDS for MySQL instance.|
    |password|String|Yes|sfdsfldsfksfls\*\*\*\*|The password that is used to log on to the ApsaraDB RDS for MySQL instance.|
    |db|String|Yes|meta|The name of the database in the ApsaraDB RDS for MySQL instance.|
    |table|String|Yes|join\_meta|The name of the database table in the ApsaraDB RDS for MySQL instance.|
    |region|String|Yes|cn-qingdao|The region to which the ApsaraDB RDS for MySQL instance belongs. Valid values: cn-qingdao, cn-beijing, and cn-hangzhou.|


## Response parameters

-   Response headers

    The CreateExternalStore operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the HTTP status code 200 is returned, the request is successful. If the request is successful, no other elements are returned.


## Examples

-   Sample requests

    ```
    POST /externalstores HTTP/1.1
    Header :
    {
    x-log-bodyrawsize: 0
    Content-Type: application/json
    Content-Length: 307
    Content-MD5: 7C1D14659C0BBBA7C7BFF9E5A1A46705
    x-log-apiversion: 0.6.0
    x-log-signaturemethod: hmac-sha1
    Host: ali-test-project.cn-chengdu.log.aliyuncs.com
    Date: Thu, 19 Apr 2018 02:15:41 GMT
    Authorization: LOG yourAccessKeyId:yourSignature
    }
    Body :
    {
    	"externalStoreName": "rds_store",
    	"storeType": "rds-vpc",
    	"parameter": {
    		"vpc-id": "vpc-bp1aevy8sofi8mh1q****",
    		"instance-id": "i-bp1b6c719dfa08exf****",
    		"host": "192.168.XX.XX",
    		"port": "3306",
    		"username": "root",
    		"password": "sfdsfldsfksfls****",
    		"db": "meta",
    		"table": "join_meta",
    		"region": "cn-qingdao"
    	}
    }
    ```

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    Header
    {
    date: Mon, 09 Nov 2015 07:45:30 GMT
    connection: close
    x-log-requestid: 56404F1A99248CA26C002180
    content-length: 0
    server: nginx/1.6.1
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|:---------------|:---------|:------------|-----------|
|404|ProjectNotExist|The Project does not exist : projectName|The error message returned because the specified project does not exist.|
|400|ParameterInvalid|The body is not valid json string.|The error message returned because a parameter value is invalid in the configurations of the external store.|
|500|InternalServerError|Specified Server Error Message.|The error message returned because an internal server error has occurred.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

