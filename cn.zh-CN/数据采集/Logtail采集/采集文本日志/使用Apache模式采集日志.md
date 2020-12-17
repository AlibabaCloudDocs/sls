# 使用Apache模式采集日志

Apache日志是运维网站的重要信息，日志服务支持通过Apache模式快速采集Apache日志并进行多维度分析。本文介绍如何通过日志服务控制台创建Apache模式的Logtail配置采集日志。

-   已创建Project和Logstore。更多信息，请参见[步骤1：创建Project和Logstore](/cn.zh-CN/快速入门/快速入门.md)。
-   已在服务器上安装Logtail。更多信息，请参见[安装Logtail（ECS实例）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（ECS实例）.md)、[安装Logtail（Linux系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)或[安装Logtail（Windows系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md)。
-   已开通服务器的80端口和443端口。
-   已在Apache日志配置文件中指定日志的打印格式、日志文件路径和名称。更多信息，请参见[附录：日志格式和样例](#section_cwm_hiv_jnm)。

## 操作步骤

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在接入数据区域，选择**Apache-文本日志**。

3.  在选择日志空间页签中，选择目标Project和Logstore，单击**下一步**。

    您也可以单击**立即创建**，重新创建Project和Logstore，详情请参见[步骤1：创建Project和Logstore](/cn.zh-CN/快速入门/快速入门.md)。

4.  在创建机器组页签中，创建机器组。

    -   如果您已有可用的机器组，请单击**使用现有机器组**。
    -   如果您还没有可用的机器组，请执行以下操作（以ECS为例）。
        1.  选择ECS实例安装Logtail。更多信息，请参见[安装Logtail（ECS实例）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（ECS实例）.md)。

            **说明：** 如果是自建集群、其他云厂商服务器，需要手动安装Logtail。更多信息，请参见[安装Logtail（Linux系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md#)或[安装Logtail（Windows系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md)。

        2.  安装完成后，单击**确认安装完毕**。
        3.  创建机器组。更多信息，请参见[创建IP地址机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建IP地址机器组.md)或[创建用户自定义标识机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建用户自定义标识机器组.md)。
5.  在机器组配置页签中，应用机器组。

    选择一个机器组，将该机器组从**源机器组**移动到**应用机器组**。

6.  在Logtail配置页签中，创建Logtail配置。

    ![Apache模式](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3195276061/p186526.png)

    |配置项|详情|
    |---|--|
    |配置名称|Logtail配置的名称，创建Logtail配置成功后不可修改该名称。 您也可以单击**导入其他配置**，导入其他Project中已创建的Logtail配置。 |
    |日志路径|指定日志的目录和文件名。 日志文件名支持完整文件名和通配符两种模式，文件名规则请参见[Wildcard matching](http://man7.org/linux/man-pages/man7/glob.7.html)。日志文件查找模式为多层目录匹配，即指定目录（包含所有层级的目录）下所有符合条件的文件都会被查找到。例如：

    -   /apsara/nuwa/ … /\*.log表示/apsara/nuwa目录（包含该目录的递归子目录）中后缀名为.log的文件。
    -   /var/logs/app\_\* … /\*.log\*表示/var/logs目录下所有符合app\_\*模式的目录（包含该目录的递归子目录）中包含.log的文件。
**说明：**

    -   一个文件只能被一个Logtail配置采集。
    -   目录通配符只支持星号（\*）和问号（?）。 |
    |设置采集黑名单|打开**设置采集黑名单**开关后，可进行黑名单配置，即可在采集时忽略指定的目录或文件。支持完整匹配和通配符模式匹配目录和文件名。例如：     -   选择**按目录路径**，路径为/tmp/mydir，则在采集时过滤掉该目录下的所有文件。
    -   选择**按文件路径**，路径为/tmp/mydir/file，则在采集时过滤掉该文件。 |
    |是否为Docker文件|如果是Docker文件，可打开**是否为Docker文件**开关，直接配置内部路径与容器Tag。Logtail会自动监测容器创建和销毁，并根据Tag进行过滤采集指定容器的日志。关于容器文本日志采集请参见[通过DaemonSet-控制台方式采集Kubernetes文件](/cn.zh-CN/数据采集/Logtail采集/采集容器日志/通过DaemonSet-控制台方式采集Kubernetes文件.md)。|
    |模式|默认为**APACHE配置模式**，可修改为其它模式，其他模式的配置请参见[概述](/cn.zh-CN/数据采集/Logtail采集/采集文本日志/概述.md)。|
    |日志格式|根据您的Apache日志配置文件中定义的格式进行选择，包括common、combined和自定义。|
    |APACHE配置字段|Apache配置文件中的日志配置部分，通常以LogFormat开头。更多信息，请参见[附录：日志格式和样例](#section_cwm_hiv_jnm)。    -   当配置**日志格式**为**common**或**combined**时，此处会自动填充对应格式的配置字段，请确认是否和Apache配置文件中定义的格式一致。
    -   当配置**日志格式**为**自定义**时，请根据实际情况填写，例如LogFormat "%h %l %u %t \\"%r\\" %\>s %b \\"%\{Referer\}i\\" \\"%\{User-Agent\}i\\" %D %f %k %p %q %R %T %I %O" customized。 |
    |APACHE键名称|日志服务会根据**APACHE配置字段**中的内容自动读取Apache键。|
    |丢弃解析失败日志|是否丢弃解析失败的日志，具体说明如下：    -   打开**丢弃解析失败日志**开关，解析失败的日志不上传到日志服务。
    -   关闭**丢弃解析失败日志**开关，日志解析失败时上传原始日志到日志服务。 |
    |最大监控目录深度|设置日志目录被监控的最大深度。最大深度范围：0~1000，0代表只监控本层目录。|

    请根据您的需求选择高级配置。如没有特殊需求，建议保持默认配置。

    |参数|描述|
    |:-|:-|
    |启用插件处理|打开**启用插件处理**开关后，您可以配置Logtail插件处理日志。更多信息，请参见[处理数据](/cn.zh-CN/数据采集/Logtail采集/使用Logtail插件采集数据/处理数据.md)。**说明：** 打开**启用插件处理**开关后，上传原始日志、时区属性、丢弃解析失败日志、过滤器配置、接受部分字段（分隔符模式）等功能不可用。 |
    |上传原始日志|打开**上传原始日志**开关后，原始日志将作为\_\_raw\_\_字段的值与解析过的日志一起上传到日志服务。|
    |Topic生成方式|配置Topic生成方式，更多信息，请参见[日志主题](/cn.zh-CN/数据采集/Logtail采集/采集文本日志/日志主题.md)。    -   **空-不生成Topic**：默认选项，表示设置Topic为空字符串，在查询日志时不需要输入Topic即可查询。
    -   **机器组Topic属性**：设置为机器组Topic属性，用于明确区分不同服务器产生的日志数据。
    -   **文件路径正则**：设置为文件路径正则，则需要配置**自定义正则**，用正则表达式从路径里提取一部分内容作为Topic。用于区分不同用户或实例产生的日志数据。 |
    |日志文件编码|配置日志文件编码格式，包括utf8和gbk。|
    |时区属性|采集日志时，日志时间的时区属性。     -   机器时区：默认为机器所在时区。
    -   自定义时区：手动选择时区。 |
    |超时属性|如果一个日志文件在指定时间内没有任何更新，则认为该文件已超时。     -   永不超时：持续监控所有日志文件，永不超时。
    -   30分钟超时：如果日志文件在30分钟内没有更新，则认为已超时，并不再监控该文件。

选择**30分钟超时**时，还需配置**最大超时目录深度**，范围为1~3。 |
    |过滤器配置|只采集完全符合过滤器条件的日志。例如：     -   满足条件即采集，例如配置**Key**为**level**，**Regex**为**WARNING\|ERROR**，表示只采集level为WARNING或ERROR类型的日志。
    -   过滤不符合条件的日志。更多信息，请参见[Regular-Expressions.info](http://www.regular-expressions.info/lookaround.html)。
        -   配置**Key**为**level**，**Regex**为**^\(?!.\*\(INFO\|DEBUG\)\).\***，表示不采集level为INFO或DEBUG类型的日志。
        -   配置**Key**为**url**，**Regex**为**.\*^\(?!.\*\(healthcheck\)\).\***，表示不采集URL中带有healthcheck的日志。例如Key为url，Value为/inner/healthcheck/jiankong.html的日志将不会被采集。
更多信息，请参见[regex-exclude-word](https://stackoverflow.com/questions/2404010/match-everything-except-for-specified-strings)、[regex-exclude-pattern](https://stackoverflow.com/questions/2078915/a-regular-expression-to-exclude-a-word-string)。 |

    单击**下一步**即表示完成Logtail采集配置，日志服务开始采集日志。

    **说明：**

    -   Logtail配置生效时间最长需要3分钟，请耐心等待。
    -   如果遇到Logtail采集报错，请参见[诊断采集错误]()。
7.  在**查询分析配置**页签中，预览数据及设置索引。

    默认开启全文索引。您也可以根据采集到的日志，手动或者自动设置字段索引。如何设置字段索引，请参见[开启并配置索引](/cn.zh-CN/查询与分析/开启并配置索引.md)。

    **说明：**

    -   如果您要查询分析日志，那么全文索引和字段索引属性必须至少启用一种。同时启用时，以字段索引为准。
    -   索引类型为long、double时，大小写敏感和分词符属性无效。

## 附录：日志格式和样例

当您要采集Apache日志时，您需要在Apache日志配置文件中指定日志的打印格式、日志文件路径和名称。例如CustomLog "/var/log/apache2/access\_log" combined，表示日志打印时使用combined格式，日志文件路径为/var/log/apache2/access\_log。具体的日志格式和日志样例如下所示：

-   Apache日志格式

    -   combined格式

        ```
        LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
        ```

    -   common格式

        ```
        LogFormat "%h %l %u %t \"%r\" %>s %b" 
        ```

    -   自定义格式

        ```
        LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %D %f %k %p %q %R %T %I %O" customized
        ```

    相关字段说明如下所示，更多信息，请参见[mod\_log\_config](https://httpd.apache.org/docs/2.4/tr/mod/mod_log_config.html)。

    |字段格式|字段名称|说明|
    |:---|:---|:-|
    |%a|client\_addr|客户端IP地址。|
    |%A|local\_addr|本地IP地址。|
    |%b|response\_size\_bytes|响应字节大小，空值时显示为短划线（-）。|
    |%B|response\_bytes|响应字节大小，空值时为0。|
    |%D|request\_time\_msec|请求时间，单位为微秒。|
    |%f|filename|文件名。|
    |%h|remote\_addr|远程的主机名。|
    |%H|request\_protocol\_supple|请求协议。|
    |%I|bytes\_received|服务器接收的字节数，需要启用mod\_logio模块。|
    |%k|keep\_alive|在此连接上处理的请求数。|
    |%l|remote\_ident|远程主机提供的识别信息。|
    |%m|request\_method\_supple|请求方法。|
    |%O|bytes\_sent|服务器发送的字节数，需要启用mod\_logio模块。|
    |%p|remote\_port|服务器端口号。|
    |%P|child\_process|子进程ID。|
    |%q|request\_query|查询字符串，如果不存在则为空字符串。|
    |%r|request|请求内容，包括方法名、地址和HTTP协议。|
    |%R|response\_handler|服务端的处理程序类型。|
    |%s|status|响应的HTTP状态，初始状态。|
    |%\>s|status|响应的HTTP状态，最终状态。|
    |%t|time\_local|服务器时间。|
    |%T|request\_time\_sec|请求时间，单位为秒。|
    |%u|remote\_user|客户端用户名。|
    |%U|request\_uri\_supple|请求的URI路径，不带查询字符串。|
    |%v|server\_name|服务器名称。|
    |%V|server\_name\_canonical|根据UseCanonicalName指令设定的服务器名称。|
    |“%\{User-Agent\}i”|http\_user\_agent|客户端信息。|
    |“%\{Rererer\}i”|http\_referer|来源页。|

-   Apache日志样例

    ```
    192.168.1.2 - - [02/Feb/2020:17:44:13 +0800] "GET /favicon.ico HTTP/1.1" 404 209 "http://localhost/x1.html" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/48.0.2564.97 Safari/537.36" 
    ```


