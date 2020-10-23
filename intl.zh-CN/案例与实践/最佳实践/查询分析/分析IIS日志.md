# 分析IIS日志

日志服务支持采集IIS日志，并进行多维度分析。本文通过PV、UV、访问地域分布、错误请求、请求方法等维度分析II日志，以评估网站访问情况。

已采集IIS日志，详情请参见[IIS模式](/intl.zh-CN/数据采集/Logtail采集/采集文本日志/IIS模式.md)。

在日志采集配置向导中，已根据日志字段自动生成索引，如果您要修改索引，详情请参见[开启并配置索引](/intl.zh-CN/查询与分析/开启并配置索引.md)。

IIS是一款主流的网站服务器，具备简单易用、安全性能高等优势。当您选用IIS搭建网站时，IIS日志是运维网站的重要信息。

日志服务推荐选用W3C日志格式，W3C配置格式如下所示：

```
logExtFileFlags="Date, Time, ClientIP, UserName, SiteName, ComputerName, ServerIP, Method, UriStem, UriQuery, HttpStatus, Win32Status, BytesSent, BytesRecv, TimeTaken, ServerPort, UserAgent, Cookie, Referer, ProtocolVersion, Host, HttpSubStatus"
```

IIS日志样例如下所示：

```
#Software: Microsoft Internet Information Services 7.5
#Version: 1.0
#Date: 2020-09-08 09:30:26
#Fields: date time s-sitename s-ip cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken
2009-11-26 06:14:21 W3SVC692644773 125.67.67.* GET /index.html - 80 - 10.10.10.10 Baiduspider+(+http://www.baidu.com)200 0 64 185173 296 0
```

-   字段前缀说明

    |前缀|说明|
    |:-|:-|
    |s-|服务器操作|
    |c-|客户端操作|
    |cs-|客户端到服务器的操作|
    |sc-|服务器到客户端的操作|

-   各个字段说明

    |字段|说明|
    |:-|:-|
    |date|客户端发送请求的日期.。|
    |time|客户端发送请求的时间。|
    |s-sitename|服务名，表示客户端所访问的站点的Internet服务和实例的号码。|
    |s-computername|服务器名，表示生成日志的服务器名称。|
    |s-ip|服务器IP地址，表示生成日志的服务器的IP地址。|
    |cs-method|请求​方法，例如：GET、POST。|
    |cs-uri-stem|​URI资源，表示请求访问的地址。|
    |cs-uri-query|URI查询，表示查询HTTP请求中问号（?）后的信息。|
    |s-port|服务器端口，表示连接客户端的服务器端口号。|
    |cs-username|通过验证的域或用户名。对于通过身份验证的用户，格式为`域\用户名`；对于匿名用户，显示短划线（-）。 |
    |c-ip|客户端IP地址，表示访问服务器的客户端真实IP地址。|
    |cs-version|协议版本，例如：HTTP 1.0、HTTP 1.1。|
    |cs\(User-Agent\)|用户代理，表示在客户端使用的浏览器。|
    |Cookie|Cookie，表示发送或接受的Cookie内容，如果没有Cookie，则显示短划线（-）。|
    |referer|引用站点，表示用户访问的前一个站点。|
    |cs-host|主机信息。|
    |sc-status|​协议返回状态，表示HTTP或FTP的操作状态。|
    |sc-substatus|HTTP子协议的状态。|
    |sc-win32-status|​win32状态，表示操作状态。|
    |sc-bytes|​服务器发送的字节数。|
    |cs-bytes|​服务器接收的字节数。|
    |time-taken|​操作所花费的时间，单位：毫秒。|


1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在Project列表区域，单击目标Project。

3.  在**日志存储** \> **日志库**页签中，单击目标Logstore。

4.  在页面右上角，单击**15分钟（相对）**，设置查询的时间范围。

    您可以选择相对时间、整点时间和自定义时间范围。

    **说明：** 查询结果相对于指定的时间范围来说，有1min以内的误差。

5.  在搜索框中输入查询分析语句，单击**查询/分析**。

    查询分析语句详情请参见[查询分析语句格式](/intl.zh-CN/查询与分析/查询简介.md)。

    -   通过IP地址来源分析访问地域，查询分析语句如下所示：

        ```
        | select ip_to_geo("c-ip") as country, count(1) as c group by ip_to_geo("c-ip") limit 100
        ```

        ![访问地域分析](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3530659951/p6672.png)

    -   统计最近的PV数和UV数，查询分析语句如下所示：

        ```
        *| select approx_distinct("c-ip") as uv ,count(1) as pv , date_format(date_trunc('hour', __time__), '%m-%d %H:%i') as time group by date_format(date_trunc('hour', __time__), '%m-%d %H:%i') order by time limit 1000
        ```

        ![pv/uv统计](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3530659951/p6673.png)

    -   统计HTTP请求状态码的占比，查询分析语句如下所示：

        ```
        *| select count(1) as pv ,"sc-status" group by "sc-status"
        ```

        ![请求状态占比](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3530659951/p6674.png)

    -   统计流量的流入和流出情况，查询分析语句如下所示：

        ```
        *| select sum("sc-bytes") as net_out, sum("cs-bytes") as net_in ,date_format(date_trunc('hour', time), '%m-%d %H:%i') as time group by date_format(date_trunc('hour', time), '%m-%d %H:%i') order by time limit 10000
        ```

        ![出入流量统计](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3530659951/p6675.png)

    -   统计各种请求方法的占比，查询分析语句如下所示：

        ```
        *| select count(1) as pv ,"cs-method" group by "cs-method"
        ```

        ![请求方法占比](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3530659951/p6676.png)

    -   统计各种浏览器的占比，查询分析语句如下所示：

        ```
        *| select count(1) as pv, case when "user-agent" like '%Chrome%' then 'Chrome' when "user-agent" like '%Firefox%' then 'Firefox' when "user-agent" like '%Safari%' then 'Safari' else 'unKnown' end as "user-agent" group by case when "user-agent" like '%Chrome%' then 'Chrome' when "user-agent" like '%Firefox%' then 'Firefox' when "user-agent" like '%Safari%' then 'Safari' else 'unKnown' end order by pv desc limit 10
        ```

        ![请求UA占比](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3530659951/p6677.png)

    -   统计访问数量前十的地址，查询分析语句如下所示：

        ```
        *| select count(1) as pv, split_part("cs-uri-stem",'?',1) as path group by split_part("cs-uri-stem",'?',1) order by pv desc limit 10
        ```

        ![访问前十地址](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3530659951/p6678.png)


