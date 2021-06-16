# WAF

This topic describes the fields of access logs in Web Application Firewall \(WAF\).

|Log field|Description|
|---------|-----------|
|\_\_topic\_\_|The topic of a log entry. Valid value: waf\_access\_log.|
|owner\_id|The ID of an Alibaba Cloud account.|
|acl\_action|The action that is performed by WAF. This is the action that is triggered in response to a request based on an HTTP ACL policy, for example, pass, drop, or captcha. If the value is null or a hyphen \(-\), this field also indicates the pass action. |
|acl\_blocks|Indicates whether a request is blocked by an HTTP ACL policy.-   If the value is 1, the request is blocked.
-   If the value is not 1, the request is passed. |
|antibot|The type of an Anti-Bot Service protection policy that is matched. Valid values:-   ratelimit: frequency control
-   sdk: app protection
-   algorithm: intelligent algorithm
-   intelligence: bot threat intelligence
-   acl: HTTP ACL policy
-   blacklist: blacklist |
|antibot\_action|The action that is performed based on an Anti-Bot Service protection policy. Valid values:-   challenge: verifies a request by using an embedded JavaScript.
-   drop: blocks bot threats.
-   report: logs access events.
-   captcha: verifies a request by using a slider captcha. |
|block\_action|The type of a WAF protection feature that is matched. Valid values:-   tmd: protection against HTTP flood attacks
-   waf: protection against Web application attacks
-   acl: HTTP ACL policy
-   geo: region blocking
-   antifraud: data risk control
-   antibot: anti-bot |
|body\_bytes\_sent|The size of an HTTP message body that is sent to a client. Unit: bytes.|
|cc\_action|The action that is performed based on an HTTP flood protection policy. The action can be none, challenge, pass, close, captcha, wait, login, or n.|
|cc\_blocks|Indicates whether the request is blocked by the HTTP flood protection feature.-   If the value is 1, the request is blocked.
-   If the value is not 1, the request is passed. |
|cc\_phase|The HTTP flood protection policy that is triggered. The policy can be seccookie, server\_ip\_blacklist, static\_whitelist, server\_header\_blacklist, server\_cookie\_blacklist, server\_args\_blacklist, or qps\_overmax.|
|content\_type|The content type of an access request.|
|host|The origin server.|
|http\_cookie|The Cookie HTTP header. This field includes the information of a client.|
|http\_referer|The Referer HTTP header. This field includes the information of the source URL. If no information of the source URL is logged, a hyphen \(-\) is displayed.|
|http\_user\_agent|The User-Agent HTTP header. This field includes information such as a client browser and an operating system.|
|http\_x\_forwarded\_for|The X-Forwarded-For \(XFF\) HTTP header. This field identifies the original IP address of a client that connects to a web server by using an HTTP proxy or load balancing device.|
|https|Indicates whether the request is an HTTPS request. Valid values:-   true: The request is an HTTPS request.
-   false: The request is an HTTP request. |
|matched\_host|The matched origin server. This can be a wildcard domain name. If no origin server is matched, a hyphen \(-\) is displayed.|
|querystring|The query string in a request URL.|
|real\_client\_ip|The real IP address of a client. If no real IP address is obtained, a hyphen \(-\) is displayed.|
|region|The region where a WAF instance resides.|
|remote\_addr|The IP address of a client that sends a request.|
|remote\_port|The port number of a client.|
|request\_length|The size of a request message. Unit: bytes.|
|request\_method|The method of an HTTP access request.|
|request\_path|The relative path of a request. The query string is not included.|
|request\_time\_msec|The duration in which a request is processed. Unit: milliseconds.|
|request\_traceid|The unique ID of a request that is traced by WAF.|
|server\_protocol|The type and version number of a response protocol that is used by an origin server.|
|status|The HTTP status code that is returned by WAF to a client.|
|time|The time when a request is sent.|
|ua\_browser|The information of a browser that sends a request.|
|ua\_browser\_family|The family of a browser that sends a request.|
|ua\_browser\_type|The type of a browser that sends a request.|
|ua\_browser\_version|The version of a browser that sends a request.|
|ua\_device\_type|The type of a client.|
|ua\_os|The operating system of a client.|
|ua\_os\_family|The family of the operating system that runs on a client.|
|upstream\_addr|The list of back-to-origin IP addresses used by WAF. Each IP address is in the IP:Port format. Multiple IP addresses are separated by commas \(,\). |
|upstream\_ip|The IP address of an origin server that responds to a request. For example, if the origin server is an Elastic Compute Service \(ECS\) instance, the value of this field is the IP address of the ECS instance.|
|upstream\_response\_time|The duration in which an origin server processes a WAF request. Unit: seconds. If a hyphen \(-\) is returned, this field indicates that the response times out. |
|upstream\_status|The status code that an origin server returns to WAF. If a hyphen \(-\) is returned, the request is blocked by WAF or the response from the origin server times out. |
|user\_id|The ID of an Alibaba Cloud account.|
|waf\_action|The action that is performed based on a web attack protection policy. -   If the value is block, the request is blocked.
-   If the value is not block, the request is passed. |
|web\_attack\_type|The type of a web attack, for example, xss, code\_exec, webshell, sqli, lfilei, rfilei, or other.|
|waf\_rule\_id|The ID of a WAF rule that is matched.|
|ssl\_cipher|The SSL cipher suite.|
|ssl\_protocol|The version of the SSL protocol.|

