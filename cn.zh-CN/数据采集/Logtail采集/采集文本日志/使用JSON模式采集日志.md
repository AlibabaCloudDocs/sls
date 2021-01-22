# 使用JSON模式采集日志

日志服务提供Logtail JSON模式快速采集JSON日志。采集到日志后，您可以进行多维度分析、加工、投递等操作。本文介绍如何通过日志服务控制台创建JSON模式的Logtail配置采集日志。

-   已创建Project和Logstore。更多信息，请参见[创建Project](/cn.zh-CN/数据采集/准备工作/管理Project.md)和[创建Logstore](/cn.zh-CN/数据采集/准备工作/管理Logstore.md)。
-   已开通服务器的80端口和443端口。

JSON日志建构于两种结构，包括Object类型（键值对的集合）和Array类型（值的有序列表）。

Logtail JSON模式支持解析Object类型的JSON日志，自动提取Object首层的键作为字段名称，Object首层的值作为字段值。但不支持解析Array类型的JSON日志。您可以使用完整正则模式或者极简模式采集Array类型的JSON日志。具体操作，请参见[使用极简模式采集日志](/cn.zh-CN/数据采集/Logtail采集/采集文本日志/使用极简模式采集日志.md)或[使用完整正则模式采集日志](/cn.zh-CN/数据采集/Logtail采集/采集文本日志/使用完整正则模式采集日志.md)。

JSON日志示例如下所示：

```
{"url": "POST /PutData?Category=YunOsAccountOpLog&AccessKeyId=U0Ujpek********&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=pD12XYLmGxKQ%2Bmkd6x7hAgQ7b1c%3D HTTP/1.1", "ip": "10.200.98.220", "user-agent": "aliyun-sdk-java", "request": {"status": "200", "latency": "18204"}, "time": "05/Jan/2020:13:30:28"}
{"url": "POST /PutData?Category=YunOsAccountOpLog&AccessKeyId=U0Ujpek********&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=pD12XYLmGxKQ%2Bmkd6x7hAgQ7b1c%3D HTTP/1.1", "ip": "10.200.98.210", "user-agent": "aliyun-sdk-java", "request": {"status": "200", "latency": "10204"}, "time": "05/Jan/2020:13:30:29"}
```

## 操作步骤

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在接入数据区域，选择**JSON-文本日志**。

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

6.  创建Logtail配置，单击**下一步**。

    |参数|描述|
    |--|--|
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
    |是否为Docker文件|如果是Docker文件，可打开**是否为Docker文件**开关，直接配置内部路径与容器Tag。Logtail会自动监测容器创建和销毁，并根据Tag进行过滤采集指定容器的日志。关于容器文本日志采集，请参见[通过DaemonSet-控制台方式采集Kubernetes文件](/cn.zh-CN/数据采集/Logtail采集/采集容器日志/通过DaemonSet-控制台方式采集Kubernetes文件.md)。|
    |模式|默认为**JSON模式**，可修改为其它模式。|
    |使用系统时间|配置日志时间，具体说明如下：    -   打开**使用系统时间**开关，则日志时间为采集日志时，Logtail所在主机的系统时间。
    -   关闭**使用系统时间**开关，则您需要配置**指定时间字段Key名称**为JSON日志中的时间字段，并根据时间字段的值配置**时间转换格式**。时间格式详情请参见[时间格式](/cn.zh-CN/数据采集/Logtail采集/采集文本日志/时间格式.md)。

例如JSON日志中的时间信息为"time": "05/May/2016:13:30:28"，则您可以配置**指定时间字段Key名称**为**time**，**时间转换格式**为**%d/%b/%Y:%H:%M:%S**。 |
    |丢弃解析失败日志|是否丢弃解析失败的日志，具体说明如下：    -   打开**丢弃解析失败日志**开关，解析失败的日志不上传到日志服务。
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

