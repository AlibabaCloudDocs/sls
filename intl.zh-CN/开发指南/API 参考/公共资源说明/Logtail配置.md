# Logtail配置

Logtail配置是Logtail采集日志的策略集合。通过在创建Logtail配置时设置数据源、采集模式等参数，实现定制化的采集策略。本文介绍API模式下Logtail配置相关的参数说明。

## 基础参数

Logtail配置的基础参数说明如下表所示：

|参数名称|数据类型|是否必填|示例值|描述|
|:---|:---|:---|---|:-|
|configName|string|是|config-sample|Logtail配置的名称，在其所属Project内必须唯一。创建Logtail配置成功后，无法修改其名称。 命名规则如下：

-   只能包括小写字母、数字、短划线（-）和下划线（\_）。
-   必须以小写字母或者数字开头和结尾。
-   长度必须在2~128字符之间。 |
|inputType|string|是|file|日志输入的方式。可选值如下：-   plugin：通过Logtail插件采集MySQL Binlog等日志。
-   file：通过固定模式（正则模式、分隔符模式等）采集文本文件中的日志。 |
|inputDetail|JSON object|是|无|日志输入的相关配置。更多信息，请参见[inputDetail参数](#section_ehz_hub_h06)。|
|outputType|string|是|LogService|日志输出的方式，只支持LogService，即只支持将数据上传到日志服务。|
|outputDetail|JSON object|是|无|日志输出的相关配置。更多信息，请参见[outputDetail参数](#section_ajt_0tx_gwe)。|
|logSample|string|否|无|日志样例。|

## inputDetail参数

inputDetail参数用于配置日志输入的相关信息。

-   inputDetail基础参数

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |filterKey|array|否|\["ip"\]|用于过滤日志的字段。只有该字段的值满足filterRegex参数中设置的正则表达式时，对应的日志才会被采集。|
    |filterRegex|array|否|\["^10.\*"\]|与filterKey对应的正则表达式。filterRegex中的元素个数和filterKey中的元素个数必须相同。|
    |shardHashKey|array|否|\["\_\_topic\_\_"\]|数据写入模式。默认按照负载均衡模式写入数据。更多信息，请参见[负载均衡模式](/intl.zh-CN/开发指南/API 参考/日志库相关接口/PutLogs.md)。配置该参数后，按照Shard模式写入。更多信息，请参见[Shard模式](/intl.zh-CN/开发指南/API 参考/日志库相关接口/PutLogs.md)。支持的值包括\_\_topic\_\_、\_\_hostname\_\_、\_\_source\_\_。 |
    |enableRawLog|boolean|否|false|是否上传原始日志。可选值如下：    -   true：上传原始日志。
    -   false：不上传原始日志。 |
    |sensitive\_keys|array|否|无|脱敏功能。更多信息，请参见[表 3](#table_u1p_53g_t4e)。|
    |mergeType|string|否|topic|聚合方式。可选值如下：    -   topic（默认）：根据Topic聚合。
    -   logstore：根据Logstore聚合。 |
    |delayAlarmBytes|int|否|209715200|采集进度落后的告警阈值。默认值为209715200，即200 MB。|
    |adjustTimezone|boolean|否|false|是否调整日志时区，仅在配置时间解析（例如配置了timeFormat参数）的情况下使用。|
    |logTimezone|string|否|GMT+08:00|时区偏移量，格式为GMT+HH:MM（东区）、GMT-HH:MM（西区）。例如日志时间为东八区，则该值为GMT+08:00。|
    |advanced|JSON object|否|无|扩展功能。更多信息，请参见[表 4](#table_xuw_zvz_tp7)。|

    sensitive\_keys参数说明如下表所示：

    -   参数配置

        |参数名称|数据类型|是否必填|示例值|描述|
        |:---|:---|:---|---|:-|
        |key|string|是|content|日志字段名称。|
        |type|string|是|const|脱敏方式。可选值如下：         -   const：将敏感内容替换成const字段取值内容。
        -   md5：将敏感内容替换为其对应的MD5值。 |
        |regex\_begin|string|是|'password':'|敏感内容前缀的正则表达式，用于查找敏感内容。使用RE2语法。更多信息，请参见[RE2语法](https://github.com/google/re2/blob/master/doc/syntax.txt)。|
        |regex\_content|string|是|\[^'\]\*|敏感内容的正则表达式，使用RE2语法。更多信息，请参见[RE2语法](https://github.com/google/re2/blob/master/doc/syntax.txt)。|
        |all|boolean|是|true|是否替换该字段中所有的敏感内容。可选值如下：        -   true（推荐）：替换。
        -   false：只替换字段中匹配正则表达式的第一部分内容。 |
        |const|string|否|"\*\*\*\*\*\*\*\*"|当type设置为const时，必须配置。|

    -   配置示例

        例如日志中content字段的值为`[{'account':'1812213231432969','password':'04a23f38'}, {'account':'1812213685634','password':'123a'}]`，现需要将`password`部分替换为\*\*\*\*\*\*\*，则sensitive\_keys配置为：

        ```
        "key" : "content"
        "type" : "const"
        "regex_begin" : "'password':'"
        "regex_content" : "[^']*"
        "all" : true
        "const" : "********"
                                                        
        ```

    -   日志样例

        ```
        [{'account':'1812213231432969','password':'********'}, {'account':'1812213685634','password':'********'}]
        ```

    advanced参数说明如下表所示：

    |参数名称|数据类型|是否必填|示例|描述|
    |----|----|----|--|--|
    |exactly\_once\_concurrency|int|否|1|是否启用ExactlyOnce功能。ExactlyOnce功能用于指定单个文件允许的发送并发数。取值范围：0~512。更多信息，请参见[附录：ExactlyOnce写入功能说明](#section_okt_nus_mpa)。可选值如下：    -   0：不启用ExactlyOnce功能。
    -   其他值：开启ExactlyOnce功能，并指定单个文件运行的发送并发数。
**说明：**

    -   此参数值过大会增加内存和磁盘开销。请根据本地的写入流量评估参数值。
    -   Logtail本地会进行随机化处理，即使此参数值小于日志服务端的Shard数量，也能保证写入均衡（Shard范围一致），不需要完全对齐。
    -   配置该参数后，只对新文件生效。
    -   仅支持Logtail 1.0.20及以上版本。 |
    |enable\_log\_position\_meta|boolean|否|true|是否在日志中添加该日志所属原始文件的元数据信息，即新增\_\_tag\_\_:\_\_inode\_\_字段和\_\_file\_offset\_\_字段。可选值如下：    -   true：添加。
    -   false：不添加。
**说明：** 仅支持Logtail 1.0.20及以上版本。 |
    |specified\_year|uint|否|0|当原始日志的时间缺少年份信息时，您可以设置该参数，使用当前时间中的年份或指定年份补全日志时间。可选值如下：    -   0：使用当前时间中的年份。
    -   具体年份（例如2020）：使用指定年份。
**说明：** 仅支持Logtail 1.0.19及以上版本。 |
    |force\_multiconfig|boolean|否|false|是否允许该Logtail配置采集其他Logtail配置已匹配的文件。默认为false，表示不允许。适用于文件多写场景，例如一个文件通过两个采集配置采集到不同的logstore。 |
    |raw\_log\_tag|string|否|\_\_raw\_\_|上传原始日志时，用于存放原始日志的字段，默认为\_\_raw\_\_。|
    |blacklist|object|否|无|采集黑名单配置。更多信息，请参见[表 5](#table_ew9_6o5_h3p)。|
    |tail\_size\_kb|int|否|1024|新文件采集时回退采集的大小。单位：KB，默认值：1024，即表示回退采集1 MB。|
    |batch\_send\_interval|int|否|3|聚合发送周期。单位：秒，默认值：3。|
    |max\_rotate\_queue\_size|int|否|20|单文件轮转队列的长度。默认值：20。|

    blacklist参数说明如下表所示：

    |参数说明|数据类型|是否必填|示例|描述|
    |----|----|----|--|--|
    |dir\_blacklist|array|否|\["/home/admin/dir1", "/home/admin/dir2\*"\]|目录（绝对路径）黑名单。支持使用通配符星号（\*）匹配多个目录。例如配置路径为/home/admin/dir1，则表示在采集时忽略/home/admin/dir1目录下的所有内容。 |
    |filename\_blacklist|array|否|\["app\*.log", "password"\]|文件名黑名单，指定的文件名在任何目录下都不会被采集。支持使用通配符星号（\*）匹配多个文件名。|
    |filepath\_blacklist|array|否|\["/home/admin/private\*.log"\]|文件路径（绝对路径）黑名单。支持使用通配符星号（\*）匹配多个文件。配置路径为/home/admin/private\*.log，则表示在采集时忽略/home/admin/目录下所有以private开头，以.log结尾的文件。 |

-   文本日志的Logtail配置
    -   基础配置

        |参数名称|数据类型|是否必填|示例|描述|
        |:---|:---|:---|--|:-|
        |logType|string|是|common\_reg\_log|日志的采集模式。具体说明如下：        -   json\_log：JSON模式。
        -   common\_reg\_log：完整正则模式。
        -   delimiter\_log：分隔符模式。 |
        |logPath|string|是|/var/log/http/|日志文件的路径。|
        |filePattern|string|是|access\*.log|日志文件名称。|
        |topicFormat|string|是|none|Topic生成方式。可选值如下：        -   none：不生成日志主题。
        -   default：将日志文件的路径作为日志主题。
        -   group\_topic：将应用该Logtail配置的机器组的Topic作为日志主题。
        -   文件路径正则表达式：将日志文件路径的某一部分作为日志主题。例如/var/log/\(.\*\).log。
更多信息，请参见[日志主题](/intl.zh-CN/数据采集/Logtail采集/采集文本日志/日志主题.md)。 |
        |timeFormat|string|否|%Y/%m/%d %H:%M:%S|日志时间格式。更多信息，请参见[时间格式](/intl.zh-CN/数据采集/Logtail采集/采集文本日志/时间格式.md)。|
        |preserve|boolean|否|true|如果一个日志文件在指定时间内没有任何更新，则认为该文件已超时。可选值如下：        -   true（默认）：永不超时。
        -   false：如果日志文件在30分钟内没有更新，则认为已超时，并不再监控该文件。 |
        |preserveDepth|integer|否|1|当设置preserve为false时，需指定最大超时目录深度，取值范围：1~3。|
        |fileEncoding|string|否|utf8|日志文件编码格式，取值为utf8、gbk。|
        |discardUnmatch|boolean|否|true|是否丢弃匹配失败的日志。可选值如下：        -   true：匹配失败的日志被上传到日志服务。
        -   false：不上传匹配失败的日志到日志服务。 |
        |maxDepth|int|否|100|设置日志目录被监控的最大深度。取值范围：0~1000，0代表只监控本层目录。|
        |delaySkipBytes|int|否|0|采集落后时是否丢弃落后数据的阈值。可选值如下：        -   0（默认）：不丢弃。
        -   其他值：当采集落后超过该值（例如1024）时，则直接丢弃落后的数据。 |
        |dockerFile|boolean|否|false|采集的目标文件是否为容器内文件，默认为false。|
        |dockerIncludeLabel|JSON object|否|无|如果要设置Label白名单，则LabelKey必填。如果LabelValue不为空，则只采集容器Label中包含LabelKey=LabelValue的容器；如果LabelValue为空，则采集所有Label中包含LabelKey的容器。 **说明：**

        -   多个键值对之间为或关系，即只要容器的Label满足任一键值对即可被采集。
        -   LabelValue默认为字符串匹配，即只有LabelValue和容器名称完全相同才会匹配。如果该值以^开头并且以$结尾，则为正则匹配，例如：LabelValue配置为^\(kube-system\|istio-system\)$，可同时匹配kube-system和istio-system。
        -   请勿设置相同的LabelKey，如果重名只生效一个。 |
        |dockerExcludeLabel|JSON object|否|无|如果要设置Label黑名单，则LabelKey必填。如果LabelValue不为空，则只排除容器Label中包含LabelKey=LabelValue的容器；如果LabelValue为空，则排除所有Label中包含LabelKey的容器。 **说明：**

        -   多个键值对之间为或关系，即只要容器的Label满足任一键值对即可被采集。
        -   LabelValue默认为字符串匹配，即只有LabelValue和容器名称完全相同才会匹配。如果该值以^开头并且以$结尾，则为正则匹配，例如：LabelValue配置为^\(kube-system\|istio-system\)$，可同时匹配kube-system和istio-system。
        -   请勿设置相同的LabelKey，如果重名只生效一个。 |
        |dockerIncludeEnv|JSON object|否|无|如果要设置环境变量白名单，则EnvKey必填。如果EnvValue不为空，则只采集容器环境变量中包含EnvKey=EnvValue的容器；如果EnvValue为空，则采集所有环境变量中包含EnvKey的容器。 **说明：**

        -   多个键值对之间为或关系，即只要容器的环境变量满足任一键值对即可被采集。
        -   EnvValue默认为字符串匹配，即只有EnvValue和容器名称完全相同才会匹配。如果该值以^开头并且以$结尾，则为正则匹配，例如：EnvValue配置为^\(kube-system\|istio-system\)$，可同时匹配kube-system和istio-system。 |
        |dockerExcludeEnv|JSON object|否|无|如果要设置环境变量黑名单，则EnvKey必填。如果EnvValue不为空，则只排除容器环境变量中包含EnvKey=EnvValue的容器；若EnvValue为空，则排除所有环境变量中包含EnvKey的容器。 **说明：**

        -   多个键值对之间为或关系，即只要容器的环境变量满足任一键值对即可被采集。
        -   EnvValue默认为字符串匹配，即只有EnvValue和容器名称完全相同才会匹配。如果该值以^开头并且以$结尾，则为正则匹配，例如：EnvValue配置为^\(kube-system\|istio-system\)$，可同时匹配kube-system和istio-system。 |

    -   完整正则模式和极简模式特有配置

        |参数名称|数据类型|是否必填|示例值|描述|
        |:---|:---|:---|---|:-|
        |key|array|是|\["content"\]|字段列表，用于为原始日志内容配置字段。|
        |logBeginRegex|string|否|.\*|行首正则表达式。|
        |regex|string|否|\(.\*\)|提取字段的正则表达式。|

        完整正则模式的Logtail配置示例如下：

        ```
        {
            "configName": "logConfigName", 
            "outputType": "LogService", 
            "inputType": "file", 
            "inputDetail": {
                "logPath": "/logPath", 
                "filePattern": "*", 
                "logType": "common_reg_log", 
                "topicFormat": "default", 
                "discardUnmatch": false, 
                "enableRawLog": true, 
                "fileEncoding": "utf8", 
                "maxDepth": 10, 
                "key": [
                    "content"
                ], 
                "logBeginRegex": ".*", 
                "regex": "(.*)"
            }, 
            "outputDetail": {
                "projectName": "test-project", 
                "logstoreName": "test-logstore"
            }
        }
        ```

    -   JSON模式特有配置

        |参数名称|数据类型|是否必填|示例值|描述|
        |:---|:---|:---|---|:-|
        |timeKey|string|否|time|指定时间字段的key名称。|

    -   分隔符模式特有配置

        |参数名称|数据类型|是否必填|示例值|描述|
        |:---|:---|:---|---|:-|
        |separator|string|否|,|根据您的日志格式选择正确的分隔符。更多信息，请参见[附录：分隔符简介及日志样例](/intl.zh-CN/数据采集/Logtail采集/采集文本日志/使用分隔符模式采集日志.md)。|
        |quote|string|是|\\|当日志字段内容中包含分隔符时，需要指定引用符进行包裹，被引用符包裹的内容会被日志服务解析为一个完整字段。请根据您的日志格式选择正确的引用符。更多信息，请参见[附录：分隔符简介及日志样例](/intl.zh-CN/数据采集/Logtail采集/采集文本日志/使用分隔符模式采集日志.md)。|
        |key|array|是|\[ "ip", "time"\]|字段列表，用于为原始日志内容配置字段。|
        |timeKey|string|是|time|指定key列表中的某个字段为时间字段。|
        |autoExtend|boolean|否|true|如果日志中分割出的字段数少于配置的Key数量，是否上传已解析的字段。例如日志为11\|22\|33\|44\|55，分隔符为竖线（\|），日志内容将被解析为11、22、33、44和55，为其分别设置Key为A、B、C、D和E。

        -   true：采集日志11\|22\|33\|55时，55会作为Key D的Value被上传到日志服务。
        -   **false**：集日志11\|22\|33\|55时，该条日志会因字段与Key不匹配而被丢弃。 |

        分隔符模式的Logtail配置示例如下：

        ```
        {
            "configName": "logConfigName", 
            "logSample": "testlog", 
            "inputType": "file", 
            "outputType": "LogService", 
            "inputDetail": {
                "logPath": "/logPath", 
                "filePattern": "*", 
                "logType": "delimiter_log", 
                "topicFormat": "default", 
                "discardUnmatch": true, 
                "enableRawLog": true, 
                "fileEncoding": "utf8", 
                "maxDepth": 999, 
                "separator": ",", 
                "quote": "\"", 
                "key": [
                    "ip", 
                    "time"
                ], 
                "autoExtend": true
            }, 
            "outputDetail": {
                "projectName": "test-project", 
                "logstoreName": "test-logstore"
            }
        }
        ```

-   插件配置

    插件模式特有的配置。

    |参数名称|数据类型|是否必填|示例值|描述|
    |:---|:---|:---|---|:-|
    |plugin|JSON object|是|无|使用Logtail插件采集日志时，需配置此参数。更多信息，请参见[使用Logtail插件采集数据](/intl.zh-CN/数据采集/Logtail采集/使用Logtail插件采集数据/概述.md)。|

    插件模式配置示例如下：

    ```
    {
        "configName": "logConfigName", 
        "outputType": "LogService", 
        "inputType": "plugin",
        "inputDetail": {
            "plugin": {
                "inputs": [
                    {
                        "detail": {
                            "ExcludeEnv": null, 
                            "ExcludeLabel": null, 
                            "IncludeEnv": null, 
                            "IncludeLabel": null, 
                            "Stderr": true, 
                            "Stdout": true
                        }, 
                        "type": "service_docker_stdout"
                    }
                ]
            }
        }, 
        "outputDetail": {
            "projectName": "test-project", 
            "logstoreName": "test-logstore"
        }
    }
    ```


## outputDetail参数

用于配置日志输出的Project和Logstore。

|参数名称|数据类型|是否必填|示例值|描述|
|:---|:---|:---|---|:-|
|projectName|string|是|my-project|Project名称，必须为请求的Project名称。|
|logstoreName|string|是|my-logstore|Logstore名称。|

## 附录：ExactlyOnce写入功能说明

开启ExactlyOnce写入功能后，Logtail会在本地磁盘记录细粒度的Checkpoint信息（文件级别）。当出现进程异常、机器重启等情况后，Logtail会在启动时借助Checkpoint信息来确定每个文件需要重新处理的范围以及通过日志服务端对于重复数据的拒绝机制（递增序号），可有效避免数据的重复发送。但该功能会占用一定的磁盘写入资源。相关限制如下：

-   Checkpoint信息需要利用本地磁盘进行存储。如果是磁盘原因导致Checkpoint无法记录（磁盘满）或者内容损坏（磁盘故障），可能导致恢复失败。
-   Checkpoint仅记录文件的元数据信息，不包含文件数据。所以如果文件本身被删除或者修改，可能导致无法恢复。
-   ExactlyOnce写入功能依赖日志服务端记录的当前写入序号，目前每个Shard仅支持10,000条记录，在超出限制后，会产生记录替换。因此为了保证可靠性，对于写入同一个Logstore的活跃文件数\*Logtail实例数的结果不可超过9500（建议预留一定量）。
    -   活跃文件数：正在被读取和发送的文件。同一个逻辑文件名的不同轮转文件会串行发送，它们仅被算作一个活跃文件。
    -   Logtail实例数：即Logtail进程数。默认情况下每台机器就一个实例，即一般情况下等同于机器数。

出于性能考虑，默认写入Checkpoint时不会调用sync落盘，所以如果机器重启导致buffer数据来不及写入磁盘时，可能导致Checkpoint丢失。您可以在Logtail启动参数配置文件（/usr/local/ilogtail/ilogtail\_config.json）中，增加`"enable_checkpoint_sync_write": true,`，开启sync写功能。具体操作，请参见[设置Logtail启动参数](/intl.zh-CN/数据采集/Logtail采集/安装/设置Logtail启动参数.md)。

