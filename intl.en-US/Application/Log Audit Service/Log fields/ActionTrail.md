# ActionTrail

This topic describes the fields of operation logs in ActionTrail.

|Log field|Description|
|---------|-----------|
|\_\_topic\_\_|The topic of a log entry. Valid value: actiontrail\_event.|
|owner\_id|The ID of an Alibaba Cloud account.|
|event|The log event in the JSON format. The content of this field varies based on the log event.|
|event.eventId|The ID of an event.|
|event.eventName|The name of an event.|
|event.eventSource|The source of an event.|
|event.eventType|The type of an event.|
|event.eventVersion|The data format version of an event. Valid value: 1.|
|event.acsRegion|The region where an event occurs.|
|event.requestId|The ID of an API request.|
|event.apiVersion|The version of the API.|
|event.errorMessage|The error message of an event.|
|event.serviceName|The name of the Alibaba Cloud service that is associated with an event.|
|event.sourceIpAddress|The source IP address that is associated with an event.|
|event.userAgent|The User-Agent HTTP header that is associated with an event.|
|event.requestParameters.HostId|The ID of the host from which a request is sent.|
|event.requestParameters.Name|The name of a request parameter.|
|event.requestParameters.Region|The region from which a request is sent.|
|event.userIdentity.accessKeyId|The AccessKey ID of an account that sends a request.|
|event.userIdentity.accountId|The ID of an account that sends a request.|
|event.userIdentity.principalId|The principal ID of an account that sends a request.|
|event.userIdentity.type|The type of an account that sends a request.|
|event.userIdentity.userName|The username of an account that sends a request.|
|event.errorCode|The error code of an event.|
|addionalEventData.isMFAChecked|Indicates whether multi-factor authentication \(MFA\) is enabled for the account that is used to log on to Log Service.|
|addionalEventData.loginAccount|The logon account.|

