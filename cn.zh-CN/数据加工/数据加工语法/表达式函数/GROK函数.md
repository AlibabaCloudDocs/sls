# GROK函数

本文档主要介绍GROK函数的语法规则，包括参数解释、函数示例等。

## 函数介绍

由于[正则表达式函数](/cn.zh-CN/数据加工/数据加工语法/表达式函数/正则表达式函数.md)较为复杂，推荐您优先使用GROK函数。您也可以将GROK函数与正则表达式函数混合使用，如下所示：

```
e_match("content", grok(r"\w+: (%{IP})"))  #匹配abc: 192.168.0.0或者xyz: 192.168.1.1等形式。
e_match("content", grok(r"\w+: (%{IP})", escape=True)) #不会匹配abc: 192.168.0.0，而是匹配\w+: 192.168.0.0。
```

GROK函数根据正则表达式提取特定的值。

-   函数格式

    ```
    grok(pattern, escape=False, extend=None)
    ```

-   GROK语法

    ```
    %{SYNTAX} 
    %{SYNTAX:NAME}
    ```

    其中SYNTAX表示预定义正则模式，NAME表示分组。

    ```
    "%{IP}"               #等价于r"(?:\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})"
    "%{IP:source_id}"     #等价于r"(?P<source_id>\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})"
    ("%{IP}")             #等价于r"(\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})"
    ```

    GROK有两种分组模式：

    -   捕获分组模式

        GROK模式中部分是自带命名分组捕获的，所以针对这种模式只能使用%\{SYNTAX\}方式的语法。此类模式常见于语句解析，具体请参见[GROK模式参考](/cn.zh-CN/数据加工/数据加工语法/通用参考/GROK模式参考.md)中的Log formats模块。

        ```
        "%{SYSLOGBASE}"        
        "%{COMMONAPACHELOG}" 
        "%{COMBINEDAPACHELOG}"
        "%{HTTPD20_ERRORLOG}"
        "%{HTTPD24_ERRORLOG}"
        "%{HTTPD_ERRORLOG}"
        ...
        ```

    -   非捕获分组模式

        在GROK模式中部分是非捕获分组模式，例如：

        ```
        "%{INT}"    
        "%{YEAR}"
        "%{HOUR}"
        ...
        ```

-   参数说明

    |参数名称|参数类型|是否必填|说明|
    |----|----|----|--|
    |pattern|String|是|GROK语法。具体的GROK模式，请参见[GROK模式参考](/cn.zh-CN/数据加工/数据加工语法/通用参考/GROK模式参考.md)。|
    |escape|Bool|否|是否将其他非GROK pattern中的正则相关特殊字符做转义，默认不转义。|
    |extend|Dict|否|用户自定义的GROK表达式。|


## 函数示例

-   示例1：提取日期和引用内容。
    -   原始日志

        ```
        content: 2019 June 24 "I am iron man"
        ```

    -   加工规则

        ```
        e_regex('content',grok('%{YEAR:year} %{MONTH:month} %{MONTHDAY:day} %{QUOTEDSTRING:motto}'))
        ```

    -   加工结果

        ```
        content: 2019 June 24 "I am iron man"
        year: 2019
        month: June
        day: 24
        motto: "I am iron man"
        ```

-   示例2：提取HTTP请求日志。
    -   原始日志

        ```
        content: 10.0.0.0 GET /index.html 15824 0.043
        ```

    -   加工规则

        ```
        e_regex('content',grok('%{IP:client} %{WORD:method} %{URIPATHPARAM:request} %{NUMBER:bytes} %{NUMBER:duration}'))
        ```

    -   加工结果

        ```
        content: 10.0.0.0 GET /index.html 15824 0.043
        client: 10.0.0.0
        method: GET
        request: /index.html
        bytes: 15824
        duration: 0.043
        ```

-   示例3：提取Apache日志。
    -   原始日志

        ```
        content: 127.0.0.1 - - [13/Apr/2015:17:22:03 +0800] "GET /router.php HTTP/1.1" 404 285 "-" "curl/7.19.7 (x86_64-redhat-linux-gnu) libcurl/7.19.7 NSS/3.15.3 zlib/1.2.3 libidn/1.18 libssh2/1.4.2"
        ```

    -   加工规则

        ```
        e_regex('content',grok('%{COMBINEDAPACHELOG}'))
        ```

    -   加工结果

        ```
        content: 127.0.0.1 - - [13/Apr/2015:17:22:03 +0800] "GET /router.php HTTP/1.1" 404 285 "-" "curl/7.19.7 (x86_64-redhat-linux-gnu) libcurl/7.19.7 NSS/3.15.3 zlib/1.2.3 libidn/1.18 libssh2/1.4.2"
        clientip: 127.0.0.1
        ident: -
        auth: -
        timestamp: 13/Apr/2015:17:22:03 +0800
        verb: GET
        request: /router.php
        httpversion: 1.1
        response: 404
        bytes: 285
        referrer: "-"
        agent: "curl/7.19.7 (x86_64-redhat-linux-gnu) libcurl/7.19.7 NSS/3.15.3 zlib/1.2.3 libidn/1.18 libssh2/1.4.2"
        ```

-   示例4：Syslog默认格式日志。
    -   原始日志

        ```
        content: May 29 16:37:11 sadness logger: hello world
        ```

    -   加工规则

        ```
        e_regex('content',grok('%{SYSLOGBASE} %{DATA:message}'))
        ```

    -   加工结果

        ```
        content: May 29 16:37:11 sadness logger: hello world
        timestamp: May 29 16:37:11
        logsource: sadness
        program: logger
        message: hello world
        ```

-   示例5：转义特殊字符。
    -   原始日志

        ```
        content: Nov  1 21:14:23 scorn kernel: pid 84558 (expect), uid 30206: exited on signal 3
        ```

    -   加工规则

        ```
        e_regex('content',grok(r'%{SYSLOGBASE} pid %{NUMBER:pid} \(%{WORD:program}\), uid %{NUMBER:uid}: exited on signal %{NUMBER:signal}'))
        ```

        因为加工规则中包含了正则特殊字符括号\(\)，如果您不使用转义符，则添加escape=True参数即可，如下所示：

        ```
        e_regex('content',grok('%{SYSLOGBASE} pid %{NUMBER:pid} (%{WORD:program}), uid %{NUMBER:uid}: exited on signal %{NUMBER:signal}', escape=True))
        ```

    -   加工结果

        ```
        content: Nov  1 21:14:23 scorn kernel: pid 84558 (expect), uid 30206: exited on signal 3
        timestamp: Nov  1 21:14:23
        logsource: scorn
        program: expect
        pid: 84558
        uid: 30206
        signal: 3
        ```

-   示例6：用户自定义GROK表达式。
    -   原始日志

        ```
        content: Beijing-1104,gary 25 "never quit"
        ```

    -   加工规则

        ```
        e_regex('content',grok('%{ID:user_id},%{WORD:name} %{INT:age} %{QUOTEDSTRING:motto}',extend={'ID': '%{WORD}-%{INT}'}))
        ```

    -   加工结果

        ```
        content: Beijing-1104,gary 25 "never quit"
        user_id: Beijing-1104
        name: gary
        age: 25
        motto: "never quit"
        ```

-   示例7：匹配JSON数据。
    -   原始日志

        ```
        content: 2019-10-29 16:41:39,218 - INFO: owt.AudioFrameConstructor - McsStats: {"event":"mediaStats","connectionId":"331578616547393100","durationMs":"5000","rtpPackets":"250","rtpBytes":"36945","nackPackets":"0","nackBytes":"0","rtpIntervalAvg":"20","rtpIntervalMax":"104","rtpIntervalVar":"4","rtcpRecvPackets":"0","rtcpRecvBytes":"0","rtcpSendPackets":"1","rtcpSendBytes":"32","frame":"250","frameBytes":"36945","timeStampOutOfOrder":"0","frameIntervalAvg":"20","frameIntervalMax":"104","frameIntervalVar":"4","timeStampIntervalAvg":"960","timeStampIntervalMax":"960","timeStampIntervalVar":"0"}
        ```

    -   加工规则

        ```
        e_regex('content',grok('%{EXTRACTJSON}'))
        ```

    -   加工结果

        ```
        content: 2019-10-29 16:41:39,218 - INFO: owt.AudioFrameConstructor - McsStats: {"event":"mediaStats","connectionId":"331578616547393100","durationMs":"5000","rtpPackets":"250","rtpBytes":"36945","nackPackets":"0","nackBytes":"0","rtpIntervalAvg":"20","rtpIntervalMax":"104","rtpIntervalVar":"4","rtcpRecvPackets":"0","rtcpRecvBytes":"0","rtcpSendPackets":"1","rtcpSendBytes":"32","frame":"250","frameBytes":"36945","timeStampOutOfOrder":"0","frameIntervalAvg":"20","frameIntervalMax":"104","frameIntervalVar":"4","timeStampIntervalAvg":"960","timeStampIntervalMax":"960","timeStampIntervalVar":"0"}
        json:{"event":"mediaStats","connectionId":"331578616547393100","durationMs":"5000","rtpPackets":"250","rtpBytes":"36945","nackPackets":"0","nackBytes":"0","rtpIntervalAvg":"20","rtpIntervalMax":"104","rtpIntervalVar":"4","rtcpRecvPackets":"0","rtcpRecvBytes":"0","rtcpSendPackets":"1","rtcpSendBytes":"32","frame":"250","frameBytes":"36945","timeStampOutOfOrder":"0","frameIntervalAvg":"20","frameIntervalMax":"104","frameIntervalVar":"4","timeStampIntervalAvg":"960","timeStampIntervalMax":"960","timeStampIntervalVar":"0"}
        ```

-   示例8：解析标准w3c格式日志。
    -   原始日志

        ```
        content: 2018-12-26 00:00:00 W3SVC2 application001 192.168.0.0 HEAD / - 8000 - 10.0.0.0 HTTP/1.0 - - - - 404 0 64 0 19 0
        ```

    -   加工规则

        w3c中没有的字段使用了短划线（-）替代，在GROK中也使用短划线（-）去匹配这些字段。

        ```
        e_regex("content",grok('%{DATE:data} %{TIME:time} %{WORD:s_sitename} %{WORD:s_computername} %{IP:s_ip} %{WORD:cs_method} %{NOTSPACE:cs_uri_stem} - %{NUMBER:s_port} - %{IP:c_ip} %{NOTSPACE:cs_version} - - - - %{NUMBER:sc_status} %{NUMBER:sc_substatus} %{NUMBER:sc_win32_status} %{NUMBER:sc_bytes} %{NUMBER:cs_bytes} %{NUMBER:time_taken}'))
        ```

    -   加工结果

        ```
        content: 2018-12-26 00:00:00 W3SVC2 application001 192.168.0.0 HEAD / - 8000 - 10.0.0.0 HTTP/1.0 - - - - 404 0 64 0 19 0 
        data: 18-12-26
        time: 00:00:00
        s_sitename: W3SVC2
        s_computername: application001
        s_ip: 192.168.0.0
        cs_method: HEAD 
        cs_uri_stem: /
        s_port: 8000
        c_ip: 10.0.0.0
        cs_version: HTTP/1.0
        sc_status: 404
        sc_substatus: 0
        sc_win32_status: 64 
        sc_bytes: 0 
        cs_bytes: 19 
        time_taken: 0
        ```


