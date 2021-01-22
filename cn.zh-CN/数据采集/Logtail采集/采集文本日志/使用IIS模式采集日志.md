# 使用IIS模式采集日志

IIS日志是服务器中的重要日志，日志服务支持通过IIS模式快速采集IIS日志并进行多维度分析。本文介绍如何通过日志服务控制台创建IIS模式的Logtail配置采集日志。

-   已创建Project和Logstore。更多信息，请参见[创建Project](/cn.zh-CN/数据采集/准备工作/管理Project.md)和[创建Logstore](/cn.zh-CN/数据采集/准备工作/管理Logstore.md)。
-   已开通服务器的80端口和443端口。
-   已在服务器上选择合适的日志格式（IIS、NCSA和W3C）生成日志。

    日志服务推荐使用W3C格式记录IIS日志。当您选择W3C格式时，需要先配置W3C日志记录字段，即选中**发送的字节数（sc-bytes）**和**接收的字节数（cs-bytes）**，其他字段保持默认配置。

    ![w3c日志](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3230559951/p60704.png)


## 操作步骤

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在接入数据区域，选择**IIS-文本日志**。

3.  选择目标Project和Logstore，单击**下一步**。

    您也可以单击**立即创建**，重新创建Project和Logstore。

4.  创建机器组。

    -   如果您已有可用的机器组，请单击**使用现有机器组**。
    -   如果您还没有可用的机器组，请执行以下操作（以ECS为例）。
        1.  在ECS页签中，选中目标ECS实例，单击**立即执行**。

            更多信息，请参见[安装Logtail（ECS实例）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（ECS实例）.md)。

            **说明：** 如果是自建集群、其他云厂商服务器，需要手动安装Logtail。更多信息，请参见[安装Logtail（Linux系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)或[安装Logtail（Windows系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md)。

        2.  安装完成后，单击**确认安装完毕**。
        3.  创建机器组，单击**下一步**。

            日志服务支持创建IP地址机器组和用户自定义标识机器组，详细参数说明请参见[创建IP地址机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建IP地址机器组.md)和[创建用户自定义标识机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建用户自定义标识机器组.md)。

5.  选中目标机器组，将该机器组从**源机器组**移动到**应用机器组**，单击**下一步**。

    **说明：** 如果创建机器组后立刻应用，可能因为连接未生效，导致心跳为**FAIL**，您可单击**自动重试**。如果还未解决，请参见[Logtail机器组无心跳]()进行排查。

6.  在Logtail配置页签中，创建Logtail配置。

    ![IIS模式](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5408276061/p186545.png)

    |配置项|详情|
    |---|--|
    |配置名称|Logtail配置的名称，在其所属Project内必须唯一。创建Logtail配置成功后，无法修改其名称。您也可以单击**导入其他配置**，导入其他已创建的Logtail配置。 |
    |日志路径|指定日志的目录和文件名。 日志文件名支持完整文件名和通配符两种模式，文件名规则请参见[Wildcard matching](http://man7.org/linux/man-pages/man7/glob.7.html)。日志文件查找模式为多层目录匹配，即指定目录（包含所有层级的目录）下所有符合条件的文件都会被查找到。例如：

    -   /apsara/nuwa/…/\*.log表示/apsara/nuwa目录（包含该目录的递归子目录）中后缀名为.log的文件。
    -   /var/logs/app\_\*/\*.log表示/var/logs目录下所有符合app\_\*模式的目录（包含该目录的递归子目录）中包含.log的文件。
**说明：**

    -   默认情况下，一个文件只能被一个Logtail配置采集。
    -   如果一个文件需要被采集多份，建议软链接文件所在的目录。例如/home/log/nginx/log/log.log文件需要被采集两份，则执行如下命令创建该文件所在目录的软链接。在一个Logtail配置中使用原路径，在另一个Logtail配置中使用软链接路径。

        ```
ln -s /home/log/nginx/log /home/log/nginx/link_log
        ```

    -   目录通配符只支持星号（\*）和问号（?）。 |
    |设置采集黑名单|打开**设置采集黑名单**开关后，可进行黑名单配置，即可在采集时忽略指定的目录或文件。支持完整匹配和通配符模式匹配目录和文件名。例如：     -   选择**按目录路径**，路径为/tmp/mydir，则在采集时过滤掉该目录下的所有文件。
    -   选择**按文件路径**，路径为/tmp/mydir/file，则在采集时过滤掉该文件。 |
    |是否为Docker文件|如果是Docker文件，可打开**是否为Docker文件**开关，直接配置内部路径与容器Tag。Logtail会自动监测容器创建和销毁，并根据Tag进行过滤采集指定容器的日志。关于容器文本日志采集请参见[通过DaemonSet-控制台方式采集Kubernetes文件](/cn.zh-CN/数据采集/Logtail采集/采集容器日志/通过DaemonSet-控制台方式采集Kubernetes文件.md)。|
    |模式|默认为**IIS配置模式**，可修改为其它模式，其他模式的配置请参见[概述](/cn.zh-CN/数据采集/Logtail采集/采集文本日志/概述.md)。|
    |日志格式|选择您的IIS服务器日志采用的日志格式，具体说明如下：     -   IIS：Microsoft IIS日志文件格式。
    -   NCSA：NCSA公用日志文件格式。
    -   W3C：W3C扩展日志文件格式。 |
    |IIS配置字段|配置IIS配置字段，具体说明如下：     -   日志格式为IIS或NCSA时，日志服务已默认设置了IIS配置字段。
    -   日志格式为W3C日志时，设置为IIS配置文件中logFile logExtFileFlags参数中的内容，例如：

        ```
logExtFileFlags="Date, Time, ClientIP, UserName, SiteName, ComputerName, ServerIP, Method, UriStem, UriQuery, HttpStatus, Win32Status, BytesSent, BytesRecv, TimeTaken, ServerPort, UserAgent, Cookie, Referer, ProtocolVersion, Host, HttpSubStatus"
        ```

        -   IIS5配置文件默认路径：C:\\WINNT\\system32\\inetsrv\\MetaBase.bin
        -   IIS6配置文件默认路径：C:\\WINDOWS\\system32\\inetsrv\\MetaBase.xml
        -   IIS7配置文件默认路径：C:\\Windows\\System32\\inetsrv\\config\\applicationHost.config |
    |IIS键名称|日志服务根据**IIS配置字段**中的配置，自动提取IIS键。|
    |丢弃解析失败日志|    -   打开**丢弃解析失败日志**开关，解析失败的日志不上传到日志服务。
    -   关闭**丢弃解析失败日志**开关，日志解析失败时上传原始日志到日志服务。 |
    |最大监控目录深度|设置日志目录被监控的最大深度。最大深度范围：0~1000，0代表只监控本层目录。|

    请根据您的需求选择高级配置。如果没有特殊需求，建议保持默认配置。

    |参数|描述|
    |:-|:-|
    |启用插件处理|打开**启用插件处理**开关后，您可以设置Logtail插件处理日志。更多信息，请参见[概述](/cn.zh-CN/数据采集/Logtail采集/使用Logtail插件处理数据/概述.md)。**说明：** 打开**启用插件处理**开关后，上传原始日志、时区属性、丢弃解析失败日志、过滤器配置、接受部分字段（分隔符模式）等功能不可用。 |
    |上传原始日志|打开**上传原始日志**开关后，原始日志将作为\_\_raw\_\_字段的值与解析过的日志一起上传到日志服务。|
    |Topic生成方式|设置Topic生成方式。更多信息，请参见[日志主题](/cn.zh-CN/数据采集/Logtail采集/采集文本日志/日志主题.md)。    -   **空-不生成Topic**：默认选项，表示设置Topic为空字符串，在查询日志时不需要输入Topic即可查询。
    -   **机器组Topic属性**：设置为机器组Topic属性，用于明确区分不同服务器产生的日志数据。
    -   **文件路径正则**：设置为文件路径正则，则需要设置**自定义正则**，用正则表达式从路径里提取一部分内容作为Topic。用于区分不同用户或实例产生的日志数据。 |
    |日志文件编码|设置日志文件编码格式，取值为utf8、gbk。|
    |时区属性|采集日志时，日志时间的时区属性。     -   机器时区：默认为机器所在时区。
    -   自定义时区：手动选择时区。 |
    |超时属性|如果一个日志文件在指定时间内没有任何更新，则认为该文件已超时。     -   永不超时：持续监控所有日志文件，永不超时。
    -   30分钟超时：如果日志文件在30分钟内没有更新，则认为已超时，并不再监控该文件。

选择**30分钟超时**时，还需设置**最大超时目录深度**，范围为1~3。 |
    |过滤器配置|只采集完全符合过滤器条件的日志。例如：     -   满足条件即采集，例如设置**Key**为**level**，**Regex**为**WARNING\|ERROR**，表示只采集level为WARNING或ERROR类型的日志。
    -   过滤不符合条件的日志。更多信息，请参见[Regular-Expressions.info](http://www.regular-expressions.info/lookaround.html)。
        -   设置**Key**为**level**，**Regex**为**^\(?!.\*\(INFO\|DEBUG\)\).\***，表示不采集level为INFO或DEBUG类型的日志。
        -   设置**Key**为**url**，**Regex**为**.\*^\(?!.\*\(healthcheck\)\).\***，表示不采集URL中带有healthcheck的日志。例如Key为url，Value为/inner/healthcheck/jiankong.html的日志将不会被采集。
更多信息，请参见[regex-exclude-word](https://stackoverflow.com/questions/2404010/match-everything-except-for-specified-strings)、[regex-exclude-pattern](https://stackoverflow.com/questions/2078915/a-regular-expression-to-exclude-a-word-string)。 |

    单击**下一步**即表示完成Logtail采集配置，日志服务开始采集日志。

    **说明：**

    -   Logtail配置生效时间最长需要3分钟，请耐心等待。
    -   如果遇到Logtail采集报错，请参见[诊断采集错误]()。
7.  预览数据及设置索引，单击**下一步**。

    日志服务默认开启全文索引。您也可以根据采集到的日志，手动或者自动设置字段索引。更多信息，请参见[配置索引](/cn.zh-CN/查询与分析/配置索引.md)。

    **说明：**

    -   如果您要查询分析日志，那么全文索引和字段索引属性必须至少启用一种。同时启用时，以字段索引为准。
    -   索引类型为long、double时，大小写敏感和分词符属性无效。

## 附录：日志样例及字段说明

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
    |s-sitename|客户端所访问的站点的Internet服务和实例的号码。|
    |s-computername|生成日志的服务器名称。|
    |s-ip|生成日志的服务器的IP地址。|
    |cs-method|请求​方法，例如：GET、POST。|
    |cs-uri-stem|​URI资源，表示请求访问的地址。|
    |cs-uri-query|URI查询，表示查询HTTP请求中问号（?）后的信息。|
    |s-port|服务器端口号。|
    |cs-username|通过验证的域或用户名。    -   如果是通过身份验证的用户，格式为`域\用户名`。
    -   如果是匿名用户，显示短划线（-）。 |
    |c-ip|访问服务器的客户端真实IP地址。|
    |cs-version|协议版本，例如：HTTP 1.0、HTTP 1.1。|
    |cs\(User-Agent\)|客户端使用的浏览器。|
    |Cookie|发送或接受的Cookie内容，如果没有Cookie，则显示短划线（-）。|
    |referer|表示用户访问的前一个站点。|
    |cs-host|主机信息。|
    |sc-status|​HTTP协议返回状态。|
    |sc-substatus|HTTP子协议的状态。|
    |sc-win32-status|​使用Windows术语表示的操作状态。|
    |sc-bytes|​服务器发送的字节数。|
    |cs-bytes|​服务器接收的字节数。|
    |time-taken|​请求所花费的时间，单位：毫秒。|


