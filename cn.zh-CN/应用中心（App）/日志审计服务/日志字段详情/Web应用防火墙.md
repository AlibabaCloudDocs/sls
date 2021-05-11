# Web应用防火墙

本文介绍Web应用防火墙访问日志的字段详情。

|字段|说明|
|--|--|
|\_\_topic\_\_|日志主题，固定为waf\_access\_log。|
|owner\_id|阿里云账号ID|
|acl\_action|WAF精准访问控制规则行为，例如pass、drop、captcha。空值或短划线（-）也表示pass。 |
|acl\_blocks|是否被精准访问控制规则拦截，包括：-   1表示拦截。
-   其他值均表示通过。 |
|antibot|触发的爬虫风险管理防护策略类型，详情说明如下：-   ratelimit：频率控制
-   sdk：APP端增强防护
-   algorithm：算法模型
-   intelligence：爬虫情报
-   acl：精准访问控制
-   blacklist：黑名单 |
|antibot\_action|爬虫风险管理防护策略执行的操作，详细说明如下：-   challenge：嵌入JS进行验证
-   drop：拦截
-   report：记录
-   captcha：滑块验证 |
|block\_action|触发拦截的WAF防护类型，详细说明如下：-   tmd：CC攻击防护
-   waf：Web应用攻击防护
-   acl：精准访问控制
-   geo：地区封禁
-   antifraud：数据风控
-   antibot：防爬封禁 |
|body\_bytes\_sent|发送给客户端的HTTP Body字节数|
|cc\_action|CC防护策略行为，例如none、challenge、pass、close、captcha、wait、login、n等。|
|cc\_blocks|是否被CC防护功能拦截，包括：-   1表示拦截。
-   其他值均表示通过。 |
|cc\_phase|触发的CC防护策略，包括seccookie、server\_ip\_blacklist、static\_whitelist、server\_header\_blacklist、server\_cookie\_blacklist、server\_args\_blacklist、qps\_overmax等。|
|content\_type|访问请求内容类型|
|host|源站服务器|
|http\_cookie|访问请求头部中带有的访问来源客户端Cookie信息|
|http\_referer|访问请求头部中带有的访问请求的来源URL信息。如果无来源URL信息，则显示为短划线（-）。|
|http\_user\_agent|访问请求头部中的User Agent字段，一般包含来源客户端浏览器标识、操作系统标识等信息。|
|http\_x\_forwarded\_for|访问请求头部中带有的XFF头信息，用于识别通过HTTP代理或负载均衡方式连接到Web服务器的客户端最原始的IP地址。|
|https|访问请求是否为HTTPS请求，包括：-   true：HTTPS请求
-   false：HTTP请求 |
|matched\_host|匹配到的已接入WAF防护配置的域名，可能是泛域名。如果无法匹配到相关域名配置，则显示短划线（-）。|
|querystring|请求中的查询字符串|
|real\_client\_ip|访问的客户端的真实IP地址。如果无法获取，则显示为短划线（-）。|
|threat\_real\_client\_ip|访问客户端的真实IP地址的威胁情报。更多信息，请参见[威胁情报字段](/cn.zh-CN/应用中心（App）/日志审计服务/生成威胁情报.md)。|
|region|WAF实例地域信息|
|remote\_addr|访问请求的客户端IP地址|
|remote\_port|访问请求的客户端端口|
|request\_length|访问请求长度，单位：字节。|
|request\_method|访问请求的HTTP请求方法|
|request\_path|请求的相对路径（不包含查询字符串）|
|request\_time\_msec|访问请求时间，单位：毫秒。|
|request\_traceid|WAF记录的访问请求唯一ID标识|
|server\_protocol|源站服务器响应的协议及版本号|
|status|WAF返回给客户端的HTTP响应状态信息|
|time|访问请求发生的时间|
|ua\_browser|访问请求来源的浏览器信息|
|ua\_browser\_family|访问请求来源所属浏览器系列|
|ua\_browser\_type|访问请求来源的浏览器类型|
|ua\_browser\_version|访问请求来源的浏览器版本|
|ua\_device\_type|访问请求来源客户端的设备类型|
|ua\_os|访问请求来源客户端的操作系统信息|
|ua\_os\_family|访问请求来源客户端所属操作系统系列|
|upstream\_addr|WAF使用的回源地址列表，格式为IP:Port。多个地址之间以半角逗号（,）分隔。 |
|upstream\_ip|访问请求所对应的源站IP地址。例如WAF回源到ECS时，该参数返回ECS的IP地址。|
|upstream\_response\_time|源站响应WAF请求的时间，单位：秒。如果返回短划线（-），表示响应超时。 |
|upstream\_status|源站返回给WAF的响应状态。如果返回短划线（-），表示没有响应，例如该请求被WAF拦截。 |
|user\_id|阿里云账号ID|
|waf\_action|Web攻击防护策略行为。包括：-   block表示拦截。
-   by pass或其它值均表示放行。 |
|web\_attack\_type|Web攻击类型，例如xss、code\_exec、webshell、sqli、lfilei、rfilei、other等。|
|waf\_rule\_id|匹配的WAF的相关规则ID|
|ssl\_cipher|SSL加密套件|
|ssl\_protocol|SSL协议版本|

