# Log fields

This topic describes the fields of Anti-DDos Pro log entries.

|Log field|Description|
|:--------|:----------|
|\_\_topic\_\_|The topic of a log entry. Valid value: ddos\_access\_log.|
|body\_bytes\_sent|The size of a request body. Unit: bytes.|
|cache\_status|The cache status.|
|cc\_action|The action that is performed to block HTTP flood attacks, for example, challenge, pass, close, captcha, wait, or login. If no action is performed, "none" is displayed.|
|cc\_phase|The protection policy that is used to block HTTP flood attacks, for example, seccookie, server\_ip\_blacklist, static\_whitelist, server\_header\_blacklist, server\_cookie\_blacklist, server\_args\_blacklist, or qps\_overmax.|
|cc\_blocks|Indicates whether a request is blocked by a protection policy.-   If the value is 1, the request is blocked.
-   If the value is not 1, the request is allowed. |
|content\_type|The content type of a request.|
|host|The origin server.|
|http\_cookie|The cookie of a request.|
|http\_referer|The referer of a request. If an HTTP header does not contain a referer, a hyphen \(-\) is displayed.|
|http\_user\_agent|The user agent of a request.|
|http\_x\_forwarded\_for|The IP address of an upstream user. The IP address is forwarded by a proxy server.|
|https|Indicates whether a request is an HTTPS request. Valid values:-   true: The request is an HTTPS request.
-   false: The request is an HTTP request. |
|isp\_line|The information of an Internet service provider \(ISP\), for example, BGP, China Telecom, or China Unicom.|
|matched\_host|The matched origin server. This can be a wildcard domain name. If no origin server is matched, a hyphen \(-\) is displayed.|
|querystring|The string of a request.|
|real\_client\_ip|The real IP address of a client. If no real IP address can be obtained, a hyphen \(-\) is displayed.|
|remote\_addr|The IP address of a client that sends an access request.|
|remote\_port|The port number of a client that sends an access request.|
|request\_length|The size of a request. Unit: bytes.|
|request\_method|The HTTP method of a request.|
|request\_time\_msec|The duration for which a request is processed. Unit: microseconds.|
|request\_uri|The uniform resource identifier \(URI\) of a request.|
|server\_name|The name of a matched server. If no server name is matched, default is displayed.|
|status|The HTTP status code, for example, 200.|
|time|The time when a request is sent.|
|ua\_browser|The browser.|
|ua\_browser\_family|The family to which a browser belongs.|
|ua\_browser\_type|The type of a browser.|
|ua\_browser\_version|The version of a browser.|
|ua\_device\_type|The type of a client.|
|ua\_os|The operating system of a client.|
|ua\_os\_family|The family of the operating system that runs on a client.|
|upstream\_addr|The list of back-to-origin IP addresses that are separated by commas \(,\). Each IP address is in the IP:Port format.|
|upstream\_ip|The real IP address of an origin server.|
|upstream\_response\_time|The response time of a back-to-origin process. Unit: seconds.|
|upstream\_status|The HTTP status code of a back-to-origin request.|
|user\_id|The ID of an Alibaba Cloud account.|

