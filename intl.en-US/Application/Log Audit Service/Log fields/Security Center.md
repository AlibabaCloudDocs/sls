# Security Center

This topic describes the fields of Security Center logs. Security Center logs include network logs, security logs, and host logs.

## Network logs

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
    |in\_out|The direction of data flows. Valid values:    -   in: inbound data flows
    -   out: outbound data flows |
    |qid|The ID of a query.|
    |qname|The domain name to be queried.|
    |qtype|The type of a resource to be queried.|
    |query\_datetime|The timestamp of a query. Unit: milliseconds.|
    |rcode|The code of a response.|
    |region|The ID of a source region. Valid values:    -   1: China \(Beijing\)
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
    |time\_usecond|The response time. Unit: microseconds.|
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
    |referer|The Referer HTTP header. This field includes the address of the web page that sends a request.|
    |request\_datetime|The time when a request is sent.|
    |ret\_code|The HTTP status code.|
    |rqs\_content\_type|The content type of an HTTP request message.|
    |rsp\_content\_type|The content type of an HTTP response message.|
    |src\_ip|The IP address of a source server.|
    |src\_port|The source port.|
    |uri|The URI of a request.|
    |user\_agent|The user agent of a client that sends a request.|
    |x\_forward\_for|The X-Forwarded-For \(XFF\) HTTP header.|


## Security logs

-   Vulnerability logs

    |Log field|Description|
    |---------|-----------|
    |\_\_topic\_\_|The topic of a log entry. Valid value: sas-vul-log.|
    |owner\_id|The ID of an Alibaba Cloud account.|
    |name|The name of a vulnerability.|
    |alias\_name|The alias of a vulnerability.|
    |op|The action that is performed on a vulnerability. Valid values:     -   new: detects a new vulnerability.
    -   verify: verifies the vulnerability.
    -   fix: fixes the vulnerability. |
    |status|The status of a vulnerability. For more information, see [Table 2](#table_zcd_26q_vi1).|
    |tag|The tag of a vulnerability, for example, oval, system, or cms. This field is used to distinguish between different emergency \(EMG\) vulnerabilities.|
    |type|The type of a vulnerability. Valid values:    -   sys: Windows vulnerability
    -   cve: Linux vulnerability
    -   cms: Web CMS vulnerability
    -   EMG: Emergency vulnerability |
    |uuid|The universally unique identifier \(UUID\) of a client.|

-   Baseline logs

    |Log field|Description|
    |---------|-----------|
    |\_\_topic\_\_|The topic of a log entry. Valid value: sas-hc-log.|
    |owner\_id|The ID of an Alibaba Cloud account.|
    |level|The level of a baseline.|
    |op|The action that is performed on a baseline. Valid values:    -   new: detects a new baseline.
    -   verify: verifies the baseline |
    |risk\_name|The name of a baseline risk.|
    |status|The status of a baseline. For more information, see [Table 2](#table_zcd_26q_vi1).|
    |sub\_type\_alias|The subtype alias of a baseline.|
    |sub\_type\_name|The subtype of a baseline.|
    |type\_name|The type of a baseline. For more information, see [Table 1](#table_pd5_rqj_a8d).|
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
    |\_\_topic\_\_|The topic of a log entry. Valid value: sas-security-log.|
    |data\_source|The data source. For more information, see [Table 3](#table_iwf_4vy_jpn).|
    |level|The severity of an alert.|
    |name|The name of an alert.|
    |op|The action that is performed on an alert. Valid values:    -   new: An alert is triggered.
    -   dealing: The alert is being processed. |
    |status|The status of an alert. For more information, see [Table 2](#table_zcd_26q_vi1).|
    |uuid|The UUID of a client.|
    |detail|The details of an alert.|
    |unique\_info|The unique identifier of an alert for a single server.|

    |Value|Description|
    |-----|-----------|
    |aegis\_suspicious\_event|Server exceptions|
    |aegis\_suspicious\_file\_v2|Webshell|
    |aegis\_login\_log|Suspicious logon|
    |security\_event|Security Center exceptions|


## Host logs

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
    |proto|The protocol that is used to establish a network connection.|
    |status|The connection status. For more information, see [Table 4](#table_e3e_l2j_flw).|

    |Status code|Description|
    |-----------|-----------|
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
    |proto|The protocol that is used by a listener.|
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
    |op|The action that is performed on a vulnerability. Valid values:    -   new: detects a new vulnerability.
    -   verify: verifies the vulnerability.
    -   fix: fixes the vulnerability. |
    |status|The connection status. For more information, see [Table 4](#table_e3e_l2j_flw).|
    |tag|The tag of a vulnerability, for example, oval, system, or cms. This field is used to distinguish between different emergency \(EMG\) vulnerabilities.|
    |type|The type of a vulnerability. Valid values:    -   sys: Windows vulnerability
    -   cve: Linux vulnerability
    -   cms: Web CMS vulnerability
    -   EMG: Emergency vulnerability |
    |uuid|The UUID of a client.|


