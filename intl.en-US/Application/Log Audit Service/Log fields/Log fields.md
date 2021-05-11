# Log fields

This topic describes the fields of log entries that are collected from Alibaba Cloud services.

## ActionTrail

|Log field|Description|
|---------|-----------|
|\_\_topic\_\_|The topic of a log entry. Valid value: actiontrail\_event.|
|owner\_id|The ID of an Alibaba Cloud account.|
|event|The log event in the JSON format. The content of this field varies based on the log event.|
|event.eventId|The ID of an event.|
|event.eventName|The name of an event.|
|event.eventSource|The source of an event.|
|event.eventType|The type of an event.|
|event.eventVersion|The data format version of an event. Valid value: 1.|
|event.acsRegion|The region where an event occurs.|
|event.requestId|The ID of an API request.|
|event.apiVersion|The version of an API operation.|
|event.errorMessage|The error message of an event.|
|event.serviceName|The name of the Alibaba Cloud service that is associated with an event.|
|event.sourceIpAddress|The source IP address that is associated with an event.|
|event.userAgent|The User-Agent HTTP header that is associated with an event.|
|event.requestParameters.HostId|The ID of the host from which a request is sent.|
|event.requestParameters.Name|The name of a request parameter.|
|event.requestParameters.Region|The region from which a request is sent.|
|event.userIdentity.accessKeyId|The AccessKey ID of an account that sends a request.|
|event.userIdentity.accountId|The ID of an account that sends a request.|
|event.userIdentity.principalId|The principal ID of an account that sends a request.|
|event.userIdentity.type|The type of an account that sends a request.|
|event.userIdentity.userName|The username of an account that sends a request.|
|event.errorCode|The error code of an event.|
|addionalEventData.isMFAChecked|Indicates whether multi-factor authentication \(MFA\) is enabled for the account that is used to log on to Log Service.|
|addionalEventData.loginAccount|The logon account.|

## Server Load Balancer \(SLB\)

|Log field|Description|
|---------|-----------|
|owner\_id|The ID of an Alibaba Cloud account.|
|region|The region where an instance resides.|
|instance\_id|The ID of an instance.|
|instance\_name|The name of an instance.|
|network\_type|The type of network. Valid values: VPC and Classic.|
|vpc\_id|VPC ID|
|body\_bytes\_sent|The size of the HTTP response message body that is sent to a client.|
|client\_ip|The IP address of a client that sends a request.|
|client\_port|The port number of a client that sends a request.|
|host|The IP address of a server. The value is first obtained from the request parameters. If no value is obtained, the value is obtained from the host header field. If the value still cannot be obtained, the IP address of the backend server that processes the request is obtained as the field value.|
|http\_host|The HTTP Host header in a request message.|
|http\_referer|The HTTP Referer header in a request message that is received by the proxy.|
|http\_user\_agent|The User-Agent HTTP header in a request message that is received by the proxy.|
|http\_x\_forwarded\_for|The X-Forwarded-For \(XFF\) HTTP header in a request message that is received by the proxy.|
|http\_x\_real\_ip|The real IP address of a client.|
|read\_request\_time|The duration in which the proxy reads a request message. Unit: milliseconds.|
|request\_length|The length of a request message. This field includes the start-line, HTTP headers, and HTTP body.|
|request\_method|The request method.|
|request\_time|The duration between the time when the proxy receives the first request message and the time when the proxy returns a response message. Unit: seconds.|
|request\_uri|The URI of a request that is received by the proxy.|
|scheme|The protocol of a request, for example, HTTP or HTTPS.|
|server\_protocol|The HTTP version that is received by the proxy, for example, HTTP/1.0 or HTTP/1.1.|
|slb\_vport|The listening port of an SLB instance.|
|slbid|The ID of an SLB instance.|
|ssl\_cipher|The used cipher suite, for example, ECDHE-RSA-AES128-GCM-SHA256.|
|ssl\_protocol|The protocol that is used to establish an SSL connection, for example, TLSv1.2.|
|status|The HTTP status code that is sent from the proxy.|
|tcpinfo\_rtt|The RTT of TCP packets. Unit: microseconds.|
|time|The time when a log entry is recorded.|
|upstream\_addr|The IP address and port number of the backend server.|
|upstream\_response\_time|The duration of the connection between the proxy and backend server. Unit: seconds.|
|upstream\_status|The HTTP status code that is received by the proxy from the backend server.|
|vip\_addr|The virtual IP address.|
|write\_response\_time|The duration in which the proxy writes a response message. Unit: milliseconds.|

## API Gateway

|Log field|Description|
|---------|-----------|
|owner\_id|The ID of the account to which an API belongs.|
|apiGroupUid|The ID of the group to which an API belongs.|
|apiGroupName|The name of the group to which an API belongs.|
|apiUid|API ID|
|apiName|The name of an API.|
|apiStageUid|The stage ID of an API.|
|apiStageName|The stage name of an API.|
|httpMethod|The HTTP method that is used by an API request.|
|path|The path of an API request.|
|domain|The domain name of the resource for which an API request is sent.|
|statusCode|The HTTP status code.|
|errorMessage|The error message that is returned.|
|appId|The ID of the application from which an API request is sent.|
|appName|The name of the application from which an API request is sent.|
|clientIp|The IP address of a client that sends an API request.|
|exception|The specific error message that is returned by the backend server.|
|region|The ID of a region, for example, cn-hangzhou.|
|requestHandleTime|The time when an API request is sent. The time is in Greenwich Mean Time \(GMT\).|
|requestId|The ID of an API request. The ID is globally unique.|
|requestSize|The size of an API request. Unit: bytes.|
|responseSize|The size of a response message. Unit: bytes.|
|serviceLatency|The response latency of the backend server. Unit: milliseconds.|

## Web Application Firewall \(WAF\)

|Log field|Description|
|---------|-----------|
|\_\_topic\_\_|The topic of a log entry. Valid value: waf\_access\_log.|
|owner\_id|The ID of an Alibaba Cloud account.|
|acl\_action|The action that is performed by WAF. This is the action that is triggered in response to a request based on an HTTP ACL policy, for example, pass, drop, or captcha. **Note:** If the value is null or a hyphen \(-\), this field also indicates the pass action. |
|acl\_blocks|Indicates whether a request is blocked by an HTTP ACL policy.-   If the value is 1, the request is blocked.
-   If the value is not 1, the request is passed. |
|antibot|The type of an Anti-Bot Service protection policy that is triggered. Valid values: -   ratelimit: frequency control
-   sdk: app protection
-   algorithm: intelligent algorithm
-   intelligence: bot threat intelligence
-   acl: HTTP ACL policy
-   blacklist: blacklist |
|antibot\_action|The action that is performed based on an Anti-Bot Service protection policy. Valid values: -   challenge: verifies a request by using an embedded JavaScript.
-   drop: blocks bot threats.
-   report: logs access events.
-   captcha: verifies a request by using a slider captcha. |
|block\_action|The type of a WAF protection feature that is triggered. Valid values: -   tmd: protection against HTTP flood attacks
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
|http\_cookie|The HTTP Cookie header. This field includes the information of a client.|
|http\_referer|The HTTP Referer header. This field includes the information of the source URL. If no information of the source URL is logged, a hyphen \(-\) is displayed.|
|http\_user\_agent|The User-Agent HTTP header. This field includes information such as a client browser and an operating system.|
|http\_x\_forwarded\_for|The XFF HTTP header. This field identifies the original IP address of a client that connects to a web server by using an HTTP proxy or load balancing device.|
|https|Indicates whether a request is an HTTPS request. Valid values: -   true: The request is an HTTPS request.
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
|upstream\_addr|The list of back-to-origin IP addresses used by WAF. These IP addresses are separated by commas \(,\). Each IP address is in the IP:Port format.|
|upstream\_ip|The IP address of an origin server that responds to a request. For example, if the origin server is an Elastic Compute Service \(ECS\) instance, the value of this field is the IP address of the ECS instance.|
|upstream\_response\_time|The duration in which an origin server processes a WAF request. Unit: seconds. If a hyphen \(-\) is returned, this field indicates that the response times out.|
|upstream\_status|The status code that an origin server returns to WAF. If a hyphen \(-\) is returned, the request is blocked by WAF or the response from the origin server times out.|
|user\_id|The ID of an Alibaba Cloud account.|
|waf\_action|The action that is performed based on a web attack protection policy. If the value is block, the request is blocked. If the value is not block, the request is passed.|
|web\_attack\_type|The type of a web attack, for example, xss, code\_exec, webshell, sqli, lfilei, rfilei, or other.|
|waf\_rule\_id|The ID of a WAF rule that is matched.|
|ssl\_cipher|The SSL cipher suite.|
|ssl\_protocol|The version of the SSL protocol.|

## Security Center

-   Network logs
    -   DNS logs

        |Log field|Description|
        |---------|-----------|
        |\_\_topic\_\_|The topic of a log entry. Valid value: sas-log-dns.|
        |owner\_id|The ID of an Alibaba Cloud account.|
        |additional|The fields in the additional section. The fields are separated by vertical bars \(\|\).|
        |additional\_num|The number of fields in the additional section.|
        |answer|The DNS responses. These responses are separated by vertical bars \(\|\).|
        |answer\_num|The number of DNS responses.|
        |authority|The fields in the authority section.|
        |authority\_num|The number of fields in the authority section.|
        |client\_subnet|The subnet where a client resides.|
        |dst\_ip|The IP address of a destination server.|
        |dst\_port|The destination port.|
        |in\_out|The direction of data flows. Valid values:         -   in: inbound data flows
        -   out: outbound data flows |
        |qid|The ID of a query.|
        |qname|The domain name to be queried.|
        |qtype|The type of a resource to be queried.|
        |query\_datetime|The timestamp of a query. Unit: milliseconds.|
        |rcode|The code of a response.|
        |region|The ID of a source region. Valid values:         -   1: China \(Beijing\)
        -   2: China \(Qingdao\)
        -   3: China \(Hangzhou\)
        -   4: China \(Shanghai\)
        -   5: China \(Shenzhen\)
        -   6: Others |
        |response\_datetime|The time when a response is returned.|
        |src\_ip|The IP address of a source server.|
        |src\_port|The source port.|

    -   Local DNS logs

        |Log field|Description|
        |---------|-----------|
        |\_\_topic\_\_|The topic of a log entry. Valid value: local-dns.|
        |owner\_id|The ID of an Alibaba Cloud account.|
        |answer\_rda|The DNS responses. These responses are separated by vertical bars \(\|\).|
        |answer\_ttl|The time-to-live \(TTL\) of resource records in DNS responses. The values are separated by vertical bars \(\|\).|
        |answer\_type|The types of resource records in DNS responses. The values are separated by vertical bars \(\|\).|
        |anwser\_name|The domain names in DNS responses. The values are separated by vertical bars \(\|\).|
        |dest\_ip|The IP address of a destination server.|
        |dest\_port|The destination port.|
        |group\_id|The ID of the group to which a host belongs.|
        |hostname|The hostname.|
        |id|The IP address of a host.|
        |instance\_id|The ID of an instance.|
        |internet\_ip|The public IP address of a host.|
        |ip\_ttl|The TTL of the data packets that are sent by a host.|
        |query\_name|The domain name to be queried.|
        |query\_type|The type of a resource to be queried.|
        |src\_ip|The IP address of a source server.|
        |src\_port|The source port.|
        |time|The timestamp of a query. Unit: seconds.|
        |time\_usecond|The response duration. Unit: microseconds.|
        |tunnel\_id|The ID of a DNS tunnel.|

    -   Network session logs

        |Log field|Description|
        |---------|-----------|
        |\_\_topic\_\_|The topic of a log entry. Valid value: sas-log-session.|
        |owner\_id|The ID of an Alibaba Cloud account.|
        |asset\_type|The type of an associated Alibaba Cloud service, for example, ECS, SLB, or ApsaraDB RDS.|
        |dst\_ip|The IP address of a destination server.|
        |dst\_port|The destination port.|
        |proto|The type of a transport layer protocol, for example, TCP or UDP.|
        |session\_time|The duration of a session.|
        |src\_ip|The IP address of a source server.|
        |src\_port|The source port.|

    -   Web logs

        |Log field|Description|
        |---------|-----------|
        |\_\_topic\_\_|The topic of a log entry. Valid value: sas-log-http.|
        |owner\_id|The ID of an Alibaba Cloud account.|
        |content\_length|The content length of an HTTP request message.|
        |dst\_ip|The IP address of a destination server.|
        |dst\_port|The destination port.|
        |host|The hostname of a web server.|
        |jump\_location|The IP address of an HTTP redirect.|
        |method|The HTTP request method.|
        |referer|The HTTP Referer header. This field includes the address of the web page that sends a request.|
        |request\_datetime|The time when a request is sent.|
        |ret\_code|The HTTP status code.|
        |rqs\_content\_type|The content type of an HTTP request message.|
        |rsp\_content\_type|The content type of an HTTP response message.|
        |src\_ip|The IP address of a source server.|
        |src\_port|The source port.|
        |uri|The URI of a request.|
        |user\_agent|The user agent of a client that sends a request.|
        |x\_forward\_for|The XFF HTTP header.|

-   Security logs
    -   Vulnerability logs

        |Log field|Description|
        |---------|-----------|
        |\_\_topic\_\_|The topic of a log entry. Valid value: sas-vul-log.|
        |owner\_id|The ID of an Alibaba Cloud account.|
        |name|The name of a vulnerability.|
        |alias\_name|The alias of a vulnerability.|
        |op|The action that is performed on a vulnerability. Valid values:         -   new: detects a baseline.
        -   verify: verifies the vulnerability.
        -   fix: fixes the vulnerability. |
        |status|The status of a vulnerability. For more information, see [Table 2](#table_76m_auq_obw).|
        |tag|The tag of a vulnerability, for example, oval, system, or cms. This field is used to distinguish between different emergency \(EMG\) vulnerabilities.|
        |type|The type of a vulnerability. Valid values:         -   sys: Windows vulnerability
        -   cve: Linux vulnerability
        -   cms: Web CMS vulnerability
        -   EMG: Emergency vulnerability |
        |uuid|The universally unique identifier \(UUID\) of a client.|

    -   Baseline logs

        |Log field|Description|
        |---------|-----------|
        |\_\_topic\_\_|The topic of a log entry. Valid value: sas-hc-log.|
        |owner\_id|The ID of an Alibaba Cloud account.|
        |level|The level of a baseline. Valid values: low, medium, and high.|
        |op|The action that is performed on a baseline. Valid values:         -   new: detects a baseline.
        -   verify: verifies the baseline. |
        |risk\_name|The name of a baseline risk.|
        |status|The status of a baseline. For more information, see [Table 2](#table_76m_auq_obw).|
        |sub\_type\_alias|The subtype alias of a baseline.|
        |sub\_type\_name|The subtype of a baseline.|
        |type\_name|The type of a baseline.|
        |type\_alias|The type alias of a baseline.|
        |uuid|The UUID of a client.|
        |check\_item|The name of a check item.|
        |check\_level|The level of a check item.|
        |check\_type|The type of a check item.|

        |type\_name|sub\_type\_name|
        |----------|---------------|
        |system|baseline|
        |weak\_password|postsql\_weak\_password|
        |database|redis\_check|
        |account|system\_account\_security|
        |account|system\_account\_security|
        |weak\_password|mysq\_weak\_password|
        |weak\_password|ftp\_anonymous|
        |weak\_password|rdp\_weak\_password|
        |system|group\_policy|
        |system|register|
        |account|system\_account\_security|
        |weak\_password|sqlserver\_weak\_password|
        |system|register|
        |weak\_password|ssh\_weak\_password|
        |weak\_password|ftp\_weak\_password|
        |cis|centos7|
        |cis|tomcat7|
        |cis|memcached-check|
        |cis|mongodb-check|
        |cis|ubuntu14|
        |cis|win2008\_r2|
        |system|file\_integrity\_mon|
        |cis|linux-httpd-2.2-cis|
        |cis|linux-docker-1.6-cis|
        |cis|SUSE11|
        |cis|redhat6|
        |cis|bind9.9|
        |cis|centos6|
        |cis|debain8|
        |cis|redhat7|
        |cis|SUSE12|
        |cis|ubuntu16|

        |Status code|Description|
        |-----------|-----------|
        |1|Unfixed.|
        |2|Fix failed.|
        |3|Rollback failed.|
        |4|Fixing.|
        |5|Rolling back.|
        |6|Verifying.|
        |7|Fixed.|
        |8|Fixed. Waiting for a restart.|
        |9|Rollback succeeded.|
        |10|Ignored.|
        |11|Rollback succeeded. Waiting for a restart.|
        |12|No longer exists.|
        |20|Expired.|

    -   Security alert logs

        |Log field|Description|
        |---------|-----------|
        |\_\_time\_\_|The time when a connection is established, for example, 2018-02-27 11:58:15.|
        |\_\_topic\_\_|The topic of a log entry. Valid value: sas-security-log.|
        |data\_source|The source of the data. For more information, see [Table 3](#table_rlq_mbq_q3a).|
        |level|The severity of an alert.|
        |name|The name of an alert, for example, Suspicious Process-SSH-based Remote Execution of Non-interactive Commands.|
        |op|The action that is performed on an alert. Valid values:         -   new: An alert is triggered.
        -   dealing: The alert is being processed. |
        |status|The status of an alert. For more information, see [Table 2](#table_76m_auq_obw).|
        |uuid|The UUID of a client.|
        |detail|The detail of an alert, for example, \{"loginSourceIp":"120.27.28.118","loginTimes":1,"type":"login\_common\_location","loginDestinationPort":22,"loginUser":"aike","protocol":2,"protocolName":"SSH","location":"Qingdao"\}.|
        |unique\_info|The unique identifier of an alert for a single server, for example, 2536dd765f804916a1fa3b9516b5d512.|

        |Value|Description|
        |-----|-----------|
        |aegis\_suspicious\_event|Server exceptions|
        |aegis\_suspicious\_file\_v2|Webshell|
        |aegis\_login\_log|Suspicious logon|
        |security\_event|Security Center exceptions|

-   Host logs
    -   Process startup logs

        |Log field|Description|
        |---------|-----------|
        |\_\_topic\_\_|The topic of a log entry. Valid value: aegis-log-process.|
        |owner\_id|The ID of an Alibaba Cloud account.|
        |uuid|The UUID of a client.|
        |ip|The IP address of a client.|
        |cmdline|The full command line to start a process.|
        |username|The username.|
        |uid|The ID of a user.|
        |pid|The ID of a process.|
        |filename|The name of a process file.|
        |filepath|The full path of a process file.|
        |groupname|The name of a user group.|
        |ppid|The ID of a parent process.|
        |pfilename|The name of a parent process file.|
        |pfilepath|The full path of a parent process file.|

    -   Process snapshot logs

        |Log field|Description|
        |---------|-----------|
        |\_\_topic\_\_|The topic of a log entry. Valid value: aegis-snapshot-process.|
        |owner\_id|The ID of an Alibaba Cloud account.|
        |uuid|The UUID of a client.|
        |ip|The IP address of a client.|
        |cmdline|The full command line to start a process.|
        |pid|The ID of a process.|
        |name|The name of a process file.|
        |path|The full path of a process file.|
        |md5|The MD5 hash of a process file. If the process file exceeds 1 MB, the MD5 hash is not calculated.|
        |pname|The name of a parent process file.|
        |start\_time|The time when a process starts. This field is a built-in field.|
        |user|The username.|
        |uid|The ID of a user.|

    -   Logon logs

        The logon attempts within 1 minute are recorded in one log entry.

        |Log field|Description|
        |---------|-----------|
        |\_\_topic\_\_|The topic of a log entry. Valid value: aegis-log-login.|
        |owner\_id|The ID of an Alibaba Cloud account.|
        |uuid|The UUID of a client.|
        |ip|The IP address of a client.|
        |warn\_ip|The IP address of a source server.|
        |warn\_port|The logon port.|
        |warn\_type|The type of a logon. Valid values: SSHLOGIN, RDPLOGIN, and IPCLOGIN.|
        |warn\_user|The logon username.|
        |warn\_count|The number of logon attempts. In this example, the value 3 indicates that two logon requests are sent 1 minute before the current logon.|

    -   Brute-force cracking logs

        |Log field|Description|
        |---------|-----------|
        |\_\_topic\_\_|The topic of a log entry. Valid value: aegis-log-crack.|
        |owner\_id|The ID of an Alibaba Cloud account.|
        |uuid|The UUID of a client.|
        |ip|The IP address of a client.|
        |warn\_ip|The IP address of a source server.|
        |warn\_port|The logon port.|
        |warn\_type|The type of a logon. Valid values: SSHLOGIN, RDPLOGIN, and IPCLOGIN.|
        |warn\_user|The logon username.|
        |warn\_count|The number of failed logon attempts.|

    -   Network connection logs

        Changes in network connections are collected on the host every 10 seconds to 1 minute.

        |Log field|Description|
        |---------|-----------|
        |\_\_topic\_\_|The topic of a log entry. Valid value: aegis-log-network.|
        |owner\_id|The ID of an Alibaba Cloud account.|
        |uuid|The UUID of a client.|
        |ip|The IP address of a client.|
        |src\_ip|The IP address of a source server.|
        |src\_port|The source port.|
        |dst\_ip|The IP address of a destination server.|
        |dst\_port|The destination port.|
        |proc\_name|The name of a process.|
        |proc\_path|The path of a process file.|
        |proto|The protocol that is used to establish a network connection, for example, TCP, UDP, or raw \(raw socket\).|
        |status|The connection status. For more information, see [Table 4](#table_v7l_b25_hii).|

        |Status|Description|
        |------|-----------|
        |1|closed|
        |2|listen|
        |3|syn send|
        |4|syn recv|
        |5|establisted|
        |6|close wait|
        |7|closing|
        |8|fin\_wait1|
        |9|fin\_wait2|
        |10|time\_wait|
        |11|delete\_tcb|

    -   Port listening snapshot logs

        |Log field|Description|
        |---------|-----------|
        |\_\_topic\_\_|The topic of a log entry. Valid value: aegis-snapshot-port.|
        |owner\_id|The ID of an Alibaba Cloud account.|
        |uuid|The UUID of a client.|
        |ip|The IP address of a client.|
        |proto|The protocol that is used to establish a network connection, for example, TCP, UDP, or raw \(raw socket\).|
        |src\_ip|The IP address of a listener port.|
        |src\_port|The listener port.|
        |pid|The ID of a process.|
        |proc\_name|The name of a process.|

    -   Account snapshot logs

        |Log field|Description|
        |---------|-----------|
        |\_\_topic\_\_|The topic of a log entry. Valid value: aegis-snapshot-host.|
        |owner\_id|The ID of an Alibaba Cloud account.|
        |name|The name of a vulnerability.|
        |alias\_name|The alias of a vulnerability.|
        |op|The action that is performed on a vulnerability. Valid values:         -   new: detects a new vulnerability.
        -   verify: verifies the vulnerability.
        -   fix: fixes the vulnerability. |
        |status|The connection status. For more information, see [Table 4](#table_v7l_b25_hii).|
        |tag|The tag of a vulnerability, for example, oval, system, or cms. This field is used to distinguish between different emergency \(EMG\) vulnerabilities.|
        |type|The type of a vulnerability. Valid values:         -   sys: Windows vulnerability
        -   cve: Linux vulnerability
        -   cms: Web CMS vulnerability
        -   EMG: Emergency vulnerability |
        |uuid|The UUID of a client.|


## PolarDB-X

|Log field|Description|
|---------|-----------|
|\_\_topic\_\_|The topic of a log entry. Valid value: drds\_audit\_log.|
|instance\_id|The ID of a PolarDB-X instance.|
|instance\_name|The name of a PolarDB-X instance.|
|owner\_id|The ID of an Alibaba Cloud account.|
|region|The region where a PolarDB-X instance resides.|
|db\_name|The name of a PolarDB-X database.|
|user|The name of the user who executes an SQL statement.|
|client\_ip|The IP address of a client that accesses a PolarDB-X instance.|
|client\_port|The port number of a client that accesses a PolarDB-X instance.|
|sql|The SQL statement.|
|trace\_id|The trace ID of an SQL statement when it is executed. If a transaction is executed, it is tracked by using an ID. The ID consists of the trace ID, a hyphen \(-\), and a number, for example, drdsabcdxyz-1 and drdsabcdxyz-2.|
|sql\_code|The hash value of a template SQL statement.|
|hint|The hint that is used to execute an SQL statement.|
|table\_name|The names of the tables that are involved in a query. Multiple tables are separated by commas \(,\).|
|sql\_type|The type of an SQL statement. Valid values: Select, Insert, Update, Delete, Set, Alter, Create, Drop, Truncate, Replace, and Other.|
|sql\_type\_detail|The name of an SQL parser.|
|response\_time|The response duration. Unit: milliseconds.|
|affect\_rows|The number of affected or returned rows when an SQL statement is executed.|
|fail|Indicates the result after an SQL statement is executed. Valid values: -   0: successful
-   1: failed |
|sql\_time|The time when an SQL statement is executed.|

## Cloud Firewall

|Log field|Description|
|---------|-----------|
|\_\_topic\_\_|The topic of a log entry. Valid value: cloudfirewall\_access\_log.|
|owner\_id|The ID of an Alibaba Cloud account.|
|log\_type|The type of a log entry.|
|app\_name|The name of the protocol over which an application is accessed. The value can be HTTPS, NTP, SIP, SMB, NFS, or DNS. If the protocol is unknown, the value is displayed as Unknown.|
|direction|The direction of Internet traffic. Valid values: -   in: inbound traffic
-   out: outbound traffic |
|domain|The domain name of a destination server.|
|dst\_ip|The IP address of a destination server.|
|dst\_port|The destination port.|
|end\_time|The time when a session ends. Unit: seconds \(UNIX timestamp\).|
|in\_bps|The rate of inbound traffic. Unit: bit/s.|
|in\_packet\_bytes|The total size of inbound packets. Unit: bytes.|
|in\_packet\_count|The total number of inbound packets.|
|in\_pps|The rate of inbound packets. Unit: packet/s.|
|ip\_protocol|The type of an IP protocol. Valid values: TCP and UDP.|
|out\_bps|The rate of outbound traffic. Unit: bit/s.|
|out\_packet\_bytes|The total size of the outbound traffic. Unit: bytes.|
|out\_packet\_count|The total number of outbound packets.|
|out\_pps|The rate of outbound packets. Unit: packet/s.|
|region\_id|The region from which access traffic originates.|
|rule\_result|The result of how an access policy processes Internet traffic. Valid values: -   pass: Data packets are allowed to pass Cloud Firewall.
-   alert: An alert is triggered when data packets attempt to pass Cloud Firewall.
-   drop: Data packets are dropped. |
|src\_ip|The IP address of a source server.|
|src\_port|The source port of a host that sends traffic data.|
|start\_time|The time when a session starts. Unit: seconds \(UNIX timestamp\).|
|start\_time\_min|The time when a session starts. The value of this field is rounded up to the next minute. Unit: seconds \(UNIX timestamp\).|
|tcp\_seq|The sequence number of a TCP segment.|
|total\_bps|The total rate of inbound and outbound packets. Unit: bit/s.|
|total\_packet\_bytes|The total size of inbound and outbound packets. Unit: bytes.|
|total\_packet\_count|The total number of packets.|
|total\_pps|The total rate of inbound and outbound packets. Unit: bit/s.|
|src\_private\_ip|The private IP address of a source server.|
|vul\_level|The risk level of a vulnerability. Valid values:-   1: low
-   2: medium
-   3: high |
|url|The URL of a resource that is accessed.|
|acl\_rule\_id|The ID of an access control list \(ACL\) policy that is matched.|
|ips\_rule\_id|The ID of an intrusion prevention system \(IPS\) policy that is matched.|
|ips\_ai\_rule\_id|The ID of an intelligent policy that is matched.|

## Bastionhost

|Log field|Description|
|---------|-----------|
|\_\_topic\_\_|The topic of a log entry.|
|owner\_id|The ID of an Alibaba Cloud account.|
|content|The content of a log entry.|
|event\_type|The type of an event. For more information, see [Table 5](#table_ka4_b5i_2tr).|
|instance\_id|The ID of a bastion host.|
|log\_level|The severity of a log entry.|
|resource\_address|The address of the server where a resource resides.|
|resource\_name|The name of the resource on which an operation is performed.|
|result|The result of an operation.|
|session\_id|The ID of a session.|
|user\_client\_ip|The source IP address.|
|user\_id|The ID of a user.|
|user\_name|The username.|

|Event type|Description|
|----------|-----------|
|cmd.Command|The CMD commands.|
|file.Upload|Uploads a file.|
|file.Download|Downloads a file.|
|file.Rename|Renames a file.|
|file.Delete|Deletes a file.|
|file.DeleteDir|Deletes a directory.|
|file.CreateDir|Creates a directory.|
|graph.Text|Text event.|
|graph.Keyboard|Keyboard event.|

## Object Storage Service \(OSS\)

|Log type|Description|
|--------|-----------|
|Access logs|Records access to OSS buckets. The logs are collected in real time.|
|Batch deletion logs|Records information of deleted objects. The logs are collected in real time. **Note:** When you call the DeleteObjects API operation, a request record is generated in an access log. The information of the deleted objects is stored in the HTTP body of a request. A hyphen \(-\) is used to indicate the deleted objects in the access log. To retrieve the deleted objects, you can use the request\_id parameter to query the deleted objects in the batch deletion log. |
|Hourly metering logs|Records the hourly metering statistics of a specific bucket. A latency of several hours exists in log collection.|

|Storage type|Description|
|------------|-----------|
|standard|Standard|
|archive|Archive|
|infrequent\_access|IA|

For information about related API operations, see [API overview](/intl.en-US/API Reference/API overview.md).

|Operation|Description|
|---------|-----------|
|AbortMultiPartUpload|Cancels a multipart upload task.|
|AppendObject|Appends an object to an existing object.|
|CompleteUploadPart|Completes the multipart upload task of an object.|
|CopyObject|Copies an object.|
|DeleteBucket|Deletes a bucket.|
|DeleteLiveChannel|Deletes a LiveChannel.|
|DeleteObject|Deletes an object.|
|DeleteObjects|Deletes multiple objects.|
|GetBucket|Lists all objects in a bucket.|
|GetBucketAcl|Queries the access control list \(ACL\) of a bucket.|
|GetBucketCors|Queries the cross-origin resource sharing \(CORS\) rules of a bucket.|
|GetBucketEventNotification|Queries the notification configurations of a bucket.|
|GetBucketInfo|Queries the information of a bucket.|
|GetBucketLifecycle|Queries the lifecycle rules configured for the objects in a bucket.|
|GetBucketLocation|Queries the region where a bucket resides.|
|GetBucketLog|Queries the access log configurations of a bucket.|
|GetBucketReferer|Queries the hotlink protection rules configured for a bucket.|
|GetBucketReplication|Queries the cross-region replication \(CRR\) rules configured for a bucket.|
|GetBucketReplicationProgress|Queries the progress of a CRR task that is performed on a bucket.|
|GetBucketStat|Queries the information of a bucket.|
|GetBucketWebSite|Queries the status of the static website hosting for a bucket.|
|GetLiveChannelStat|Queries the status of a LiveChannel.|
|GetObject|Reads an object.|
|GetObjectAcl|Queries the ACL of an object.|
|GetObjectInfo|Queries the information of an object.|
|GetObjectMeta|Queries the metadata of an object.|
|GetObjectSymlink|Queries the symbolic link of an object.|
|GetPartData|Queries the data in all parts of an object.|
|GetPartInfo|Queries the information of all parts of an object.|
|GetProcessConfiguration|Queries the image processing configurations of a bucket.|
|GetService|Lists all buckets.|
|HeadBucket|Queries the information of a bucket.|
|HeadObject|Queries the information of an object.|
|InitiateMultipartUpload|Initializes the multipart upload for an object.|
|ListMultiPartUploads|Lists multipart upload events.|
|ListParts|Queries the status of all parts of an object.|
|PostObject|Uploads an object by using a form.|
|PostProcessTask|Commits data processing operations, such as screenshots.|
|PostVodPlaylist|Creates a video-on-demand \(VOD\) playlist of a LiveChannel.|
|ProcessImage|Processes an image.|
|PutBucket|Creates a bucket.|
|PutBucketCors|Specifies the CORS rule for a bucket.|
|PutBucketLifecycle|Specifies the lifecycle of a bucket.|
|PutBucketLog|Specifies the access log for a bucket.|
|PutBucketWebSite|Specifies the static website hosting mode for a bucket.|
|PutLiveChannel|Creates a LiveChannel.|
|PutLiveChannelStatus|Specifies the status of a LiveChannel.|
|PutObject|Uploads an object.|
|PutObjectAcl|Modifies the ACL of an object.|
|PutObjectSymlink|Creates a symbolic link for an object.|
|RedirectBucket|Redirects the request to a bucket endpoint.|
|RestoreObject|Restores an object.|
|UploadPart|Resumes the upload of an object from a specified checkpoint.|
|UploadPartCopy|Copies a part of an object.|
|get\_image\_exif|Queries the exchangeable image file format \(Exif\) data of an image.|
|get\_image\_info|Queries the length and width of an image.|
|get\_image\_infoexif|Queries the length, width, and Exif data of an image.|
|get\_style|Queries the style of a bucket.|
|list\_style|Queries all styles of a bucket.|
|put\_style|Creates a picture processing rule for a bucket.|

|Synchronization request type|Description|
|----------------------------|-----------|
|-|General requests|
|cdn|CDN back-to-origin requests|

For information about signatures, see [Verify user signatures](/intl.en-US/API Reference/Access control/Verify user signatures.md).

|Signature type|Description|
|--------------|-----------|
|NotSign|A request is unsigned.|
|NormalSign|A request is signed with a regular signature.|
|UriSign|A request is signed with a URL signature.|
|AdminSign|A request is signed with an administrator account.|

-   Access logs

    |Log field|Description|
    |---------|-----------|
    |\_\_topic\_\_|The topic of a log entry. Valid value: oss\_access\_log.|
    |owner\_id|The ID of an Alibaba Cloud account.|
    |region|The region where a bucket resides.|
    |access\_id|The AccessKey ID that is used to access OSS.|
    |time|The time when OSS receives a request. If a timestamp is required, use the value of the \_\_time\_\_ field.|
    |owner\_id|The ID of an Alibaba Cloud account that belongs to a bucket owner.|
    |User-Agent|The User-Agent HTTP header.|
    |logging\_flag|Indicates whether logging has been enabled to export logs to OSS buckets at regular intervals.|
    |bucket|The name of a bucket.|
    |content\_length\_in|The value of the Content-Length field in an HTTP request. Unit: bytes.|
    |content\_length\_out|The value of the Content-Length field in an HTTP response. Unit: bytes.|
    |object|The requested URL-encoded object. You can include the select url\_decode\(object\) clause in a query statement to decode the object.|
    |object\_size|The size of a requested object. Unit: bytes.|
    |operation|The API operation. For more information, see [Table 7](#table_o6k_8jw_p14).|
    |request\_uri|The URL-encoded URI of a request. This includes the query\_string parameter. You can include the select url\_decode\(request\_uri\) clause in a query statement to decode the URI.|
    |error\_code|The error code that is returned by OSS. For more information, see [Error responses](/intl.en-US/Developer Guide/Troubleshooting/Error responses.md).|
    |request\_length|The size of an HTTP request message that includes the header information. Unit: bytes.|
    |client\_ip|The IP address from which a request is sent. This can be the IP address of a client, firewall, or proxy.|
    |response\_body\_length|The size of an HTTP response body that excludes the header information.|
    |http\_method|The HTTP request method.|
    |referer|The HTTP Referer header.|
    |requester\_id|The ID of an Alibaba Cloud account that belongs to a requester. If you use anonymous logon, the value of this field is a hyphen \(-\).|
    |request\_id|The ID of a request.|
    |response\_time|The response duration. Unit: milliseconds.|
    |server\_cost\_time|The processing time of an OSS instance. Unit: milliseconds. The value of this field is the time that is required by the OSS instance to process a request.|
    |http\_type|The protocol of an HTTP request. Valid values: HTTP and HTTPS.|
    |sign\_type|The type of a signature. For more information, see [Table 9](#table_lj8_60k_knn).|
    |http\_status|The status code of an HTTP connection that is returned in a request to OSS.|
    |sync\_request|The type of a synchronization request. For more information, see [Table 8](#table_m3x_4em_7k4).|
    |bucket\_storage\_type|The bucket storage class. For more information, see [Table 6](#table_twd_9y6_bxl).|
    |host|The domain name of an OSS server from which resources are requested.|
    |vpc\_addr|The VPC IP address of an OSS server. The IP address is based on the domain name of the server.|
    |vpc\_id|VPC ID|
    |delta\_data\_size|The size change of an object. If the object size does not change, the value of this field is 0. If a request is not an upload request, the value of this field is a hyphen \(-\).|
    |acc\_access\_region|If a request is a transfer acceleration request, this field indicates the ID of the region where the requested access point resides. Otherwise, the value of this field is a hyphen \(-\).|

-   Batch deletion logs

    |Log field|Description|
    |---------|-----------|
    |\_\_topic\_\_|The topic of a log entry. Valid value: oss\_batch\_delete\_log.|
    |owner\_id|The ID of an Alibaba Cloud account.|
    |region|The region where a bucket resides.|
    |client\_ip|The IP address from which a request is sent. This can be the IP address of a client, firewall, or proxy.|
    |user\_agent|The User-Agent HTTP header.|
    |bucket|The name of a bucket.|
    |error\_code|The error code that is returned by OSS. For more information, see [Error responses](/intl.en-US/Developer Guide/Troubleshooting/Error responses.md).|
    |request\_length|The size of an HTTP request message that includes the header information. Unit: bytes.|
    |response\_body\_length|The size of an HTTP response body that excludes the header information.|
    |object|The requested URL-encoded object. You can include the select url\_decode\(object\) clause in a query statement to decode the object.|
    |object\_size|The size of a requested object. Unit: bytes.|
    |operation|The API operation. For more information, see [Table 7](#table_o6k_8jw_p14).|
    |bucket\_location|The cluster to which a bucket belongs.|
    |http\_method|The HTTP request method.|
    |referer|The HTTP Referer header.|
    |request\_id|The ID of a request.|
    |http\_status|The HTTP status code that is returned by an OSS request.|
    |sync\_request|The type of a synchronization request. For more information, see [Table 8](#table_m3x_4em_7k4).|
    |request\_uri|The URL-encoded URI of a request. This includes the query\_string parameter. You can include the select url\_decode\(request\_uri\) clause in a query statement to decode the URI.|
    |host|The domain name of an OSS server from which resources are requested.|
    |logging\_flag|Indicates whether logging has been enabled to export logs to OSS buckets at regular intervals.|
    |server\_cost\_time|The duration in which an OSS server processes a request. Unit: milliseconds.|
    |owner\_id|The ID of an Alibaba Cloud account that belongs to a bucket owner.|
    |requester\_id|The ID of an Alibaba Cloud account that belongs to a requester. If you use anonymous logon, the value of this field is a hyphen \(-\).|
    |delta\_data\_size|The size change of an object. If the object size does not change, the value of this field is 0. If a request is not an upload request, the value of this field is a hyphen \(-\).|

-   Hourly metering logs

    |Log field|Description|
    |---------|-----------|
    |\_\_topic\_\_|The topic of a log entry. Valid value: oss\_metering\_log.|
    |owner\_id|The ID of an Alibaba Cloud account that belongs to a bucket owner.|
    |bucket|The name of a bucket.|
    |cdn\_in|The inbound traffic from CDN. Unit: bytes.|
    |cdn\_out|The outbound traffic to CDN. Unit: bytes.|
    |get\_request|The number of GET requests.|
    |intranet\_in|The inbound traffic from the internal network. Unit: bytes.|
    |intranet\_out|The outbound traffic of the internal network. Unit: bytes.|
    |network\_in|The inbound traffic from the public network. Unit: bytes.|
    |network\_out|The outbound traffic to the public network. Unit: bytes.|
    |put\_request|The number of PUT requests.|
    |storage\_type|The bucket storage class. For more information, see [Table 6](#table_twd_9y6_bxl).|
    |storage|The storage usage of a bucket. Unit: bytes.|
    |metering\_datasize|The size of metering data of non-Standard OSS buckets.|
    |process\_img\_size|The size of a processed image. Unit: bytes.|
    |process\_img|The processed image.|
    |sync\_in|The inbound synchronization traffic. Unit: bytes.|
    |sync\_out|The outbound synchronization traffic. Unit: bytes.|
    |start\_time|The time when a metering operation starts.|
    |end\_time|The time when a metering operation ends.|
    |region|The region where a bucket resides.|


## ApsaraDB RDS

|Log field|Description|
|---------|-----------|
|\_\_topic\_\_|The topic of a log entry. Valid value: rds\_audit\_log.|
|owner\_id|The ID of an Alibaba Cloud account.|
|region|The region where an RDS instance resides.|
|instance\_name|The name of an RDS instance.|
|instance\_id|The ID of an RDS instance.|
|db\_type|The type of an RDS instance, for example, mysql, mssql, or pgsql.|
|db\_version|The version of an RDS instance.|
|check\_rows|The number of scanned rows.|
|db|The name of a database.|
|fail|Indicates the result after an SQL statement is executed. Valid values: -   0: successful
-   1: failed |
|client\_ip|The IP address of a client that accesses an RDS instance.|
|latency|The network latency. Unit: microseconds.|
|origin\_time|The time when an SQL statement is executed. Unit: microseconds.|
|return\_rows|The number of returned rows.|
|sql|The SQL statement.|
|thread\_id|The ID of a thread.|
|user|The name of a user who executes an SQL statement.|
|update\_rows|The number of updated rows.|

## Apsara File Storage NAS

|Log field|Description|
|---------|-----------|
|owner\_id|The ID of an Alibaba Cloud account.|
|ArgIno|The inode number of a file system.|
|AuthRc|The authorization code that is returned.|
|NFSProtocolRc|The return code of the Network File System \(NFS\) protocol.|
|OpList|The procedure number of the NFSv4 protocol.|
|Proc|The procedure number of the NFSv3 protocol.|
|RWSize|The size of read and write data. Unit: bytes.|
|RequestId|The ID of a request.|
|ResIno|The inode number of a resource that is looked up.|
|SourceIp|The IP address of a client.|
|Vers|The version number of the NFS protocol.|
|Vip|The IP address of a server.|
|Volume|The ID of a file system.|
|microtime|The time when a request is sent. Unit: microseconds.|

## Alibaba Cloud Mobile Push

|Log field|Description|
|---------|-----------|
|\_\_topic\_\_|The topic of a log entry. Valid value: cps\_callback\_event.|
|owner\_id|The ID of an Alibaba Cloud account.|
|app\_key|AppKey|
|message\_id|The ID of a message.|
|event\_time|The time when a callback event occurs.|
|event\_type|The type of a callback event.|
|device\_id|The ID of a device.|
|device\_type|The type of a device.|
|last\_active\_time|The last time when a device is active.|
|app\_version|The version of an application.|
|client\_ip|The IP address of a client.|
|brand|The brand of a device.|
|network\_type|The network type of a device.|
|os|The operating system of a device.|
|os\_version|The version of the operating system that runs on a device.|
|isp|The ISP of a device.|
|job\_key|The key of a job.|
|event\_channel|The push channel.|
|vendor\_message\_id|The message ID of a vendor channel.|
|reason|The cause of a failed push.|

## PolarDB for MySQL

|Log field|Description|
|---------|-----------|
|\_\_topic\_\_|The topic of a log entry. Valid value: polardb\_audit\_log.|
|owner\_id|The ID of an Alibaba Cloud account.|
|region|The region where a PolarDB for MySQL cluster resides.|
|cluster\_id|The ID of a PolarDB for MySQL cluster.|
|node\_id|The node IDs of PolarDB for MySQL.|
|check\_rows|The number of scanned rows.|
|db|The name of a database.|
|fail|Indicates the result after an SQL statement is executed. Valid values:-   0: successful
-   1: failed |
|client\_ip|The IP address of a client that accesses a PolarDB for MySQL cluster.|
|latency|The network latency. Unit: microseconds.|
|origin\_time|The time when an SQL statement is executed. Unit: microseconds.|
|return\_rows|The number of returned rows.|
|sql|The SQL statement.|
|thread\_id|The ID of a thread.|
|user|The name of a user who executes an SQL statement.|
|update\_rows|The number of updated rows.|

