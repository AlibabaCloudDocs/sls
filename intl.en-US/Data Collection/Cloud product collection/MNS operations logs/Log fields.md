# Log fields

This topic describes the fields of the operations logs of MNS queue messages and topic messages.

## Operations logs of queue messages

Operations logs of queue messages are generated when you perform operations on queue messages. For more information, see [Message-related operations]().

The following table lists the fields of queue message operations logs.

|Field|Description|
|:----|:----------|
|Time|The time when the operation is performed.|
|MessageId|The ID of the message on which the operation is performed.|
|QueueName|The name of the queue to which the message belongs.|
|AccountId|The Alibaba Cloud account or RAM user to which the queue belongs.|
|RemoteAddress|The IP address of the client that calls the API operation.|
|NextVisibleTime|The time when the message becomes visible again after the operation is performed. If you call the PeekMessage or BatchPeekMessage operation, this field does not exist in the operations logs. |
|ReceiptHandleInRequest|The ReceiptHandle parameter used to call the API operation. If you call the SendMessage, BatchSendMessage, PeekMessage, BatchPeekMessage, DeleteMessage, or BatchDeleteMessage operation, this field does not exist in the operations logs. |
|ReceiptHandleInResponse|The ReceiptHandle parameter that is returned when the API operation is called. If you call the SendMessage, BatchSendMessage, PeekMessage, BatchPeekMessage, ReceiveMessage, or BatchReceiveMessage operation, this field does not exist in the operations logs. |

## Operations logs of topic messages

Operations logs of topic messages are generated when you perform operations on topic messages. For more information, see [Message-related operations]().

The following table lists the fields of topic message operations logs.

|Field|Description|
|:----|:----------|
|\_\_topic\_\_|The topic of the log entry.|
|Time|The time when the operation is performed.|
|MessageId|The ID of the message on which the operation is performed.|
|TopicName|The name of the topic to which the message belongs.|
|SubscriptionName|The name of the subscription to the message. If you call the PublishMessage operation, this field does not exist in the operations logs. |
|AccountId|The Alibaba Cloud account or RAM user to which the topic belongs.|
|RemoteAddress|The IP address of the client that calls the API operation. If you call the Notify operation, this field does not exist in the operations logs. |
|NotifyStatus|The status code that is returned when an MNS message is pushed. For more information, see [Table 3](#table_mow_q12_m1v). If you call the PublishMessage operation, this field does not exist in the operations logs. |

The NotifyStatus field is available only in operations logs that are generated when messages are pushed. You can use this field to locate the causes of push failures.

|Status code|Description|Troubleshooting|
|:----------|:----------|:--------------|
|2xx|The message is pushed to the receiver.|N/A|
|Non 2xx status codes|After a message is pushed, a non-2xx status code is returned.|Check the processing logic of the receiver.|
|InvalidHost|The specified endpoint of the receiver is invalid.|Check whether the specified endpoint of the receiver in the subscription is valid. You can check the error by using the curl/telnet command.|
|ConnectTimeout|The connection to the endpoint of the receiver specified in the subscription has timed out.|Check whether the endpoint of the receiver specified in the subscription is accessible. You can check the error by running the curl/telnet command.|
|ConnectFailure|Failed to connect to the endpoint of the receiver specified in the subscription.|Check whether the endpoint of the receiver specified in the subscription is accessible. You can check the error by running the curl/telnet command.|
|UnknownError|An unknown error has occurred.|Submit a [ticket](https://ticket-intl.console.aliyun.com/?accounttraceid=1cefa9e8cfb541609e124e8aa5a75e7bnltp#/ticket/add/?productId=1212).|

