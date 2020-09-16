# Log fields

This topic describes the fields of operations logs in ActionTrail.

|Field|Description|
|:----|:----------|
|\_\_topic\_\_|The topic of the log entry. Valid value: actiontrail\_event.|
|event|The log event in the JSON format. The content of this field varies depending on the log event.|
|event.eventId|The unique ID of the event.|
|event.eventName|The name of the event.|
|event.eventSource|The source of the event.|
|event.eventType|The type of the event.|
|event.eventVersion|The event format version of the event. Valid value: 1.|
|event.acsRegion|The region where the event occurs.|
|event.requestId|The ID of the API request.|
|event.apiVersion|The version of the API.|
|event.errorMessage|The error message returned if an error occurs during the processing of the API request.|
|event.serviceName|The name of the Alibaba Cloud service associated with the event, for example, VPC.|
|event.sourceIpAddress|The IP address from which the request is sent.|
|event.userAgent|The user agent that sends the API request.|
|event.requestParameters.HostId|The ID of the server from which resources are requested.|
|event.requestParameters.Name|The name of the server from which resources are requested.|
|event.requestParameters.Region|The region where requested resources reside.|
|event.userIdentity.accessKeyId|The AccessKey ID of the logon account that initiates the API request.|
|event.userIdentity.accountId|The ID of the logon account that initiates the API request.|
|event.userIdentity.principalId|The requester ID. For example, if the type parameter is set to ram-user, enter the ID of the logon account.|
|event.userIdentity.type|The identity type of the logon account. -   root-account: Alibaba Cloud account
-   ram-user: RAM user
-   assumed-role: RAM role
-   system: Alibaba Cloud service |
|event.userIdentity.userName|The requester name. For example, if the type parameter is set to ram-user, enter the name of the logon account.|

