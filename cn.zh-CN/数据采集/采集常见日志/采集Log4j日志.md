# 采集Log4j日志

本文介绍如何通过Loghub Log4j Appender或Logtail来采集Log4j日志。

Log4j是Apache的一个开放源代码项目，通过Log4j，可以控制日志的优先级、输出目的地和输出格式。日志的优先级从高到低为ERROR、WARN、 INFO、DEBUG，日志的输出目的地指定了将日志打印到控制台还是文件中，输出格式控制了输出的日志内容格式。 本文档以Log4j的默认配置为例进行说明，如下所示。

```
<Configuration status="WARN">
  <Appenders>
    <Console name="Console" target="SYSTEM_OUT">
      <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss:SSS zzz} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
  </Appenders>
  <Loggers>
    <Logger name="com.foo.Bar" level="trace">
      <AppenderRef ref="Console"/>
    </Logger>
    <Root level="error">
      <AppenderRef ref="Console"/>
    </Root>
  </Loggers>
</Configuration>
```

日志输出样例如下所示。

```
2013-12-25 19:57:06,954 [10.10.10.10] WARN impl.PermanentTairDaoImpl - Fail to Read Permanent Tair,key:e:470217319319741_1,result:com.example.tair.Result@172e3ebc[rc=code=-1, msg=connection error or timeout,value=,flag=0]
```

## 通过Loghub Log4j Appender采集Log4j日志

通过Loghub Log4j Appender采集Log4j日志的操作步骤请参见[Log4j Appender](/cn.zh-CN/数据采集/其他采集方式/SDK采集/Log4j Appender.md)。

## 通过Logtail采集Log4j日志

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在接入数据区域，选择**正则-文本日志**。

3.  在选择日志空间页签中，选择目标Project和Logstore，单击**下一步**。

    您也可以单击**立即创建**，重新创建Project和Logstore，详情请参见[步骤1：创建Project和Logstore](/cn.zh-CN/快速入门/快速入门.md)。

4.  在创建机器组页签中，创建机器组。

    -   如果您已有可用的机器组，请单击**使用现有机器组**。
    -   如果您还没有可用的机器组，请执行以下操作（以ECS为例）。
        1.  选择ECS实例安装Logtail，详情请参见[安装Logtail（ECS实例）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（ECS实例）.md)。

            如果已在ECS上安装Logtail，可直接单击**确认安装完毕**。

            **说明：** 如果是自建集群、其他云厂商服务器，需要手动安装Logtail，详情请参见[安装Logtail（Linux系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)或[安装Logtail（Windows系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md)。

        2.  安装完成后，单击**确认安装完毕**。
        3.  创建机器组，详情请参见[创建IP地址机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建IP地址机器组.md)或[创建用户自定义标识机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建用户自定义标识机器组.md)。
5.  在机器组配置页签中，应用机器组。

    选择一个机器组，将该机器组从**源机器组**移动到**应用机器组**。

6.  在Logtail配置页签中，创建Logtail配置。

    |参数|说明|
    |--|--|
    |配置名称|Logtail配置的名称，设置后不可修改。 您也可以单击**导入其他配置**，导入其他Project中已创建的Logtail配置。 |
    |日志路径|指定日志的目录和文件名。 日志文件名支持完整文件名和通配符两种模式，文件名规则请参见[Wildcard matching](http://man7.org/linux/man-pages/man7/glob.7.html)。日志文件查找模式为多层目录匹配，即指定目录（包含所有层级的目录）下所有符合条件的文件都会被查找到。例如：

    -   /apsara/nuwa/ … /\*.log表示/apsara/nuwa目录（包含该目录的递归子目录）中后缀名为.log的文件。
    -   /var/logs/app\_\* … /\*.log\*表示/var/logs目录下所有符合app\_\*模式的目录（包含该目录的递归子目录）中包含.log的文件。
**说明：**

    -   一个文件只能被一个Logtail配置采集。
    -   目录通配符只支持星号（\*）和问号（?）。 |
    |设置采集黑名单|开启该功能后，可设置**黑名单配置**。黑名单配置可在采集时忽略指定的目录或文件，目录和文件名支持完整匹配，也支持通配符模式匹配。例如：     -   选择**按目录路径**，路径为/tmp/mydir，则在采集时过滤掉该目录下的所有文件。
    -   选择**按文件路径**，路径为/tmp/mydir/file，则在采集时过滤掉该文件。 |
    |是否为Docker文件|如果是Docker文件，可以直接配置内部路径与容器Tag，Logtail会自动监测容器创建和销毁，并根据Tag进行过滤采集指定容器的日志。关于容器文本日志采集请参见[通过DaemonSet-控制台方式采集Kubernetes文件](/cn.zh-CN/数据采集/Logtail采集/采集容器日志/通过DaemonSet-控制台方式采集Kubernetes文件.md)。|
    |模式|配置为**完整正则模式**。|
    |单行模式|关闭**单行模式**。|
    |日志样例|输入如下Log4j日志样例。     ```
2013-12-25 19:57:06,954 [10.10.10.10] WARN impl.PermanentTairDaoImpl - Fail to Read Permanent Tair,key:e:470217319319741_1,result:com.example.tair.Result@172e3ebc[rc=code=-1, msg=connection error or timeout,value=,flag=0]
    ``` |
    |行首正则表示式|填写日志样例后，单击**自动生成**，生成行首正则表达式。本示例中，以日期时间表示一行的开头，行首正则表达式为\\d+-\\d+-\\d+\\s.\*。|
    |提取字段|开启**提取字段**，通过正则表达式将日志内容提取为Key-Value对。|
    |正则表达式|本示例中，配置为\(\\d+-\\d+-\\d+\\s\\d+:\\d+:\\d+,\\d+\)\\s\\\[\(\[^\\\]\]\*\)\\\]\\s\(\\S+\)\\s+\(\\S+\)\\s-\\s\(.\*\)，在实际场景中，根据以下方式配置正则表达式。     -   自动生成正则表达式

在**日志样例**框中，选中需要提取的字段，单击**生成正则**，自动生成正则表达式。

    -   手动输入正则表达式。

单击**手动输入正则表达式**，手动配置。配置完成后，单击**验证**即可验证您输入的正则表达式是否可以解析、提取日志样例，详情请参见[如何调试正则表达式]()。 |
    |日志抽取内容|通过正则表达式将日志内容提取为Value后，您需要为每个Value设置对应的Key。|
    |使用系统时间|本示例中关闭了**使用系统时间**，并配置时间格式为%Y-%m-%d %H:%M:%S，在实际场景中，根据以下方式配置。     -   开启**使用系统时间**，则日志时间为采集日志时，Logtail所在主机的系统时间。
    -   关闭**使用系统时间**，则需要在**日志抽取内容**中指定某一个Value为时间字段，并命名为time。选取时间字段后，您可以单击**时间转换格式**中的**自动生成**生成解析该时间字段的方式。关于日志时间格式的更多信息请参见[时间格式](/cn.zh-CN/数据采集/Logtail采集/采集文本日志/时间格式.md)。 |
    |丢弃解析失败日志|    -   开启**丢弃解析失败日志**，解析失败的日志不上传到日志服务。
    -   关闭**丢弃解析失败日志**，日志解析失败时上传原始日志。 |
    |最大监控目录深度|设置日志目录被监控的最大深度。最大深度范围：0~1000，0代表只监控本层目录。|

    请根据您的需求选择高级配置。如没有特殊需求，建议保持默认配置。

    |参数|详情|
    |:-|:-|
    |启用插件处理|请选择是否启用Logtail处理。开启该功能后，使用Logtail插件处理日志，具体配置请参见[处理数据](/cn.zh-CN/数据采集/Logtail采集/使用Logtail插件采集数据/处理数据.md)。|
    |上传原始日志|开启该功能后，原始日志内容作为\_\_raw\_\_字段与解析过的日志一起上传到日志服务。|
    |Topic生成方式|    -   **空-不生成Topic**：默认选项，表示设置Topic为空字符串，在查询日志时不需要输入Topic即可查询。
    -   **机器组Topic属性**：设置为机器组Topic属性，用于明确区分不同服务器产生的日志数据。
    -   **文件路径正则**：设置为文件路径正则，则需要配置**自定义正则**，用正则表达式从路径里提取一部分内容作为Topic。用于区分不同用户或实例产生的日志数据。 |
    |日志文件编码|    -   utf8：指定使用UTF-8编码。
    -   gbk：指定使用GBK编码。 |
    |时区属性|设置采集日志时，日志时间的时区属性。     -   机器时区：默认为机器所在时区。
    -   自定义时区：手动选择时区。 |
    |超时属性|如果一个日志文件在指定时间内没有任何更新，则认为该文件已超时。     -   永不超时：持续监控所有日志文件，永不超时。
    -   30分钟超时：如果日志文件在30分钟内没有更新，则认为已超时，并不再监控该文件。

选择**30分钟超时**时，还需配置**最大超时目录深度**，范围为1~3。 |
    |过滤器配置|只采集完全符合过滤器中的条件的日志。例如：     -   满足条件即采集：配置Key:level Regex:WARNING\|ERROR，表示只采集level为WARNING或ERROR类型的日志。
    -   [过滤不符合条件的数据](http://www.regular-expressions.info/lookaround.html)：
        -   配置为Key:level Regex:^\(?!.\*\(INFO\|DEBUG\)\).\*，表示不收集level为INFO或DEBUG类型的日志。
        -   配置为Key:url Regex:.\*^\(?!.\*\(healthcheck\)\).\*，表示不采集URL中带有healthcheck的日志，例如key为url，value为/inner/healthcheck/jiankong.html的日志将不会被采集。
更多示例请参见[regex-exclude-word](https://stackoverflow.com/questions/2404010/match-everything-except-for-specified-strings)、[regex-exclude-pattern](https://stackoverflow.com/questions/2078915/a-regular-expression-to-exclude-a-word-string)。 |

    Logtail配置完成后，日志服务开始采集Log4j日志。

7.  在**查询分析配置**页签中，设置索引。

    默认已设置索引，您也可以根据业务需求，重新设置索引，具体请参见[开启并配置索引](/cn.zh-CN/查询与分析/开启并配置索引.md)。

    **说明：**

    -   全文索引和字段索引属性必须至少启用一种。同时启用时，以字段索引属性为准。
    -   索引类型为long、double时，大小写敏感和分词符属性无效。

