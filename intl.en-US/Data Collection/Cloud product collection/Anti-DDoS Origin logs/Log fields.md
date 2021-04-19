# Log fields

This topic describes the key fields in Anti-DDoS Origin.

|Log field|Description|
|:--------|:----------|
|\_\_topic\_\_|The topic of a log entry. Valid value: ddos\_access\_log.|
|data\_type|Log type|
|event\_type|The type of an event.|
|ip|The IP address from which the request is sent.|
|subnet|The CIDR block of the instance that you want to reroute.|
|event\_time|The date when an event occurs, for example, 2020-01-01.|
|qps|The number of queries per second when the event occurred.|
|pps\_in|The inbound traffic when the event occurred. Unit: pps.|
|new\_con|The new connection that is established when an event occurs.|
|kbps\_in|The inbound traffic when an event occurs. Unit: bit/s.|
|instance\_id|The ID of an instance.|
|time|The time when a log is generated, for example, 2020-07-17 10:00:30.|
|destination\_ip|The IP address of a destination server.|
|port|The destination port.|
|total\_traffic\_in\_bps|The total amount of inbound traffic. Unit: bit/s.|
|total\_traffic\_drop\_bps|The amount of inbound traffic that is dropped. Unit: bit/s.|
|total\_traffic\_in\_pps|The total amount of inbound traffic. Unit: pps.|
|total\_traffic\_drop\_pps|The amount of inbound traffic that is dropped. Unit: pps.|
|pps\_types\_in\_tcp\_pps|The inbound TCP traffic that is measured by protocol. Unit: pps.|
|pps\_types\_in\_udp\_pps|The inbound UDP traffic that is measured by protocol. Unit: pps.|
|pps\_types\_in\_icmp\_pps|The inbound ICMP traffic that is measured by protocol. Unit: pps.|
|pps\_types\_in\_syn\_pps|The inbound SYN traffic that is measured by protocol. Unit: pps.|
|pps\_types\_in\_ack\_pps|The inbound ACK traffic that is measured by protocol. Unit: pps.|
|user\_id|The ID of an Alibaba Cloud account.|

