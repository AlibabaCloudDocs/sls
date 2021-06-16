# SLB

This topic describes the fields of Layer 7 access logs in Server Load Balancer \(SLB\).

|Log field|Description|
|---------|-----------|
|owner\_id|The ID of an Alibaba Cloud account.|
|region|The region where an instance resides.|
|instance\_id|The ID of an instance.|
|instance\_name|The name of an instance.|
|network\_type|The network type. Valid values:-   VPC: a virtual private cloud \(VPC\)
-   Classic: the classic network |
|vpc\_id|VPC ID|
|body\_bytes\_sent|The size of an HTTP message body that is sent to a client. Unit: bytes.|
|client\_ip|The IP address of a client.|
|client\_port|The client port.|
|host|The IP address of a server. The value is first obtained from the request parameters. If no value is obtained, the value is obtained from the host header field. If the value still cannot be obtained, the IP address of the backend server that processes the request is obtained as the field value.|
|http\_host|The Host HTTP header in a request message.|
|http\_referer|The Referer HTTP header in a request message that is received by the proxy.|
|http\_user\_agent|The User-Agent HTTP header in a request message that is received by the proxy.|
|http\_x\_forwarded\_for|The X-Forwarded-For \(XFF\) HTTP header in a request message that is received by the proxy.|
|http\_x\_real\_ip|The real IP address of a client.|
|read\_request\_time|The duration in which the proxy reads a request message. Unit: milliseconds.|
|request\_length|The length of a request message. This field includes the start-line, HTTP headers, and HTTP body.|
|request\_method|The request method.|
|request\_time|The duration between the time when the proxy receives the first request message and the time when the proxy returns a response message. Unit: seconds.|
|request\_uri|The URI of a request that is received by the proxy.|
|scheme|The protocol of an HTTP request. Valid values: HTTP and HTTPS.|
|server\_protocol|The protocol of a request.|
|slb\_vport|The listening port of an SLB instance.|
|slbid|The ID of an SLB instance.|
|ssl\_cipher|The used cipher suite.|
|ssl\_protocol|The protocol that is used to establish an SSL connection.|
|status|The HTTP status code that is sent from the proxy.|
|tcpinfo\_rtt|The RTT of TCP packets. Unit: microseconds.|
|time|The time when a log entry is recorded.|
|upstream\_addr|The IP address and port number of the backend server.|
|upstream\_response\_time|The duration of the connection between the proxy and backend server. Unit: seconds.|
|upstream\_status|The HTTP status code that is received by the proxy from the backend server.|
|vip\_addr|The virtual IP address.|
|write\_response\_time|The period of time that is required by the proxy to respond to a write request. Unit: milliseconds.|

