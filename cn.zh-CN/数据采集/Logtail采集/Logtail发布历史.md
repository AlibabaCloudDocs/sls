# Logtail发布历史

本文档为您介绍日志服务Logtail的发布历史。

## 0.16.62

**说明：** 如果您使用的是Logtail 0.16.58、0.16.60版本，建议您升级到Logtail 0.16.62版本。

-   问题修复

    修复在数据乱序场景下小概率发生的数据发送失败问题。


## 0.16.60

-   新功能

    支持采集containerd环境的容器数据。


## 0.16.56

-   优化

    调整net\_err\_stat指标的覆盖范围，仅覆盖网络引起的发送错误。


## 0.16.54

-   新功能

    在服务日志中新增net\_err\_stat指标，记录最近1分钟、5分钟、15分钟内发生的发送错误的数量。


## 0.16.52

如果和容器（标准输出、容器文件）相关的采集配置较多，建议升级Logtail到0.16.52及以上版本，可有效地降低CPU开销。

-   优化

    优化容器数据采集场景的CPU开销。


## 0.16.50

-   新功能

    支持运行时按需安装telegraf插件（仅限ECS内网用户）。


## 0.16.48

-   优化

    优化service\_telegraf插件，支持单机多个配置。


## 0.16.46

**说明：** 如果您在杭州、上海、北京地域，升级Logtail至0.16.46及以上版本，可避免Logtail在遇到网络抖动时切换Endpoint。

-   优化

    严格限制Logtail允许使用的网络类型。


## 0.16.44

-   新功能

    新增service\_telegraf插件，支持采集指标数据。


## 0.16.42

-   新功能

    黑名单过滤支持多级匹配，例如/path/\*\*/log。

-   优化

    优化本地IP地址获取策略。在原先策略失效时，获取列表中的第一个IP地址。


## 0.16.40

-   新功能
    -   新增主机状态数据插件metric\_system\_v2。
    -   新增环境变量ALIYUN\_LOGTAIL\_MAX\_DOCKER\_CONFIG\_UPDATE\_TIMES对应的参数max\_docker\_config\_update\_times，适用于在K8s环境中频繁创建Job短时任务的场景。
-   优化

    优化容器采集场景中采集配置较多时的性能（CPU 开销）。

-   问题修复

    修复processor\_split\_log\_string插件偶尔产生空行的问题。


## 0.16.38

-   新功能
    -   完整正则模式支持自定义时间字段名（time\_key）。
    -   在processor\_json、processor\_regex、processor\_split\_char插件中，新增KeepSourceIfParseError参数，支持解析失败时保留原始数据。

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

    在processor\_json插件中，新增IgnoreFirstConnector参数。


## 0.16.30

**说明：** 此版本长时间运行时有潜在的打开文件失败风险，建议升级至最新版本。

-   新功能

    在Docker标准输出及文件采集时，新增K8s级别的过滤功能。

-   优化

    优化网络条件较差时同地域Logstore之间的并发竞争。

-   问题修复

    修复由于文件打开逻辑错误小概率发生的checkpoint丢失问题。


## 0.16.28

-   新功能

    新增参数，用于配置首次采集的Tail大小。

-   优化

    优化docker center inspect容器逻辑，降低异常容器对整体的影响。

-   问题修复
    -   修复docker\_stdout在复杂环境下的内存泄漏问题。
    -   修复JSON模式下对毫秒时间戳不完整支持的问题。

## 0.16.26

-   新功能

    支持采集containerd的日志。

-   问题修复
    -   修复极低概率下发生的轮转文件丢失checkpoint的问题。
    -   修复本地采集配置/etc/ilogtail/user\_config.d在/usr/local/ilogtail/user\_log\_config.json文件不存在时未被加载的问题。

## 0.16.24

-   新功能
    -   支持通过环境变量配置working\_ip和working\_hostname。
    -   新增force\_quit\_read\_timeout参数，支持设置强制退出的超时时间（持续阻塞无法读取事件）。
    -   支持向插件传递path、topic等额外tags。
    -   新增aggregator\_shardhash插件，支持在插件内设置shardhash。
    -   新增一系列的处理插件processor\_gotime、processor\_rename、processor\_add\_fields、processor\_json、processor\_packjson。
    -   更新LogtailInsight，新增进度查看功能（需要设置mark\_offset\_global\_flag或单个配置的customized\_fields.mark\_offset）。
-   优化
    -   优化journal长时间运行内存偏高的情况，尽可能及早释放。
    -   优化在本地无配置的情况下首次获取配置的时间间隔。
-   问题修复
    -   修复多个配置的情况下可能产生的重复采集问题。
    -   修复毫秒、微秒时间戳不支持JSON int64的问题。

## 0.16.23

-   新功能
    -   支持毫秒、微秒时间戳（%s）。
    -   支持加载多个本地Logtail配置（/etc/ilogtail/config.d/）。
    -   支持加载多个本地用户配置（/etc/ilogtail/user\_config.d/）。
    -   新增处理插件 processor\_split\_key\_value、processor\_strptime。
    -   新增oas\_connect\_timeout、oas\_request\_timeout参数，支持网络慢的场景。
    -   新增安装脚本支持通过金融云公网安装Logtail。
-   优化

    取消混合配置（file+plugin）中对inputs的限制。


## 0.16.21

-   新功能
    -   支持自定义静态主题设置。
    -   支持黑名单过滤。
    -   在service\_canal插件中新增EnableEventMeta参数，支持采集binlog事件对应的header信息。
-   优化

    优化插件系统停止机制。

-   问题修复

    修复GBK日志潜在的内存泄漏。


## 0.16.18

-   新功能
    -   支持Docker Event采集。
    -   支持Journal日志采集。
    -   新增处理插件processor\_pick\_key 、processor\_drop\_last\_key。
-   优化
    -   优化容器日志以及插件采集内存占用。
    -   优化容器stdout多行日志采集性能。

## 0.16.16

-   新功能
    -   支持自动创建K8S audit（审计）日志各类资源。
    -   支持通过环境变量配置启动参数，例如CPU、内存、发送并发等。
    -   支持通过环境变量配置自定义tag上传。
    -   sidecar模式支持自动创建配置。
-   优化

    自动保存aliuid文件到本地文件。

-   问题修复
    -   修复docker file采集有极低概率crash的问题。
    -   修复通过环境变量创建出的配置在K8S中存在的`IncludeLabel`不生效问题。

## 0.16.15

-   新功能
    -   binlog支持gtid模式，该模式在MySQL支持时会自动开启。
    -   历史数据导入支持配置检测外的文件夹。
    -   K8S支持自动创建索引配置。
-   优化
    -   当分行失败时，支持检查`discardUnMatch`并上报分行失败的日志。
    -   遇到unknown send error时自动重试，防止极低情况下数据丢失（例如发送的数据包中途被篡改）。

## 0.16.14

-   优化

    docker stdout支持自动merge被docker engine拆分的日志。

-   新功能
    -   导入历史数据支持通配符模式
    -   增加启动配置项`default_tail_limit_kb`用于配置首次采集文件跳转大小（默认1024）。
    -   增加采集配置项`batch_send_seconds`用于配置数据发送merge的时间。
    -   增加采集配置项`batch_send_bytes`用于配置数据发送merge的大小。

## 0.16.13

新功能

-   支持通过环境变量配置日志采集。
-   binlog采集新增meta数据`_filename_`、`_offset_`。
-   安装脚本支持VPC下自动选择参数。
-   支持全球加速安装模式。

## 0.16.11

-   优化

    binlog采集支持加入采集的filename和offset信息。

-   问题修复

    修复容器stdout采集在multiline模式下有一定概率出现异常的问题。


## 0.16.10

-   优化

    升级容器stdout采集方式。


## 0.16.9

-   问题修复
    -   修复极低概率下出现的socket fd泄漏问题。
    -   增加容器文件采集配置更新频率限制。

## 0.16.8

-   新功能
    -   新增输入源：lumber协议（支持采集logstash、beats系列软件）。
    -   增加inotify黑名单功能。
-   问题修复
    -   订正老安装包参数不统一的问题。
    -   修复在部分系统下安装logtail时无法正确获取OS版本的问题。

## 0.16.6

-   新功能
    -   支持主机监控。
    -   支持Redis监控。
    -   支持采集Binlog中的DDL（data definition language）。
    -   支持容器stdout和文件通过docker ENV（environment）过滤
-   问题修复
    -   兼容MySQL table无主键情况下的数据采集。
    -   兼容容器采集模式下因docker engine watch不稳定造成事件丢失的问题。

## 0.16.5

-   新功能

    支持容器stdout multi line模式。


## 0.16.4

-   新功能
    -   支持Docker&Kubernetes部署方案。
    -   支持采集容器stdout和容器文件。

## 0.16.2

-   新功能

    支持geo ip处理方式。


## 0.16.0

-   新功能
    -   支持Mysql binlog、Mysql查询结果、http输入源。
    -   支持数据组合解析配置：正则、Anchor、分隔符、过滤器。

## 0.14.6

-   新功能
    -   增加启动配置项`no_inotify`，用于配置是否禁用inotify。
    -   增加启动配置项`log_parse_error`，用于配置是否记录并上报错误日志（默认true）。
    -   增加采集配置项`delay_skip_bytes`，当采集delay超过`delay_skip_bytes`时直接跳到文件尾。

## 0.14.5

-   新功能
    -   增加手动导入数据的接口。
    -   增加采集配置项`delay_alarm_bytes` 。
    -   新增启动配置`discard_old_data`。
    -   增加关闭解析告警的本地配置。
-   优化

    metric格式修改为json 。


## 0.14.3

-   问题修复

    日志open失败时不删除checkpoint，缓存10分钟，防止因日志轮转时写入顺序问题导致重复采集。


## 0.14.2

-   新功能

    增加忽略文件夹iNode变化的设置参数。

-   问题修复
    -   修复在进程退出时有一定概率没有刷出最后几条未发送日志的问题。
    -   当注册inotify失败时，进程不退出，继续使用Polling工作。
-   优化

    优化Polling limit输出。


## 0.14.1

-   新功能

    配置增加优先级功能，默认优先级为0。

-   问题修复

    修复纯粹轮询情况下文件删除后重新创建有概率丢失事件的问题。

-   优化
    -   数据发送速率从2M/s上调到20M/s。
    -   Logtail默认内存上限调整到256M。
    -   增加proxy endpoint自动识别，降低优先级。
    -   优化获取ip逻辑：hostname-\>eth0-\>bond0。
    -   限制低优先级Alarm日志打印tps，包括regex、split、outdated alarm。
    -   优化checkingtool检查脚本。
    -   如果网络异常超过2小时，强制进行重启。
    -   增加新的Alarm类型：PROCESS\_TOO\_SLOW\_ALARM，如果一次解析时间超过1秒，则上报该错误，提示用户优化数据解析逻辑。

## 0.13.9

-   新功能
    -   增加直连ip发送功能，降低vip负载。
    -   配置增加自定义delay报警功能。
    -   增加truncate时自动向前skip的配置功能。
    -   首次采集时优化向前查找1M的逻辑，防止直接跳到1M导致日志截断报错。
-   优化

    进一步调整Quota fail策略，count 1，retry interval 3，scale 2。


## 0.13.7

-   新功能
    -   增加Global server版本实现。
    -   新增脱敏功能。
    -   新增采集配置流控功能。
    -   新增时区强制转换功能。
-   优化

    增加block Event管理功能，防止遇到block Event强制sleep input线程，提高事件处理效率。

-   问题修复
    -   增强采集配置有效性检查，防止无效采集配置运行。
    -   修复多字符分隔符解析连续多个空字段结尾的日志时会漏掉最后一个字段的问题。
    -   修复RPM打包时因网络问题可能会丢失binary的问题。
    -   修复ilogtaild status检查误判问题。
    -   修复因内核、glibc bug导致配置获取线程僵死的问题。
    -   修复checkpoint文件损坏时logtail无法重启的问题。

## 0.13.4

-   优化

    gbk 按行转码，转码失败时上传原始行。


## 0.13.3

-   问题修复

    配置升级时对于max\_depth刚好匹配的Reader会被误删，导致这部分配置可能重复采集1M数据。


## 0.13.2

-   新功能

    新增status功能，支持查询logtail自身状态以及采集进度。

-   问题修复
    -   Find Best Match 路径相同通过create time判断优先级，逻辑见附件。
    -   `REGISTER_INOTIFY_FAIL_ALARM`切换成系统级别Alarm。
    -   轮询max\_depth判断错误，比配置的少1。
    -   文件删除-\>创建时若dev+inode相同，但signature与文件名不同则需重新创建reader。
    -   json模式下一条数据超过512K时可能crash的问题修复。
    -   解析阻塞多个轮转文件时，配置更新可能出现阻塞文件解析进度丢失的问题。
    -   解决logtail crash在tcmalloc 中时上报堆栈信息时会造成死锁的问题。
    -   GBK编码转换失败支持将转换失败前的原始日志上报。
    -   丢弃发送失败超过6小时的数据。
-   优化

    新增Alarm `SAME_CONFIG_ALARM`、`PROCESS_QUEUE_BUSY_ALARM`、`DROP_LOG_ALARM`。


## 0.12.9

-   新功能

    支持raw log上传。

-   问题修复

    修复fd limit设置失败导致日志无法发送的问题。


## 0.12.8

-   问题修复
    -   修复log file保持open可能会占用所有fd而导致网络不可用的问题。
    -   修复7u中config中包含preserve配置时可能存在crash的问题。

## 0.12.7

-   新功能
    -   新增轮询收集模式，支持无notify的日志收集场景（例如，5u、6u、docker部分内核版本无inotify事件或事件不完整的环境）。
    -   多租户隔离功能，多个logstore数据采集的资源分配互相隔离。
    -   新增目录监控过滤配置，支持过滤监控目录下指定子目录 （控制台暂未上线）。
-   问题修复
    -   真实路径与对应的软链接同时配置的情况下某一配置日志无法收集。
    -   注册/删除监听目录时可能包含前缀名相同的目录。
    -   老版本缓存文件有一定概率无法发送。
    -   aliuid目录被删除时继续上报删除前的aliuid问题。
    -   notify事件解析中极低概率出现segment fault问题。
    -   checkpoint定期dump未清空缓存。
-   优化
    -   日志发送调整为FIFO模式。
    -   支持时间乱序的日志文件合并发送。
    -   增加reader上限设置，超过上限时自动清理。
    -   增加config match cache，提高配置查找性能。
    -   增加inotify监控目录定期本地dump的策略。
    -   优化监控目录注册/取消注册性能。
    -   新增`LOG_TRUNCATE_ALARM`、`DIR_EXCEED_LIMIT_ALARM`、`STAT_LIMIT_ALARM`、`FILE_READER_EXCEED_ALARM`。

## 0.12.4

-   新功能
    -   增加reader定时清理机制、rotate上下文定时清理机制、冗余checkpoint清理。
    -   monitor在发现内存超出限制时强制进行垃圾清理。
    -   增加升级过程中日志记录信息。
    -   增加worker锁功能（本版本未开启）。
    -   增加多租户隔离功能（本版本未开启）。
    -   新增checkpoint定期保存机制，checkpoint路径可设置。
    -   调整reader清理策略：Reader超过10000时清理超过7天未更改的reader，若清理完毕后还是超过10000，则清理超过1天未更改的reader。
-   问题修复

    修复升级中可能出现多个worker同时运行的情况。

-   参数调整

    logreader\_count\_upperlimit：从100调整到10000。


## 0.12.3

-   新功能

    config/data server发送数据支持bind source interface。

-   问题修复

    修复多线程解析场景下，GetAlignedTime极低概率下，lastResetTime等static变量在多线程下互相影响问题。

-   参数调整
    -   READ\_LOG\_DELAY\_ALARM判定阈值：从100 MB调整为200 MB。
    -   max\_buffer\_num：从5调整为8。

## 0.12.2

-   新功能
    -   监控目录支持通配符。
    -   每次启动生成新的instance\_id，避免docker下可能发生的instance\_id冲突导致心跳Fail问题。
    -   首次心跳请求开始前random sleep 3秒，避免集中启动对config server请求压力。
-   参数调整

    ilogtaild修复docker场景下status、stop误操作问题。


## 0.12.1

-   新功能

    新增logtail诊断工具（试用，未进入安装包）。

-   问题修复

    tail\_existed查找目标目录时，应遍历所有相关配置下的监控目录。


## 0.12.0

-   新功能
    -   弹内地域支持自动探测可用Endpoint。
    -   支持多user\_defined\_id。
    -   新增SEND\_QUOTA\_EXCEED\_ALARM、READ\_LOG\_DELAY\_ALARM。
    -   分隔符日志单字符分割场景，解析性能优化。
    -   分隔符日志支持多字符分割。
    -   修复cpp 0.6 sdk gethostbyname\_r缓冲区溢出问题。
    -   配置流请求失败时打印错误码。
    -   JSON日志解析GBK编码。

## 0.11.8

-   新功能
    -   自适应CPU资源上限调整。
    -   日志解析流控。
    -   优化stop Logtail操作，Logtail停止读新数据尽快flush数据并完成退出。
-   问题修复

    修复监控目录删除时未移出监控项问题。


## 0.11.6

-   新功能
    -   支持上下文查询功能。
    -   支持ACS场景，本地映射监控目录。
-   问题修复

    升级binary失败后，重新InitMonitor，导致worker进程内出现多个Monitor线程。


## 0.11.4

-   优化

    弹内集群，心跳时间间隔改为3分钟。


## 0.11.2

-   新功能
    -   新增配置项tail\_limit、tail\_existed。
    -   Logtail数据包按照分钟对齐。

## 0.11.1

-   新功能

    启动配置支持buffer\_file\_path设置。

-   优化
    -   心跳请求间隔从60秒缩短到15秒。
    -   burst\_bytes\_upperlimit参数调整到100GB。

## 0.11.0

-   优化

    处理新错误码ShardWriteQuotaExceed。

-   问题修复
    -   修复0.10.9引入问题，更新配置时可能导致内存泄漏。
    -   修复delimiter日志解析。

## 0.10.9

-   优化
    -   修改RPM默认杭州Endpoint。
    -   random sleep优化：key的batchMap满时，不再sleep。
-   问题修复

    新版本读0.10.0前版本生成的buffer文件兼容性问题。


## 0.10.8

-   优化
    -   首次打开文件回搠最多1 MB数据，文件轮转时打开新文件从头开始读。
    -   处理文件IN\_CREATE事件，创建fileReader并设置offset为0，支持copy文件到监控目录的场景。
    -   每一次batch发送数据操作前增加random sleep（0-400ms），绕过前段load burst问题。

## 0.10.4

-   新功能
    -   优化tcmalloc hold内存不释放问题，定时调用ReleaseFreeMemory API。
    -   优化文件轮转时reader对象释放处理逻辑。
    -   JSON日志解析失败时，完整上传整条日志时，修复因原地解析JSON导致数据被破坏问题。
    -   优化checkpoint超时释放逻辑，注册目录时，扫描目录下文件，如果发现有checkpoint，则主动创建LogFileReader。
    -   获取hostname所映射的ip是，过滤`127.*.*.*`（ACS场景）。

## 0.10.2

-   新功能
    -   支持JSON格式日志。
    -   支持Delimiter格式日志。
    -   支持日志自定义Tag。
    -   启动后加载一遍符合配置要求的本地文件，解决部分情况下新增配置无法完整收集日志的问题。
    -   支持日志解析异常时上传数据。
    -   删除首行日志超时跳过逻辑，弱化5分钟日志超时丢弃的逻辑。
-   问题修复
    -   LogFileReader对象1天超时删除而导致的跨天不活跃文件数据无法收集。
    -   fix 0.10.0版本引入问题：当reader被删除时，reader指针仍在被process queue中数据所使用导致的coredump。

## 0.10.1

-   新功能

    弹内运行默认参数调整。

-   问题修复

    修复0.10.0版本引入的单行日志超过512KB引起的内存泄漏问题。


## 0.10.0

-   新功能
    -   支持相同Logstore下不同Topic数据merge，降低写服务端TPS。
    -   多线程数据解析。
    -   SDK支持lz4 string发送，去掉发送模块异常重试时的不必要资源开销。
    -   文件日志数据收集支持按照时间片公平调度。
-   问题修复

    主线程轻量化，将文件IO、解析操作移出，解决主线程忙时导致配置升级无法响应问题。


## 0.9.11

-   新功能

    阿里云官网安装包新增参数支持syslog接入源。

-   问题修复

    处理syslog协议（TCP流式）数据写入时，极端情况下可能触发读写锁使用异常。


## 0.9.10

-   新功能

    支持syslog协议（TCP流式）数据写入。


## 0.9.9

-   新功能
    -   根据系统的时区信息自动适配GMT时间。
    -   Logtail epoll轮询间隔从10秒缩短到1秒，缩短logtail异步加载配置的间隔。

## 0.9.8

-   新功能

    支持ssl SNI，提升curl\_logtail版本至7.46，配套使用ssl\_logtail（1.0.0t），crypto\_logtail静态库。

-   问题修复

    修复0.9.4版本引入问题：网络异常时，logtail busy waiting导致的cpu 100%。


## 0.9.7

-   新功能

    每次启动Logtail重新生成InstanceId上报服务端，解决ECS上不同实例公用镜像导致的心跳异常。


## 0.9.6

-   新功能
    -   优化发送模块的异常重试逻辑，降低异常重试TPS对前端机的压力。
    -   LogGroup新增machine\_uuid字段，与source字段平级。
-   问题修复

    弹外ECS VM根据GetLogtailSecurity返回的aliuid在心跳中上报服务端。


## 0.9.5

-   新功能
    -   上报Linux random time UUID作为InstanceId。
    -   兼容：弹外按照pub/corp的region tag设置阈值配置。
-   问题修复

    连接网络失败时，buffer线程进入busy waiting，导致cpu 100%。


## 0.9.4

-   新功能
    -   单线程异步发送，改lz4压缩。
    -   支持LogCollection新的机器组管理方式，通过Aliuid向服务端请求AK。
    -   支持多Endpoint数据发送。
    -   支持多binary download url。
    -   处理SIGTERM信号写checkpoint。
    -   客户端兼容逻辑，服务端下发的aliuid订正到本机。

## 0.9.2

-   新功能
    -   心跳主动上报UUID。
    -   分离用户数据SLSClient和LogtailProfiling的SLSClient。

## 0.9.1

-   新功能
    -   metric支持发送最小化神农。
    -   首行日志匹配失败时上报错误未包含错误日志样例。
-   问题修复
    -   Logtail取系统时间会忽略掉夏令时标志。
    -   主机UUID信息格式非标准时Logtail Crash。

## 0.9.0

-   新功能
    -   多线程发送。
    -   支持GBK编码日志。
    -   metric数据的profiling信息上报。
    -   提供配置最大监控深度功能。
    -   支持GroupTopic。
-   问题修复
    -   极端情况下判断日志边界引发的日志解析出错问题。
    -   注册监控项时检查是否为目录。
    -   Logtail非法map访问导致crash。
    -   Logtail rpm包判断状态，如果是stop状态，退出码不应该是0。

## 0.8.12

-   新功能
    -   处理飞天场景下，domain socket文件被删除导致metric数据无法收集的问题。
    -   增强自升级的容错性，最大化自升级成功概率。
-   问题修复
    -   Logtail获取首行日志时间出错情况下的日志丢失问题。
    -   Logtail自身日志丢失问题。
    -   配置升级可能会忽略最后一层路径的目录timeout属性配置升级可能将最后一层路径的目录timeout属性忽略掉。
    -   epoll\_wait失败后Logtail工作进程的主线程卡住导致数据无法收集。

## 0.8.11

-   新功能
    -   正则匹配catch boost crash。
    -   Logtail向config server汇报OS信息。
    -   过滤gz等压缩格式文件的修改事件。
    -   严格检查每条日志时间。
    -   支持日志取系统时间。
    -   catch处理checkpoint文件时可能存在的异常。
-   问题修复
    -   Logtail监控超过3000个目录之后无法监控到这些目录下新建的日志文件。
    -   处理回滚时计算topic出错。
    -   对内rpm脚本yum update时停止进程。

## 0.8.10

-   新功能
    -   日志merge策略：4000条或2 MB内。
    -   支持监控软连接目录加入。
    -   补全及提示configserver/dataserver配置协议。
    -   用户监控目录不存在时提示错误。

