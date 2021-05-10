# Value-added content functions

This topic describes the syntax of value-added content functions and provides parameter descriptions and examples. Value-added content functions can be used to enrich the information of some log fields. For example, you can obtain threat intelligence from an IP address and store the threat intelligence to log fields for log analysis.

## e\_threat\_intelligence

You can use the e\_threat\_intelligence function to obtain the threat intelligence from an IP address or a domain name in a log field and send the threat intelligence to a specified field.

-   If the scanned content does not contain threat intelligence, the content is not sent to the specified field. This prevents an impact on your data transformation tasks.
-   The Threat Intelligence service of Alibaba Cloud provides the threat intelligence of the last 30 days and updates the threat intelligence once a day. To obtain detailed threat intelligence, you can activate Threat Intelligence.

**Note:** The Threat Intelligence service is in public preview. You can call this function free of charge when you use the **Data Transformation** feature. The number of calls is not limited.

-   Syntax

    ```
    e_threat_intelligence(category, field, output_field=None, mode="overwrite")    
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |category|String|Yes|The type of the threat intelligence. Valid values:    -   ip: This value indicates that the threat intelligence about an IP address is obtained.
    -   domain: This value indicates that the threat intelligence about a domain name is obtained. |
    |field|String|Yes|The name of the log field from which the threat intelligence is obtained.|
    |output\_field|String|No|The name of the log field to which the threat intelligence is sent. If you do not set this parameter, threat intelligence is sent to the `__threat_intelligence__:field` field by default.|
    |mode|String|No|The overwrite mode of the field. Default value: overwrite. For more information, see [Field check and overwrite modes](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).|

-   Response

    The threat intelligence is returned in the JSON format to the field specified by the output\_field parameter. The following table describes the parameters.

    -   Threat intelligence from an IP address

        |Parameter|Description|
        |---------|-----------|
        |confidence|The confidence level of the threat intelligence. Value range: 0 to 100. A larger value indicates a higher confidence level.|
        |severity|The threat level of the threat intelligence.         -   0: riskless
        -   1: low risk
        -   2: medium risk
        -   3: high risk
        -   4: critical risk |
        |family|The malware family. The value null is returned.|
        |ioc\_type|The type of the IP address. Only IPv4 IP addresses are supported.|
        |ioc\_raw|The IP address from which the threat intelligence is obtained.|
        |intel\_type|The type of the risk tag. Multiple risk tags are separated by commas \(,\).         -   web\_attack: the IP address of network attacks.
        -   tor: the IP address of a Top of Rack \(TOR\) node.
        -   mining: the IP address of a miner.
        -   c2: the IP address of a command and control \(C2\) server.
        -   malicious: the IP address of the malicious download source.
        -   exploit: the IP address from which an exploit attack is initiated.
        -   webshell: the IP address from which a webshell attack is initiated.
        -   Scan: the IP address of network service scanning. |
        |country|The country to which the IP address belongs.|
        |province|The province to which the IP address belongs.|
        |city|The city to which the IP address belongs.|
        |isp|The telecom carrier of the network to which the IP address belongs.|

    -   Threat intelligence from a domain name

        |Parameter|Description|
        |---------|-----------|
        |confidence|The confidence level of the threat intelligence. Value range: 0 to 100. A larger value indicates a higher confidence level.|
        |severity|The threat level of the threat intelligence.         -   0: riskless
        -   1: low risk
        -   2: medium risk
        -   3: high risk
        -   4: critical risk |
        |family|The malware family. The value null is returned.|
        |ioc\_type|The domain name. The value domain is returned.|
        |ioc\_raw|The domain name from which the threat intelligence is obtained.|
        |intel\_type|The type of the risk tag. Multiple risk tags are separated by commas \(,\). For more information, see [Risk tag types](#section_aga_p3c_8bw).|
        |root\_domain|The root domain name to which the domain name belongs.|

-   Examples
    -   Scan the threat intelligence from an IP address and send the threat intelligence to a specified field.
        -   Raw log entry

            ```
            remote_addr: 203.0.113.1
            method: GET
            ```

        -   Transformation rule

            Obtain the threat intelligence of the IP address from the remote\_addr field and send the threat intelligence to the \_ti\_ field.

            ```
            e_threat_intelligence("ip", "remote_addr", output_field="_ti_")
            ```

        -   Result

            ```
            _ti_:{
                "confidence": 100,
                "severity": 4,
                "family": "",
                "ioc_raw": "203.0.113.1",
                "ioc_type": "ipv4",
                "intel_type": "web",
                "country": "China",
                "province": "Zhejiang",
                "city": "Hangzhou",
                "isp": "China Telecom"
                }
            method:GET
            remote_addr:203.0.113.1
            ```

    -   Scan the threat intelligence from an IP address and send the threat intelligence to the default field.
        -   Raw log entry

            ```
            remote_addr: 203.0.113.1
            method: GET
            ```

        -   Transformation rule

            Obtain the threat intelligence of the IP address from the remote\_addr field and send the threat intelligence to the default field.

            ```
            e_threat_intelligence("ip", "remote_addr")
            ```

        -   Result

            ```
            __threat_intelligence__:remote_addr:{
                "confidence": 100,
                "severity": 4,
                "family": "",
                "ioc_raw": "203.0.113.1",
                "ioc_type": "ipv4",
                "intel_type": "web",
                "country": "China",
                "province": "Zhejiang",
                "city": "Hangzhou",
                "isp": "China Telecom"
                }
            method:GET
            remote_addr:203.0.113.1
            ```

    -   Scan the threat intelligence from a domain name and send the threat intelligence to a specified field.
        -   Raw log entry

            ```
            domain_name: www.02a470ee85e5c43f27b9c42a3c46a8bb.info
            ```

        -   Transformation rule

            Obtain the threat intelligence of the domain name from the domain\_name field and send the threat intelligence to the \_ti\_ field.

            ```
            e_threat_intelligence("domain", "domain_name", output_field="_ti_")
            ```

        -   Result

            ```
            domain_name: www.02a470ee85e5c43f27b9c42a3c46a8bb.info
            _ti_: {
              "confidence": 91,
              "severity": 3,
              "family": "",
              "ioc_raw": "www.02a470ee85e5c43f27b9c42a3c46a8bb.info",
              "ioc_type": "domain",
              "root_domain": "02a470ee85e5c43f27b9c42a3c46a8bb.info",
              "intel_type": "sinkhole;rat_trojan;js_miner"
            }
            ```


## Appendix

|Risk tag|Description|Risk tag|Description|
|--------|-----------|--------|-----------|
|malware|Malware|botnet|Botnet|
|spy\_trojan|Trojan-spy|trojan|Trojan|
|worm|Worm|bank\_trojan|Banker trojan|
|ransomware|Ransomware|adware|Adware|
|backdoor\_trojan|Backdoor trojan|exploit|Vulnerability exploitation|
|hacktool|Hacking tool|malicious\_doc|Malicious document|
|infected\_virus|Infectious virus|bootkit\_trojan|Bootkit trojan|
|trojan\_dropper|Trojan dropper|script\_trojan|Trojan Script|
|riskware|Riskware|virus|Virus|
|apt|APT|trojan\_downloader|Trojan downloader|
|rat\_trojan|A remote access trojan \(RAT\)|rat|RAT|
|hijack|Hijack|ddos\_trojan|DDoS trojan|
|macro\_virus|Macro virus|spam\_email|Spam email|
|porn|Pornographic website|js\_miner|JavaScript miner|
|rootkit\_trojan|Rootkit trojan|compromised\_host|Compromised host|
|private\_server|Private server|gamble|Gambling website|
|c2|Central control server|dnslog\_attack|Dnslog attack|
|miner|Mining virus|infostealer|Infostealer|
|malicious\_group|Malicious group|malicious|Malicious website|
|sinkhole|Sinkhole|miner\_pool|Mining pool|
|dga|DGA|None|None|

