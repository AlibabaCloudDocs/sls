# Fields of event logs

This topic describes the fields of event logs that are collected by using the inner-ActionTrail feature.

|Field|Description|
|-----|-----------|
|EventID|The unique ID of the event.|
|EventVersion|The version of the API. Valid value: 1.0.0.|
|EventProduct|The name of the service on which an operation is performed, such as OSS.|
|EventName|The name of the event that occurs when you call an API operation on an Alibaba Cloud service, such as Set Bucket Quota Limit.|
|EventDescription|The description of the event. The description includes the ID of the ticket, ticket ID of internal operations and maintenance \(O&M\) or changes, and ID of the security scanning task.|
|EventType|The event type. Valid values: -   CUSTOMER\_INITIATED\_SUPPORT

The technical support that is initiated by Alibaba Cloud O&M engineers, such as troubleshooting based on tickets that are submitted by users.

-   ALIYUN\_INITIATED\_SERVICE

The operations that are initiated by Alibaba Cloud engineers or systems based on O&M requirements, such as bucket migration across clusters after the cluster hardware is out of warranty.

-   ALIYUN\_INITIATED\_PENALTY

The operations that are performed on the public data of users and initiated by Alibaba Cloud engineers or systems based on rules and regulations. |
|EventMethod|The method that is used to call the API operation. For example, use the API, an SDK, or the console to read or write data and perform backup recovery.|
|ResourceType|The type of the event-associated resource, such as Acs::Oss::Bucket.|
|ResourceID|The ID of the resource. For example, the value can be the ID of a specific Object Storage Service \(OSS\) bucket.|
|ResourceRegionID|The ID of the region where the event-associated resource resides.|
|ResourceOwnerID|The ID of the Alibaba Cloud account to which the event-associated resource belongs.|
|EventAdditionalDetail|The additional information about the event.|
|EventTime|The time when the event-associated operation is performed, such as 2019-09-18T05:23:37Z. The time is in UTC.|
|EventLevel|The severity level of the event. Valid values: -   NOTICE: records the event in a log file.
-   WARNING: records the event in a log file and sends an alert notification to users. |

