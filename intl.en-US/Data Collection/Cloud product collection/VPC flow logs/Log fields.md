# Log fields

This topic describes the fields of VPC flow logs.

|Field|Description|
|:----|:----------|
|\_\_topic\_\_|The topic of the log entry. Valid value: flow\_log.|
|version|The version of the flow log.|
|vswitch-id|The ID of the VSwitch to which the ENI is attached.|
|vm-id|The ID of the ECS instance to which the ENI is attached.|
|vpc-id|The ID of the VPC to which the ENI belongs.|
|account-id|The ID of the Alibaba Cloud account.|
|eni-id|The ID of the ENI.|
|srcaddr|The source IP address.|
|srcport|The source port.|
|dstaddr|The destination IP address.|
|dstport|The destination port.|
|protocol|The IANA protocol number of the traffic. For more information, see [Internet protocol number](http://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml).|
|direction|The direction of the traffic flow. Valid values: -   in: inbound traffic
-   out: outbound traffic |
|packets|The number of data packets.|
|bytes|The number of bytes in a data packet.|
|start|The start time of the capture window.|
|end|The end time of the capture window.|
|log-status|The logging status of the flow log. Valid values: -   OK: Flow log data is recorded as expected.
-   NODATA: No inbound or outbound traffic is transmitted over the ENI during the capture window.
-   SKIPDATA: Some flow log data is not recorded within the capture window. |
|action|The actions performed on traffic flows. Valid values: -   ACCEPT: the traffic that security groups allow to transfer.
-   REJECT: the traffic that security groups disallow to transfer. |

