# Log fields

This topic describes the fields of API Gateway access logs.

|Field|Description|
|:----|:----------|
|apiGroupUid|The ID of the group to which the API belongs.|
|apiGroupName|The name of the group to which the API belongs.|
|apiUid|API ID.|
|apiName|The name of the API.|
|apiStageUid|The stage ID of the API.|
|apiStageName|The stage name of the API.|
|httpMethod|The HTTP method of the request.|
|path|The path of the requested resource.|
|domain|The domain name of the requested resource.|
|statusCode|The HTTP status code.|
|errorMessage|The returned error message.|
|appId|The ID of the client that sends the request.|
|appName|The name of the client that sends the request.|
|clientIp|The IP address of the client that sends the request.|
|exception|The error message returned by the backend server.|
|providerAliUid|The ID of the Alibaba Cloud account or RAM user that the API belongs.|
|region|The ID of the region, for example, cn-hangzhou.|
|requestHandleTime|The time when the request is sent. The time is in GMT.|
|requestId|The request ID. The ID is globally unique.|
|requestSize|The size of the request message. Unit: bytes.|
|responseSize|The size of the response message. Unit: bytes.|
|serviceLatency|The response latency of the backend server. Unit: milliseconds.|

