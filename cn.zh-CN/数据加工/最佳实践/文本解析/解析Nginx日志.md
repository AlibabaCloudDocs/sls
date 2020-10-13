# 解析Nginx日志

Nginx访问日志记录了用户访问的详细信息，解析Nginx访问日志对业务运维具有重要意义。本文介绍如何使用正则表达式函数或GROK函数解析Nginx访问日志。

## 解析方案简介

日志服务支持通过数据加工的正则表达式和GROK解析Nginx日志，两种方案对比如下：

-   正则表达式方案

    对不擅长正则表达式的分析人员，使用正则表达式解析日志时会存在使用难度高、加工效率低、学习成本高和灵活性不足的问题。不推荐优先使用。详情请参见[正则表达式](/cn.zh-CN/数据加工/数据加工语法/通用参考/正则表达式.md)。

-   GROK方案（推荐）

    GROK学习成本低，只需要了解不同模式所代表的字段类型，就可以轻松解析日志内容。从灵活性、高效性、低成本、学习曲线等方面对比，GROK都要比直接使用正则表达式有优势。当前SLS数据加工已经提供了四百种模式，建议优先使用GROK模式。详情请参见[GROK模式参考](/cn.zh-CN/数据加工/数据加工语法/通用参考/GROK模式参考.md)。


**说明：**

-   GROK模式的本质是正则表达式，在有需要的情况下，可以组合使用GROK与正则表达式。
-   自定义的Nginx日志格式，可以通过正则表达式或GROK方案的调整实现解析。

## 使用正则表达式解析Nginx成功访问日志

现以一条Nginx成功访问日志为例，介绍如何使用正则表达式解析Nginx成功访问日志。

-   原始日志

    ```
    __source__:  192.168.0.1
    __tag__:__client_ip__:  192.168.254.254
    __tag__:__receive_time__:  1563443076
    content: 192.168.0.2 - - [04/Jan/2019:16:06:38 +0800] "GET http://cdn1cdedge0001.coxlab.net/_astats?application=&inf.name=eth0 HTTP/1.1" 200 273932 "-" "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"
    ```

-   解析需求
    -   需求1：从Nginx日志中提取出code、ip、datetime、protocol、request、sendbytes、refere、useragent、verb信息。
    -   需求2：对request进行再提取，提取出uri\_proto、uri\_domain、uri\_param信息。
    -   需求3：对解析出来的uri\_param进行再提取，提取出uri\_path、uri\_query信息。
-   SLS DSL编排
    -   总编排

        ```
        """第一步：初步解析Nginx日志"""
        e_regex("content",r'(?P<ip>\d+\.\d+\.\d+\.\d+)( - - \[)(?P<datetime>[\s\S]+)\] \"(?P<verb>[A-Z]+) (?P<request>[\S]*) (?P<protocol>[\S]+)["] (?P<code>\d+) (?P<sendbytes>\d+) ["](?P<refere>[\S]*)["] ["](?P<useragent>[\S\s]+)["]')
        """第二步：解析第一步得到的request"""
        e_regex('request',r'(?P<uri_proto>(\w+)):\/\/(?P<uri_domain>[a-z0-9.]*[^\/])(?P<uri_param>(.+)$)')
        """第三步：解析第二步得到的uri_param参数"""
        e_regex('uri_param',r'(?P<uri_path>\/\_[a-z]+[^?])\?(?<uri_query>(.+)$)')
        ```

    -   细分编排及对应加工结果
        -   针对需求1的加工编排如下。

            ```
            e_regex("content",r'(?P<ip>\d+\.\d+\.\d+\.\d+)( - - \[)(?P<datetime>[\s\S]+)\] \"(?P<verb>[A-Z]+) (?P<request>[\S]*) (?P<protocol>[\S]+)["] (?P<code>\d+) (?P<sendbytes>\d+) ["](?P<refere>[\S]*)["] ["](?P<useragent>[\S\s]+)["]')
            ```

            对应加工结果

            ```
            __source__:  192.168.0.1
            __tag__:  __receive_time__:  1563443076
            code:  200
            content:  192.168.0.2 - - [04/Jan/2019:16:06:38 +0800] "GET http://cdn1cdedge0001.coxlab.net/_astats?application=&amp;inf.name=eth0 HTTP/1.1" 200 273932 "-" "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"httpversion:  1.1
            datetime:  04/Jan/2019:16:06:38 +0800
            ip:  192.168.0.2
            protocol:  HTTP/1.1
            refere:  -
            request:  http://cdn1cdedge0001.coxlab.net/_astats?application=&amp;inf.name=eth0
            sendbytes:  273932
            useragent:  Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)
            verb:  GET
            ```

        -   针对需求2解析request，加工编排如下。

            ```
            e_regex('request',r'(?P<uri_proto>(\w+)):\/\/(?P<uri_domain>[a-z0-9.]*[^\/])(?P<uri_param>(.+)$)')
            ```

            对应加工结果：

            ```
            uri_param: /_astats?application=&inf.name=eth0
            uri_domain: cdn1cdedge0001.coxlab.net
            uri_proto: http
            ```

        -   针对需求3解析uri\_param，加工编排如下。

            ```
            e_regex('uri_param',r'(?P<uri_path>\/\_[a-z]+[^?])\?(?<uri_query>(.+)$)')
            ```

            对应加工结果

            ```
            uri_path: /_astats
            uri_query: application=&inf.name=eth0
            ```

-   加工结果

    ```
    __source__:  192.168.0.1
    __tag__:  __receive_time__:  1563443076
    code:  200
    content:  192.168.0.2 - - [04/Jan/2019:16:06:38 +0800] "GET http://cdn1cdedge0001.coxlab.net/_astats?application=&amp;inf.name=eth0 HTTP/1.1" 200 273932 "-" "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"httpversion:  1.1
    datetime:  04/Jan/2019:16:06:38 +0800
    ip:  192.168.0.2
    protocol:  HTTP/1.1
    refere:  -
    request:  http://cdn1cdedge0001.coxlab.net/_astats?application=&amp;inf.name=eth0
    sendbytes:  273932
    uri_domain:  cdn1cdedge0001.coxlab.net
    uri_proto:  http
    uri_param: /_astats?application=&inf.name=eth0
    uri_path: /_astats 
    uri_query: application=&inf.name=eth0
    useragent:  Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)
    verb:  GET
    ```


## 使用GROK解析Nginx成功访问日志

现以一条Nginx成功访问日志为例，介绍如何使用GROK解析Nginx成功访问日志。

-   原始日志

    ```
    __source__:  192.168.0.1
    __tag__:__client_ip__:  192.168.254.254
    __tag__:__receive_time__:  1563443076
    content: 192.168.0.2 - - [04/Jan/2019:16:06:38 +0800] "GET http://cdn1cdedge0001.coxlab.net/_astats?application=&inf.name=eth0 HTTP/1.1" 200 273932 "-" "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"
    ```

-   解析需求
    -   需求1：从Nginx日志中提取出clientip、bytes、agent、auth、verb、request、identtimestamp, httpversion、response、bytes、referrer信息。
    -   需求2：对解析出来的request进行再提取，提取出uri\_proto、uri\_domain、uri\_param信息。
    -   需求3：对解析出来的uri\_param进行再提取，提取出uri\_path、uri\_query信息。
-   SLS DSL编排
    -   总编排

        ```
        """第一步：初步解析Nginx日志"""
        e_regex('content',grok('%{COMBINEDAPACHELOG}'))
        """第二步：解析第一步得到的request"""
        e_regex('request',grok("%{URIPROTO:uri_proto}://(?:%{USER:user}(?::[^@]*)?@)?(?:%{URIHOST:uri_domain})?(?:%{URIPATHPARAM:uri_param})?"))
        """第三步：解析第二步得到的uri_param参数"""
        e_regex('uri_param',grok("%{GREEDYDATA:uri_path}\?%{GREEDYDATA:uri_query}"))
        ```

        使用GROK模式解析Nginx正确访问日志，只需要`COMBINEDAPACHELOG`模式即可。

        |模式|规则|说明|
        |--|--|--|
        |COMMONAPACHELOG|`%{IPORHOST:clientip} %`

`{HTTPDUSER:ident} %`

`{USER:auth} \[%`

`{HTTPDATE:timestamp}\] "(?:%`

`{WORD:verb} %`

`{NOTSPACE:request}(?: HTTP/%`

`{NUMBER:httpversion})?|%`

`{DATA:rawrequest})" %`

`{NUMBER:response} (?:%`

`{NUMBER:bytes}|-)`

|解析出clientip、ident、auth、timestamp、verb、request、httpversion、response、bytes信息。|
        |COMBINEDAPACHELOG|`%{COMMONAPACHELOG} %`

`{QS:referrer} %{QS:agent}`

|解析出上一行中所有字段，另外还解析出referrer、agent字段。|

    -   细分编排及对应加工结果
        -   针对需求1解析Nginx日志的加工编排如下。

            ```
            e_regex('content',grok('%{COMBINEDAPACHELOG}'))
            ```

            对应加工结果

            ```
            clientip: 192.168.0.1
            __tag__:  __receive_time__:  1563443076
            agent:  "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"
            auth:  -
            bytes:  273932
            clientip:  192.168.0.2
            content:  192.168.0.2 - - [04/Jan/2019:16:06:38 +0800] "GET http://cdn1cdedge0001.coxlab.net/_astats?application=&amp;inf.name=eth0 HTTP/1.1" 200 273932 "-" "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"
            httpversion:  1.1
            ident:  -
            referrer:  "-"
            request:  http://cdn1cdedge0001.coxlab.net/_astats?application=&amp;inf.name=eth0
            response:  200
            timestamp:  04/Jan/2019:16:06:38 +0800
            verb:  GET
            ```

        -   针对需求2解析request，加工编排如下。

            ```
            e_regex('request',grok("%{URIPROTO:uri_proto}://(?:%{USER:user}(?::[^@]*)?@)?(?:%{URIHOST:uri_domain})?(?:%{URIPATHPARAM:uri_param})?"))
            ```

            对应加工结果

            ```
            uri_proto: http
            uri_domain: cdn1cdedge0001.coxlab.net
            uri_param: /_astats?application=&inf.name=eth0
            ```

            特别说明，只需要使用GROK的以下数种模式组合即可对request完成解析，如下表所示。

            |模式|规则|说明|
            |--|--|--|
            |URIPROTO|`[A-Za-z]+(\+[A-Za-z+]+)?`|匹配uri中的头部分。例如`http://hostname.domain.tld/_astats?application=&inf.name=eth0`会匹配到http。|
            |USER|`[a-zA-Z0-9._-]+`|匹配字母、数字和符号（`._-`）组合。|
            |URIHOST|`%{IPORHOST}(?::%`|匹配IPORHOST和POSINT。|
            |URIPATHPARAM|`%{URIPATH}(?:%{URIPARAM})?`|匹配uri参数部分。|

        -   针对需求3解析uri\_param，加工编排如下。

            ```
            e_regex('uri_param',grok("%{GREEDYDATA:uri_path}\?%{GREEDYDATA:uri_query}"))
            ```

            对应加工结果：

            ```
            uri_path: /_astats
            uri_query: application=&inf.name=eth0
            ```

            使用GROK的以下模式即可完成对uri\_param的解析，如下表所示。

            |模式|规则|说明|
            |--|--|--|
            |GREEDYDATA|`.*`|匹配任意或多个除换行符。|

-   加工结果

    ```
    __source__:  192.168.0.1
    __tag__:__receive_time__:  1563443076
    agent:  "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"
    auth:  -
    bytes:  273932
    clientip:  192.168.0.2
    content:  192.168.0.2 - - [04/Jan/2019:16:06:38 +0800] "GET http://cdn1cdedge0001.coxlab.net/_astats?application=&amp;inf.name=eth0 HTTP/1.1" 200 273932 "-" "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"
    httpversion:  1.1
    ident:  -
    referrer:  "-"
    request:  http://cdn1cdedge0001.coxlab.net/_astats?application=&amp;inf.name=eth0
    response:  200
    timestamp:  04/Jan/2019:16:06:38 +0800
    uri_domain:  cdn1cdedge0001.coxlab.net
    uri_param:  /_astats?application=&amp;inf.name=eth0
    uri_path:  /_astats
    uri_proto:  http
    uri_query:  application=&amp;inf.name=eth0
    verb:  GET
    ```


## 使用GROK解析Nginx失败访问日志

以下以一条Nginx失败访问日志为例，介绍如何使用GROK解析Nginx失败访问日志。

-   原始日志

    ```
    __source__:  192.168.0.1
    __tag__:__client_ip__:  192.168.254.254
    __tag__:__receive_time__:  1563443076
    content: 2019/08/07 16:05:17 [error] 1234#1234: *1234567 attempt to send data on a closed socket: u:111111ddd, c:0000000000000000, ft:0 eof:0, client: 1.2.3.4, server: sls.aliyun.com, request: "GET /favicon.ico HTTP/1.1", host: "sls.aliyun.com", referrer: "https://sls.aliyun.com/question/answer/123.html?from=singlemessage"
    ```

-   解析需求

    从content中解析出错误访问日志host、http\_version、log\_level、pid、referrerrequest、request\_time、server、verb信息。

-   SLS DSL编排

    ```
    e_regex('content',grok('%{DATESTAMP:request_time} \[%{LOGLEVEL:log_level}\] %{POSINT:pid}#%{NUMBER}: %{GREEDYDATA:errormessage}(?:, client: (?<client>%{IP}|%{HOSTNAME}))(?:, server: %{IPORHOST:server})(?:, request: "%{WORD:verb} %{NOTSPACE:request}( HTTP/%{NUMBER:http_version})")(?:, host: "%{HOSTNAME:host}")?(?:, referrer: "%{NOTSPACE:referrer}")?'))
    ```

-   加工结果

    ```
    ___source__:  192.168.0.1
    __tag__:__client_ip__:  192.168.254.254
    __tag__:__receive_time__:  1563443076
    content:  2019/08/07 16:05:17 [error] 1234#1234: *1234567 attempt to send data on a closed socket: u:111111ddd, c:0000000000000000, ft:0 eof:0, client: 1.2.3.4, server: sls.aliyun.com, request: "GET /favicon.ico HTTP/1.1", host: "sls.aliyun.com", referrer: "https://sls.aliyun.com/question/answer/123.html?
    host: sls.aliyun.com
    http_version: 1.1
    log_level: error
    pid: 1234
    referrer: https://sls.aliyun.com/question/answer/123.html?from=singlemessage
    request: /favicon.ico
    request_time:  19/08/07 16:05:17
    server: sls.aliyun.com
    verb: GET
    ```


