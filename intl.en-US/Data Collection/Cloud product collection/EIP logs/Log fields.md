# Log fields

This topic describes the fields of Elastic IP Address \(EIP\) logs.

|Field|Description|
|-----|-----------|
|\_\_topic\_\_|The topic of the log. Valid value: eip.|
|type|The details of peak rates per second.|
|tid|The ID of the sender.|
|time|The sending time.|
|gw\_ip|The IP address of a gateway.|
|eip|The IP address of an EIP.|
|in\_Bps|The ingress bandwidth. Unit: bytes/s|
|out\_Bps|The egress bandwidth. Unit: bytes/s|
|in\_pps|The inbound packet rate. Unit: packets per second \(pps\).|
|out\_pps|The outbound packet rate. Unit: pps.|
|In\_syn\_speed|The inbound TCP session establishment rate. Unit: pps.|
|out\_syn\_speed|The outbound TCP session establishment rate. Unit: pps.|
|In\_syn\_ack\_speed|The inbound TCP session acknowledgement rate. Unit: pps.|
|out\_syn\_ack\_speed|The outbound TCP session acknowledgement rate. Unit: pps.|
|In\_fin\_speed|The inbound TCP session termination rate. Unit: pps.|
|out\_fin\_speed|The outbound TCP session termination rate. Unit: pps.|
|In\_rst\_speed|The inbound TCP session re-establishment rate. Unit: pps.|
|out\_rst\_speed|The outbound TCP session re-establishment rate. Unit: pps.|
|out\_ratelimit\_drop\_speed|The outbound packet loss threshold. Unit: pps.|
|in\_ratelimit\_drop\_speed|The inbound throttling packet loss rate. Unit: pps.|
|out\_drop\_speed|The outbound throttling packet loss rate. Unit: pps.|
|in\_drop\_speed|The inbound packet loss rate. Unit: pps.|
|timestamp|The timestamp. Unit: ms.|

