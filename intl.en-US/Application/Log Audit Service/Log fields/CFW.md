# CFW

This topic describes the fields of Internet access logs in Cloud Firewall \(CFW\).

|Log field|Description|
|---------|-----------|
|\_\_topic\_\_|The topic of a log entry. Valid value: cloudfirewall\_access\_log.|
|owner\_id|The ID of an Alibaba Cloud account.|
|log\_type|The type of a log entry.|
|app\_name|The name of the protocol over which an application is accessed. The value can be HTTPS, NTP, SIP, SMB, NFS, or DNS. If the protocol is unknown, the value Unknown is displayed.|
|direction|The direction of Internet traffic. Valid values:-   in: inbound traffic
-   out: outbound traffic |
|domain|The domain name of a destination server.|
|dst\_ip|The IP address of a destination server.|
|dst\_port|The destination port.|
|end\_time|The time when a session ends. The value is a UNIX timestamp. Unit: seconds.|
|in\_bps|The rate of inbound traffic. Unit: bit/s.|
|in\_packet\_bytes|The total size of inbound packets. Unit: bytes.|
|in\_packet\_count|The total number of inbound packets.|
|in\_pps|The rate of inbound packets. Unit: packet/s.|
|ip\_protocol|The type of an IP protocol. Valid values: TCP and UDP.|
|out\_bps|The rate of outbound traffic. Unit: bit/s.|
|out\_packet\_bytes|The total size of outbound traffic. Unit: bytes.|
|out\_packet\_count|The total number of outbound packets.|
|out\_pps|The rate of outbound packets. Unit: packet/s.|
|region\_id|The region from which access traffic originates.|
|rule\_result|The result of how an access policy processes Internet traffic. Valid values:-   pass: Data packets are allowed to pass Cloud Firewall.
-   alert: An alert is triggered when data packets attempt to pass Cloud Firewall.
-   drop: Data packets are dropped. |
|src\_ip|The IP address of a source server.|
|src\_port|The source port of a host that sends traffic data.|
|start\_time|The time when a session ends. The value is a UNIX timestamp. Unit: seconds.|
|start\_time\_min|The time when a session starts. The value is a UNIX timestamp. The value is rounded up to the next minute. Unit: seconds.|
|tcp\_seq|The sequence number of a TCP segment.|
|total\_bps|The total rate of inbound and outbound packets. Unit: bit/s.|
|total\_packet\_bytes|The total size of inbound and outbound packets. Unit: bytes.|
|total\_packet\_count|The total number of packets.|
|total\_pps|The total rate of inbound and outbound packets. Unit: packet/s.|
|src\_private\_ip|The private IP address of a source server.|
|vul\_level|The risk level of a vulnerability. Valid values:-   1: low
-   2: medium
-   3: high |
|url|The URL of a resource that is accessed.|
|acl\_rule\_id|The ID of an access control list \(ACL\) policy that is matched.|
|ips\_rule\_id|The ID of an intrusion prevention system \(IPS\) policy that is matched.|
|ips\_ai\_rule\_id|The ID of an intelligent policy that is matched.|
|ips\_rule\_name|The Chinese name of an IPS that is matched.|
|ips\_rule\_name\_en|The name of an IPS that is matched.|
|attack\_type\_name|The Chinese name of an attack type.|
|attack\_type\_name\_en|The name of an attack type.|

