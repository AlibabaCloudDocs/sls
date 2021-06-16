# API Gateway

This topic describes the fields of access logs in API Gateway.

|Log field|Description|
|---------|-----------|
|owner\_id|The ID of the Alibaba Cloud account to which an API belongs.|
|apiGroupUid|The ID of the group to which an API belongs.|
|apiGroupName|The name of the group to which an API belongs.|
|apiUid|API ID|
|apiName|The name of an API.|
|apiStageUid|The stage ID of an API.|
|apiStageName|The stage name of an API.|
|httpMethod|The HTTP request method.|
|path|The uniform resource identifier \(URI\) of a request.|
|domain|The domain name of a resource for which an API request is sent.|
|statusCode|The HTTP status code.|
|errorMessage|The error message that is returned.|
|appId|The ID of the application from which an API request is sent.|
|appName|The name of the application from which an API request is sent.|
|clientIp|The IP address of a client that sends an API request.|
|exception|The specific error message that is returned by a backend server.|
|region|The ID of a region.|
|requestHandleTime|The time when an API request is sent. The time is in Greenwich Mean Time \(GMT\).|
|requestId|The ID of an API request. The ID is globally unique.|
|requestSize|The size of an API request. Unit: bytes.|
|responseSize|The size of a response message. Unit: bytes.|
|serviceLatency|The response latency of a backend server. Unit: milliseconds.|

