# Anti-DDoS Pro

This topic describes the fields of access logs in Anti-DDoS Pro.

|Log field|Description|
|---------|-----------|
|\_\_topic\_\_|The topic of a log entry. Valid value: ddoscoo\_access\_log.|
|owner\_id|The ID of an Alibaba Cloud account.|
|body\_bytes\_sent|The size of a request body. Unit: bytes.|
|cc\_action|The action that is performed based on an HTTP flood protection policy. The action can be none, challenge, pass, close, captcha, wait, or login.|
|cc\_phase|The HTTP flood protection policy, for example, seccookie, server\_ip\_blacklist, static\_whitelist, server\_header\_blacklist, server\_cookie\_blacklist, server\_args\_blacklist, or qps\_overmax.|
|cc\_blocks|Indicates whether a request is blocked by an HTTP flood protection policy. -   If the value is 1, the request is blocked.
-   If the value is not 1, the request is passed. |
|content\_type|The content type of a request.|
|host|The origin server.|
|http\_cookie|The Cookie HTTP header.|
|http\_referer|The Referer HTTP header. If an HTTP header does not contain a referer, a hyphen \(-\) is displayed.|
|http\_user\_agent|The User-Agent HTTP header.|
|http\_x\_forwarded\_for|The IP address of an upstream user. The IP address is forwarded by a proxy server.|
|https|Indicates whether a request is an HTTPS request. Valid values:-   true: The request is an HTTPS request.
-   false: The request is an HTTP request. |
|isp\_line|The information of an Internet service provider \(ISP\) line, for example, BGP, China Telecom, or China Unicom.|
|matched\_host|The matched origin server, which can be a wildcard domain name. If no origin server is matched, a hyphen \(-\) is displayed.|
|real\_client\_ip|The real IP address of a client. If no real IP address can be obtained, a hyphen \(-\) is displayed.|
|remote\_addr|The IP address of a client that sends an access request.|
|remote\_port|The port number of a client that sends an access request.|
|request\_length|The size of a request. Unit: bytes.|
|request\_method|The HTTP method of a request.|
|request\_time\_msec|The duration in which a request is processed. Unit: microseconds.|
|request\_uri|The uniform resource identifier \(URI\) of a request.|
|server\_name|The name of a matched server. If no server name is matched, default is displayed.|
|status|The HTTP status code.|
|time|The time when a request is sent.|
|ua\_browser|The browser.|
|ua\_browser\_family|The family to which a browser belongs.|
|ua\_browser\_type|The type of a browser.|
|ua\_device\_type|The type of a client.|
|ua\_os|The operating system of a client.|
|ua\_os\_family|The family of the operating system that runs on a client.|
|upstream\_addr|The list of back-to-origin IP addresses. Each IP address is in the IP:Port format. Multiple IP addresses are separated by commas \(,\). |
|upstream\_ip|The real IP address of an origin server.|
|upstream\_response\_time|The response time of a back-to-origin process. Unit: seconds.|
|upstream\_status|The HTTP status code of a back-to-origin request.|

