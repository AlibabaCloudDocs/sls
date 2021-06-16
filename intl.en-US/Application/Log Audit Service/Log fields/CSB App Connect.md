# CSB App Connect

This topic describes the fields of operation logs in Cloud Service Bus \(CSB\) App Connect.

|Log field|Description|
|---------|-----------|
|\_\_topic\_\_|The topic of a log entry. Valid value: appconnect\_oplog.|
|uid|The ID of an Alibaba Cloud account.|
|execution\_id|The ID of a request or the ID of an execution.|
|status|The execution status of an integration flow. Valid values: begin and done.|
|flow\_name|The name of an integration flow.|
|step|The name of a step in an integration flow. The name is the unique identifier of the step.|
|id|The ID of a step. The ID is the unique index that is used to implement each integration flow. The ID can be decoded by using the stepTime timestamp field. A step can be executed multiple times in scenarios where loops are included. The step has the same name but different IDs. |
|type|The type of a step.|
|duration|The duration of a step. Unit: nanoseconds.|
|message|The output of a step. The output is in the string format.|
|step\_time|The time when a step is executed by an integration flow.|
|container\_ip|The IP address of a pod.|
|integration\_name|The name of a pod.|
|failed|Indicates whether a step is implemented.|

