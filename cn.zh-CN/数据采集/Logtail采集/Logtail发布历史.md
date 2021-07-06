# Logtail发布历史

本文档为您介绍日志服务Logtail的发布历史。

## 1.0.21

Logtail 1.0.21版本是首个全地域发布的Logtail 1.0版本，具备Logtail 0.16.64版本的所有功能，新增以下功能：

-   新功能
    -   新增配置项exactly\_once\_concurrency，实现了Logtail可以在本地磁盘记录细粒度的Checkpoint信息（文件级别）。更多信息，请参见[Logtail配置](/cn.zh-CN/开发指南/API参考/公共资源说明/Logtail配置.md)。
    -   新增配置项enable\_log\_time\_auto\_adjust，实现了日志时间可自适应服务器本地时间。更多信息，请参见[设置Logtail启动参数](/cn.zh-CN/数据采集/Logtail采集/安装/设置Logtail启动参数.md)。
    -   新增配置项enable\_log\_position\_meta，用于在日志中添加该日志所属原始文件的元数据信息。更多信息，请参见[Logtail配置](/cn.zh-CN/开发指南/API参考/公共资源说明/Logtail配置.md)。
    -   新增配置项specified\_year，用于使用当前时间中的年份或指定年份补全日志时间。更多信息，请参见[Logtail配置](/cn.zh-CN/开发指南/API参考/公共资源说明/Logtail配置.md)。

## 0.16.64

-   优化

    上调请求容器引擎时的超时时间，将3秒调整为30秒。新增环境变量DOCKER\_CLIENT\_REQUEST\_TIMEOUT，用于设置请求容器引擎的超时时间。

-   问题修复
    -   修复service\_skywalking插件的父Span ID发生错误的缺陷。
    -   修复根据环境变量创建的采集配置的逻辑在容器引擎异常时可能退出的缺陷。

## 0.16.62

**说明：** 如果您使用的是Logtail 0.16.58、0.16.60版本，建议您升级到Logtail 0.16.62版本。

-   问题修复

    修复在数据乱序场景下小概率发生的数据发送失败问题。


## 0.16.60

-   新功能

    支持采集containerd环境的容器数据。


## 0.16.56

-   优化

    调整服务日志中net\_err\_stat指标的覆盖范围，仅覆盖网络引起的发送错误。


## 0.16.54

-   新功能

    在服务日志中新增net\_err\_stat指标，记录最近1分钟、5分钟、15分钟内发生的发送错误的数量。


## 0.16.52

如果和容器（标准输出、容器文件）相关的采集配置较多，建议升级Logtail到0.16.52及以上版本，可有效地降低CPU开销。

-   优化

    优化容器数据采集场景的CPU开销。


## 0.16.50

-   新功能

    支持运行时按需安装service\_telegraf插件（仅限ECS用户）。


## 0.16.48

-   优化

    优化service\_telegraf插件，支持单机多个配置。


## 0.16.46

**说明：** 如果您在杭州、上海、北京地域，升级Logtail至0.16.46及以上版本，可避免Logtail在遇到网络抖动时切换Endpoint。

-   优化

    严格限制允许Logtail使用的网络类型。


## 0.16.44

-   新功能

    新增service\_telegraf插件，支持采集指标数据。更多信息，请参见[接入Telegraf数据](/cn.zh-CN/时序存储/数据接入/接入Telegraf数据/概述.md)。


## 0.16.42

-   新功能

    黑名单过滤支持多级匹配，例如/path/\*\*/log。

-   优化

    优化本地IP地址获取策略。在原先策略失效时，获取列表中的第一个IP地址。


## 0.16.40

-   新功能
    -   新增主机状态数据插件metric\_system\_v2。更多信息，请参见[采集主机监控数据](/cn.zh-CN/时序存储/数据接入/采集主机监控数据.md)。
    -   新增环境变量ALIYUN\_LOGTAIL\_MAX\_DOCKER\_CONFIG\_UPDATE\_TIMES对应的参数max\_docker\_config\_update\_times，适用于在K8s环境中频繁创建Job短时任务的场景。
-   优化

    优化容器采集场景中采集配置较多时的性能（CPU 开销）。

-   问题修复

    修复processor\_split\_log\_string插件偶尔产生空行的问题。


## 0.16.38

-   新功能
    -   完整正则模式支持自定义时间字段名。
    -   在processor\_json、processor\_regex、processor\_split\_char插件中，新增KeepSourceIfParseError参数，支持解析失败时保留原始数据。更多信息，请参见[使用Logtail插件处理数据](/cn.zh-CN/数据采集/Logtail采集/使用Logtail插件处理数据/概述.md)。

## 0.16.36

-   新功能

    新增加密插件processor\_encrypt。


## 0.16.34

-   新功能

    新增HTTP Probe，支持K8s健康检查。

-   问题修复
    -   修复某些环境中，由libcurl导致的core。
    -   修复在CentOS 8系统中安装Logtail，缺少libidn库的问题。

## 0.16.32

-   新功能

    在processor\_json插件中，新增IgnoreFirstConnector参数。更多信息，请参见[展开JSON字段](/cn.zh-CN/数据采集/Logtail采集/使用Logtail插件处理数据/展开JSON字段.md)。


## 0.16.30

**说明：** 此版本长时间运行时有潜在的打开文件失败风险，建议升级至最新版本。

-   新功能

    在采集Docker标准输出及文件时，新增K8s级别的过滤功能。

-   优化

    优化网络条件较差时同地域Logstore之间的并发竞争。

-   问题修复

    修复由于文件打开逻辑错误小概率发生的checkpoint丢失问题。


## 0.16.28

-   新功能

    新增参数，用于配置首次采集的Tail大小。

-   优化

    优化容器元信息获取逻辑，降低异常容器对整体的影响。

-   问题修复
    -   修复docker\_stdout在复杂环境下的内存泄漏问题。
    -   修复JSON模式下对毫秒时间戳不完整支持的问题。

## 0.16.26

-   新功能

    支持采集containerd的日志。

-   问题修复
    -   修复极低概率下发生的轮转文件丢失checkpoint的问题。
    -   修复本地采集配置文件/etc/ilogtail/user\_config.d在/usr/local/ilogtail/user\_log\_config.json文件不存在时未被加载的问题。

## 0.16.24

-   新功能
    -   支持通过环境变量配置working\_ip和working\_hostname。
    -   新增force\_quit\_read\_timeout参数，支持设置强制退出的超时时间（持续阻塞无法读取事件）。
    -   支持向插件传递path、topic等tag。
    -   新增aggregator\_shardhash插件，支持在插件内设置shardhash。
    -   新增处理插件processor\_gotime、processor\_rename、processor\_add\_fields、processor\_json、processor\_packjson。更多信息，请参见[使用Logtail插件处理数据](/cn.zh-CN/数据采集/Logtail采集/使用Logtail插件处理数据/概述.md)。
    -   更新LogtailInsight，新增进度查看功能（需要设置mark\_offset\_global\_flag或customized\_fields.mark\_offset）。
-   优化
    -   优化Journal长时间运行内存偏高的情况，尽可能及早释放。
    -   优化在本地无配置的情况下首次获取配置的时间间隔。
-   问题修复
    -   修复多个Logtail配置的情况下可能产生的重复采集问题。
    -   修复毫秒、微秒时间戳不支持JSON int64的问题。

## 0.16.23

-   新功能
    -   支持毫秒、微秒时间戳（%s）。
    -   支持加载多个本地Logtail配置（/etc/ilogtail/config.d/）。
    -   支持加载多个本地用户配置（/etc/ilogtail/user\_config.d/）。
    -   新增处理插件processor\_split\_key\_value、processor\_strptime。更多信息，请参见[键值对模式](/cn.zh-CN/数据采集/Logtail采集/使用Logtail插件处理数据/提取字段.md)、[提取日志时间](/cn.zh-CN/数据采集/Logtail采集/使用Logtail插件处理数据/提取日志时间.md)。
    -   新增oas\_connect\_timeout、oas\_request\_timeout参数，支持网络慢的场景。
    -   新增安装脚本支持通过金融云公网安装Logtail。更多信息，请参见[ECS金融云](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)。
-   优化

    取消混合配置（file+plugin）中对inputs的限制。


## 0.16.21

-   新功能
    -   支持自定义静态主题设置。
    -   支持黑名单过滤。
    -   在service\_canal插件中新增EnableEventMeta参数，支持采集MySQL Binlog对应的header信息。
-   优化

    优化插件系统停止机制。

-   问题修复

    修复GBK日志潜在的内存泄漏。


## 0.16.18

-   新功能
    -   支持采集Docker事件。更多信息，请参见[采集Docker事件](/cn.zh-CN/数据采集/Logtail采集/使用Logtail插件采集数据/采集Docker事件.md)。
    -   支持采集Systemd Journal日志。更多信息，请参见[采集Systemd Journal日志](/cn.zh-CN/数据采集/Logtail采集/使用Logtail插件采集数据/采集Systemd Journal日志.md)。
    -   新增处理插件processor\_pick\_key 、processor\_drop\_last\_key。
-   优化
    -   优化容器日志以及插件采集内存占用。
    -   优化采集容器标准输出（stdout）多行日志的性能。

## 0.16.16

-   新功能
    -   支持自动创建K8S审计日志相关的资源。
    -   支持通过环境变量配置启动参数，例如CPU、内存、发送并发等。
    -   支持通过环境变量配置自定义tag上传。
    -   sidecar模式支持自动创建配置。更多信息，请参见[通过Sidecar-CRD方式采集容器日志](/cn.zh-CN/数据采集/Logtail采集/采集容器日志/通过Sidecar-CRD方式采集容器日志.md)。
-   优化

    自动保存aliuid文件到本地文件。

-   问题修复
    -   修复采集容器文件出现极低概率的crash的问题。
    -   修复通过环境变量创建出的配置在K8S中存在的`IncludeLabel`不生效问题。

## 0.16.15

-   新功能
    -   采集MySQL Binlog时，支持GTID模式。在采集MySQL Binlog时自动开启该模式。
    -   历史数据导入文件名支持指定通配符。
    -   K8S支持自动创建索引配置。
-   优化
    -   当分行失败时，支持检查`discardUnMatch`并上报分行失败的日志。
    -   遇到unknown send error时自动重试，防止极低情况下数据丢失（例如发送的数据包中途被篡改）。

## 0.16.14

-   新功能
    -   导入历史数据支持通配符模式。
    -   增加启动配置项`default_tail_limit_kb`，用于配置首次采集文件跳转大小（默认1024KB）。
    -   增加采集配置项`batch_send_seconds`，用于配置数据包发送的时间。
    -   增加采集配置项`batch_send_bytes`，用于配置数据包的大小。
-   优化

    采集容器标准输出（stdout）时，支持自动合并被Docker Engine拆分的日志。


## 0.16.13

新功能

-   支持通过环境变量配置日志采集。
-   支持采集MySQL Binlog中的meta数据，即新增日志字段`_filename_`和`_offset_`。
-   安装脚本支持VPC下自动选择参数。
-   支持全球加速安装模式。更多信息，请参见[步骤二：配置Logtail采集加速](/cn.zh-CN/数据采集/采集加速/步骤二：配置Logtail采集加速.md)。

## 0.16.11

-   优化

    采集MySQL Binlog时，支持采集filename和offset信息。

-   问题修复

    修复使用多行模式采集容器标准输出（stdout）时有一定概率出现异常的问题。


## 0.16.10

-   优化

    升级容器标准输出（stdout）采集方式。


## 0.16.9

-   问题修复
    -   修复极低概率下出现的socket fd泄漏问题。
    -   增加容器文件采集配置更新频率限制。

## 0.16.8

-   新功能
    -   新增Logtail Lumberjack插件，用于采集Logstash、Beats数据源。更多信息，请参见[采集Beats和Logstash数据源](/cn.zh-CN/数据采集/Logtail采集/使用Logtail插件采集数据/采集Beats和Logstash数据源.md)。
    -   增加inotify黑名单功能。
-   问题修复
    -   修复旧安装包参数不统一的问题。
    -   修复在部分系统下安装Logtail时无法正确获取OS版本的问题。

## 0.16.6

-   新功能
    -   支持采集主机监控数据。
    -   支持采集Redis监控数据。
    -   支持采集MySQL Binlog中的DDL（data definition language）。
    -   支持采集容器标准输出（stdout）和容器文件时，通过docker ENV（environment）过滤。
-   问题修复
    -   兼容MySQL table无主键情况下的数据采集。
    -   兼容容器采集模式下因容器引擎订阅通道不稳定造成事件丢失的问题。

## 0.16.5

-   新功能

    采集容器标准输出（stdout）时，新增多行采集模式。更多信息，请参见[多行日志采集配置示例](/cn.zh-CN/数据采集/Logtail采集/采集容器日志/通过DaemonSet-控制台方式采集Kubernetes标准输出.mdsection_gf2_p8y_qsi)。


## 0.16.4

-   新功能
    -   支持Docker&Kubernetes部署方案。
    -   支持采集容器标准输出（stdout）和容器文件。更多信息，请参见[通过DaemonSet-控制台方式采集Kubernetes标准输出](/cn.zh-CN/数据采集/Logtail采集/采集容器日志/通过DaemonSet-控制台方式采集Kubernetes标准输出.md)、[通过DaemonSet-控制台方式采集Kubernetes文件](/cn.zh-CN/数据采集/Logtail采集/采集容器日志/通过DaemonSet-控制台方式采集Kubernetes文件.md)。

## 0.16.2

-   新功能

    新增processor\_geoip插件。更多信息，请参见[转换IP地址](/cn.zh-CN/数据采集/Logtail采集/使用Logtail插件处理数据/转换IP地址.md)。


## 0.16.0

-   新功能
    -   支持采集MySQL Binlog、MySQL查询结果、HTTP数据。更多信息，请参见[使用Logtail插件采集数据](/cn.zh-CN/数据采集/Logtail采集/使用Logtail插件采集数据/概述.md)。
    -   支持组合解析配置：正则模式、标定模式、分隔符模式、过滤器。

