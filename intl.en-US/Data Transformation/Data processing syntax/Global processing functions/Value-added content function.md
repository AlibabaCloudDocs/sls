# Value-added content function

This topic describes the syntax rules of value-added content functions and provides parameter descriptions and examples. The value-added content function can be used to enrich the information of some log fields. For example, you can obtain the threat intelligence of an IP address and store the threat intelligence to log fields for log analysis.

## e\_threat\_intelligence

You can use the e\_threat\_intelligence function to obtain the threat intelligence of an IP address in a log field and send the threat intelligence to a specified field.

-   If the scanned content does not contain threat intelligence, the content is not sent to the specified field. This prevents impact on your data transformation tasks.
-   The Threat Intelligence service of Alibaba Cloud provides threat intelligence of the last 30 days and updates the threat intelligence once a day. To obtain detailed threat intelligence, you can activate Threat Intelligence.

**Note:**

-   The Threat Intelligence service is in public preview. You can call the e\_threat\_intelligence function free of charge. The number of calls is not limited. The public review is supported in the following regions:
    -   China \(Beijing\)
    -   China North 2 Ali Gov 1
    -   China \(Hohhot\)
    -   China \(Chengdu\)
-   During the public preview, you can choose go to the **Data Transformation** page and then click **Preview Data** in the Log Service console to use this function in all regions.

-   Syntax

    ```
    e_threat_intelligence(category, field, output_field=None, mode="overwrite")    
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |category|String|Yes|The type of the threat intelligence. Set the value to ip. This value indicates that the threat intelligence of an IP address is obtained.|
    |field|String|Yes|The name of the log field from which the threat intelligence is obtained.|
    |output\_field|String|No|The name of the log field to which the threat intelligence is sent. If you do not set this parameter, threat intelligence is sent to the `__threat_intelligence__:field` field by default.|
    |mode|String|No|The overwrite mode of the field. Default value: overwrite. For more information, see [Field check and overwrite modes](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).|

-   Response

    The threat intelligence is returned in the JSON format to the field specified by the output\_field parameter. The following table describes the parameters of the threat intelligence.

    |Parameter|Description|
    |---------|-----------|
    |confidence|The confidence level of the threat intelligence. Valid values: 0 to 100. A larger value indicates a higher confidence level.|
    |severity|The threat level of the threat intelligence.    -   0: riskless
    -   1: low risk
    -   2: medium risk
    -   3: high risk
    -   4: critical risk |
    |family|The malware family. Set the value to null.|
    |ioc\_type|The type of the IP address. Only IPv4 IP addresses are supported.|
    |ioc\_raw|The IP address from which the threat intelligence is obtained.|
    |intel\_type|The type of the risk tag. Separate multiple tags with commas \(,\).    -   web\_attack: the IP address of network attacks.
    -   tor: the IP address of a Top of Rack \(TOR\) node.
    -   mining: the IP address of a miner.
    -   c2: the IP address of a command and control \(C2\) server.
    -   malicious: the IP address of the malicious download source.
    -   exploit: the IP address from which an exploit attack is initiated.
    -   webshell: the IP address from which the webshell attack is initiated.
    -   Scan: the IP address of network service scanning. |
    |country|The country to which the IP address belongs.|
    |province|The province to which the IP address belongs.|
    |city|The city to which the IP address belongs.|
    |isp|The telecom carrier of the network to which the IP address belongs.|

-   Examples
    -   Send threat intelligence to a specified field
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
                "country" : "China",
                "province": "Zhejiang",
                "city" : "Hangzhou",
                "Isp": "China Telecom",
                }
            method:GET
            remote_addr:203.0.113.1
            ```

    -   Send threat intelligence to a default field
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
                "country" : "China",
                "province": "Zhejiang",
                "city" : "Hangzhou",
                "Isp": "China Telecom",
                }
            method:GET
            remote_addr:203.0.113.1
            ```


