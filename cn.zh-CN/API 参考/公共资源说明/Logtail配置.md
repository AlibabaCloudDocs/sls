# Logtail配置 {#concept_f3n_s5q_12b .concept}

Logtail配置叫做config，每个Project默认可以创建100个配置（config）。Config名称在Project下具备唯一性。

您可以通过config指定日志收集的位置、方式和参数。

 **Logtail配置命名规范：** 

-   只能包括小写字母、数字、连字符（-）和下划线（\_）
-   必须以小写字母或者数字开头和结尾
-   长度必须在2~128字节以内

配置示例如下：

``` {#codeblock_o7o_b66_hf5}
{
    "configName": "logConfigName", 
    "outputType": "LogService", 
    "inputType": "file", 
    "inputDetail": {
        "logPath": "/logPath", 
        "filePattern": "access.log", 
        "logType": "json_log", 
        "topicFormat": "default", 
        "discardUnmatch": false, 
        "enableRawLog": true, 
        "fileEncoding": "gbk", 
        "maxDepth": 10
    }, 
    "outputDetail": {
        "projectName": "test-project", 
        "logstoreName": "test-logstore"
    }
}
```

此处以json模式为例，更多示例请参考[配置示例](#)。

## Logtail配置参数 {#section_zvx_2sc_mgb .section}

|参数|类型|是否必须|描述|
|:-|:-|:---|:-|
|configName|string|是|配置名称，同一Project下配置名必须唯一。|
|logSample|string|否|日志样例。|
|inputType|string|是|配置类型，可选`plugin`、`file`。|
| [inputDetail](#) |object|是|输入类型配置。|
|outputType|string|是|输出类型，目前只支持`LogService`。|
| [outputDetail](#) |object|是|输出类型配置。|
|createTime\(output-only\)|int32|否|创建时间。 服务端返回参数，不支持设置。|
|lastModifyTime\(output-only\)|int32|否|最后一次修改时间。 服务端返回参数，不支持设置。|

## inputDetail 基础配置 {#section_tnc_b4c_mgb .section}

下述列表为所有类型的基础配置：

|属性|类型|是否必须|描述|
|:-|:-|:---|:-|
|filterKey|array|否|用于过滤日志所用到的 key，只有 key 的值满足对应 filterRegex 列中设定的正则表达式，日志才是符合要求的。|
|filterRegex|array|否|与 filterKey 对应的正则表达式， filterRegex 的长度和 filterKey 的长度必须相同。|
|shardHashKey|array|否|默认按照[负载均衡模式](intl.zh-CN/API 参考/日志库相关接口/PutLogs.md)写入，开启后按照[ShardHashKey模式](intl.zh-CN/API 参考/日志库相关接口/PutLogs.md)写入。支持的值包括 `__topic__`，`__hostname__`, `__source__`。|
|enableRawLog|bool|否|是否上传原始日志。|
| [sensitive\_keys](#) |array|否|脱敏功能配置，类型为数组，`SensitiveKey`详细介绍参考下述表格。|
|mergeType|string|否|聚合方式，默认按照Topic方式聚合。取值为 `topic`、`logstore`。|
|delayAlarmBytes|int|否|采集进度落后的告警阈值，默认为`209715200`，即200 MB。|
|adjustTimezone|bool|否|是否调整日志时区，仅在配置时间解析情况下使用。|
|logTimezone|string|否|时区偏移量，例如若日志时间为东八区，则该值为`GMT+08:00`。|
|priority|int|否|日志发送优先级，默认为`0`，若需设置为高优先级，则设置为`1`。|

其中，**SensitiveKey配置**如下：

|属性|类型|是否必须|描述|
|:-|:-|:---|:-|
|key|string|是|日志Key名称。|
|type|string|是|脱敏方式，取值为`const`、`md5`。 -   若取值为`const`，则将敏感内容替换成`const`字段取值内容。
-   若取值为`md5`，则将敏感内容替换为其对应的MD5值。

 |
|regex\_begin|string|是|敏感内容的前缀。|
|regex\_content|string|是|敏感内容正则表达式。|
|all|bool|是|是否替换该字段中所有的敏感内容。建议设置为`true`。|
|const|string|否|当`type`设置为`const`时必须填写。|

例如日志中存在字段`content`，内容为 `[{'account':'1812213231432969','password':'04a23f38'}, {'account':'1812213685634','password':'123a'}]`，现在需要将其中的`password`部分脱敏，则`SensitiveKey`配置为：

``` {#codeblock_l5r_joq_u06}
"key" : "content"
"type" : "const"
"regex_begin" : "'password':'"
"regex_content" : "[^']*"
"all" : true
"const" : "********"
				
```

最终脱敏后的日志内容为：

``` {#codeblock_y2c_nfz_c1j}
[{'account':'1812213231432969','password':'********'}, {'account':'1812213685634','password':'********'}]
```

## inputDetail 文本类型配置 {#section_fhz_c4c_mgb .section}

该部分为所有文本文件类型需要的基础配置：

|属性|类型|是否必须|描述|
|:-|:-|:---|:-|
|logType|string|是|如果选择文件类型，参数为必填项，目前可选： Json（`json_log`）

 飞天格式（`apsara_log`）

 完整正则（`common_reg_log`）

 分隔符模式（`delimiter_log`）

 |
|logPath|string|是|日志所在的父目录，例如`/var/logs/`。|
|filePattern|string|是|日志文件的Pattern，例如 `access*.log`。|
|topicFormat|string|是|Topic 生成方式，支持以下四种类型： -    `none`，表示 topic 为空。
-    `default`，表示将日志文件路径作为 topic。
-    `group_topic`，表示将应用该配置的机器组 topic 属性作为 topic。
-   其他值表示将日志文件路径的某部分作为 topic，如`/var/log/(.*).log`。

 |
|timeFormat|string|否|日志时间格式，如`%Y/%m/%d %H:%M:%S`。|
|preserve|bool|否|true 代表监控目录永不超时，false 代表监控目录 30 分钟超时，默认值为 true。|
|preserveDepth|integer|否|当设置 preserve 为 false 时，指定监控不超时目录的深度，最大深度支持 3。|
|fileEncoding|string|否|支持两种类型：`utf8`、`gbk`。|
|discardUnmatch|bool|否|是否丢弃匹配失败的日志。|
|maxDepth|int|否|最大目录监控深度范围0-1000，0代表只监控本层目录。|
|delaySkipBytes|int|否|采集落后的丢弃阈值，默认为0，即不丢弃。当采集落后超过该值时，则直接丢弃落后的数据。|
|isDocherFile|bool|否|是否为容器内文件，默认为false。详细字段含义请参考[容器内文件采集](../../../../intl.zh-CN/用户指南/Logtail采集/容器日志采集/容器文本日志.md)。|
|dockerIncludeLabel|object|否|容器label白名单，采集包含白名单中Label的Docker容器日志，为空表示全部采集。|
|dockerExcludeLabel|object|否|容器label黑名单，不采集包含黑名单中Label的Docker容器日志，为空表示全部采集。|
|dockerIncludeEnv|object|否|容器环境变量白名单，采集包含白名单中的环境变量的日志，为空表示全部采集。|
|dockerExcludeEnv|object|否|容器环境变量黑名单，采集不包含黑名单中的环境变量的日志，为空表示全部采集。|

-    **完整正则/极简日志配置** 

    下述为完整正则/极简模式特有的匹配：

    |属性|类型|是否必须|描述|
    |:-|:-|:---|:-|
    |key|array|是|日志内容抽取结构的key。|
    |logBeginRegex|string|否|行首正则表达式。|
    |regex|string|否|提取字段的正则表达式。|

     `极简模式`是完整正则模式的一个特例，`极简模式`配置方式如下：

    ``` {#codeblock_4sd_vxt_5so}
    "key" : ["content"]
    "logBeginRegex" : ".*"
    "regex" : "(.*)"
    ```

-    **JSON日志配置** 

    下述为JSON默认特有配置：

    |属性|类型|是否必须|描述|
    |:-|:-|:---|:-|
    |timeKey|string|否|指定时间字段的key 名称。|

-    **分隔符日志配置** 

    下述为分隔符模式特有配置：

    |属性|类型|是否必须|描述|
    |:-|:-|:---|:-|
    |separator|string|否|分隔符。|
    |quote|string|是|引用符。|
    |key|array|是|日志抽取结果的key列表。|
    |timeKey|string|是|指定时间字段key名称，必须在key列表里面。|
    |autoExtend|bool|否|当日志中实际的key数量大于配置的key数量时，是否自动扩展。|

-    **飞天日志配置** 

    下述为飞天模式特有配置：

    |属性|类型|是否必须|描述|
    |:-|:-|:---|:-|
    |logBeginRegex|string|否|行首正则表达式。|


## outputDetail {#section_sjv_3pc_mgb .section}

用于配置日志输出的Project、Logstore。

|属性|类型|是否必须|描述|
|:-|:-|:---|:-|
|projectName|string|是|项目名称，必须为请求的project名，否则报错。|
|logstoreName|string|是|日志库名称。|

## 配置示例 {#section_udh_xrc_mgb .section}

-   JSON模式：

``` {#codeblock_zxo_b3d_h6r}
{
    "configName": "logConfigName", 
    "outputType": "LogService", 
    "inputType": "file", 
    "inputDetail": {
        "logPath": "/logPath", 
        "filePattern": "access.log", 
        "logType": "json_log", 
        "topicFormat": "default", 
        "discardUnmatch": false, 
        "enableRawLog": true, 
        "fileEncoding": "gbk", 
        "maxDepth": 10
    }, 
    "outputDetail": {
        "projectName": "test-project", 
        "logstoreName": "test-logstore"
    }
}
```

-   分隔符模式：

``` {#codeblock_6mo_492_d58}
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
            "test", 
            "test2"
        ], 
        "autoExtend": true
    }, 
    "outputDetail": {
        "projectName": "test-project", 
        "logstoreName": "test-logstore"
    }
}
```

-   完整正则模式：

``` {#codeblock_zh0_l4u_bq8}
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

-   Plugin插件模式（docker标准输出类型）：

``` {#codeblock_6zb_nh1_g6x}
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


