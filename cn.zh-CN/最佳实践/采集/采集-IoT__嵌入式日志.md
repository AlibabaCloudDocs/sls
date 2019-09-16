# 采集-IoT/嵌入式日志 {#concept_u3y_q2j_wfb .concept}

IoT（Internet of Things）正在高速增长，越来越多设备开始逐步走进日常生活，例如智能路由器、各种电视棒、天猫精灵、扫地机器人等，让我们体验到智能领域的便利。传统软件领域的嵌入式开发模式在IoT设备领域的应用遇到了很多挑战，IoT设备数目多、分布广，难以调试且硬件受限，传统的设备日志解决方案无法完美满足需求。

日志服务团队根据多年Logtail的开发经验，结合IoT设备的特点，为IoT设备量身定制一套日志数据采集方案：C Producer。

![日志数据采集方案](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13213/156863309432650_zh-CN.png)

## 嵌入式开发需求 {#section_lls_gmj_wfb .section}

作为IoT/嵌入式工程师，除了需要深厚的开发功底外，面对海量的设备，如何有能力管理、监控、诊断这些“黑盒”设备至关重要。嵌入式开发需求主要有以下几点：

-   数据采集：如何实时采集分散在全球各地的百万/千万级设备上的数据？
-   调试：如何使用一套方案既满足线上数据采集又满足开发时的实时调试？
-   线上诊断：某个线上设备出现错误，如何快速定位设备，查看引起该设备出错的上下文是什么？
-   监控：当前有多少个设备在线？工作状态分布如何？地理位置分布如何？出错设备如何实时告警？
-   数据实时分析：设备产生数据如何与实时计算、大数据仓库对接，构建用户画像？

![嵌入式开发需求](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13213/156863309432652_zh-CN.png)

## IoT领域面临的主要挑战 {#section_nwz_lmj_wfb .section}

思考以上问题的解决方案，我们发现在传统软件领域那一套手段面临IoT领域基本全部失效，主要挑战来自于IoT设备这些特点：

-   设备数目多：在传统运维领域管理1W台服务器属于一家大公司了，但10W在线对于IoT设备而言只是一个小门槛。
-   分布广：硬件一旦部署后，往往会部署在全国、甚至全球各地。
-   黑盒：难以登录并调试，大部分情况属于不可知状态。
-   资源受限：出于成本考虑，IoT设备硬件较为受限（例如总共只有32MB内存），传统PC领域手段往往失效。

## C Prodecer {#section_s5s_pmj_wfb .section}

日志服务量身定制的日志数据采集解决方案。

[日志服务](https://www.alibabacloud.com/zh/product/log-service?spm)客户端Logtail在X86服务器上有百万级部署，可以参见文章：Logtail技术分享： ，。除此之外，日志服务提供多样化的采集方案：

-   移动端SDK：Android/IOS平台数据采集，一天已有千万级DAU。
-   Web Tracking（JS）：类似百度统计，Analytics 轻量级采集方式，无需签名。

在IoT领域，我们从多年Logtail的开发经验中，汲取其中精华的部分，并结合IoT设备针对CPU、内存、磁盘、网络、应用方式等特点，开发出一套专为IoT定制的日志数据采集方案：C Producer。

![采集解决方案](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13213/156863309432653_zh-CN.png)

## C Producer特点 {#section_lxj_3nj_wfb .section}

C Producer Library 继承Logtail稳定、高性能、低资源消耗等特点，可以定位是一个“轻量级Logtail”，虽没有Logtail实时配置管理机制，但具备除此之外70%功能，包括：

-   提供多租户概念：可以对多种日志（例如Metric，DebugLog，ErrorLog）进行优先级分级处理，同时配置多个客户端，每个客户端可独立配置采集优先级、目的project/logstore等。
-   支持上下文查询：同一个客户端产生的日志在同一上下文中，支持查看某条日志前后相关日志。
-   并发发送，断点续传：支持缓存上线可设置，超过上限后日志写入失败。

此外，C Producer还具备以下IoT设备专享功能，例如：

-   本地调试：支持将日志内容输出到本地，并支持轮转、日志数、轮转大小设置。
-   细粒度资源控制：支持针对不同类型数据/日志设置不同的缓存上线、聚合方式。
-   日志压缩缓存：支持将未发送成功的数据压缩缓存，减少设备内存占用。

![C Producer特点](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13213/156863309532654_zh-CN.png)

## 功能优势 {#section_bmt_4nj_wfb .section}

C-Producer作为IoT设备的量身定制方案，在以下方面具备明显优势：

![功能优势](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13213/156863309532656_zh-CN.png)

-   客户端高并发写入：可配置的发送线程池，支持每秒数十万条日志写入，详情参见性能测试。
-   低资源消耗：每秒20W日志写入只消耗70% CPU；同时在低性能硬件（例如树莓派）上，每秒产生100条日志对资源基本无影响。
-   客户端日志不落盘：既数据产生后直接通过网络发往服务端。
-   客户端计算与 I/O 逻辑分离：日志异步输出，不阻塞工作线程。
-   支持多优先级：不同客户端可配置不同的优先级，保证高优先级日志最先发送。
-   本地调试：支持设置本地调试，便于您在网络不通的情况下本地测试应用程序。

在以上场景中，C Producer Library 会简化您程序开发的步骤，您无需关心日志采集细节实现、也不用担心日志采集会影响您的业务正常运行，大大降低数据采集门槛。

为了有一个感性认识，我们对C-Producer 方案与其他嵌入式采集方案做了一个对比，如下：

|类别|C Producer|其他方案|
|:-|:---------|:---|
|编程|平台|移动端+嵌入式|移动端为主|
|上下文|支持|不支持|
|多日志|支持|不支持（一种日志）|
|自定义格式|支持|不支持（提供若干个有限字段）|
|优先级|支持|不支持|
|环境参数|可配置|可配置|
|稳定性|并发度|高|一般|
|压缩算法|LZ4（效率与性能平衡）+GZIP|优化|
|低资源消耗|优化|一般|
|传输|断电续传|支持|默认不支持，需要二次开发|
|接入点|8（国内）+8（全球）|杭州|
|调试|本地日志|支持|手动支持|
|参数配置|支持|不支持|
|实时性|服务端可见|1秒（99.9%），3秒（Max）|1-2小时|
|自定义处理|15+对接方式|定制化实时+离线方案|

## C-Producer+日志服务解决方案 {#section_k22_m5j_wfb .section}

C-Producer结合阿里云[日志服务](https://www.alibabacloud.com/zh/product/log-service?spm)产品配合使用，即可完成IoT设备日志全套解决方案。

-   规模大
    -   支持亿级别客户端实时写入。
    -   支持 PB/Day数据量。
-   速度快
    -   采集快：0延迟：写入0延迟，写入即可消费。
    -   查询快：一秒内，复杂查询（5个条件）可处理10亿级数据。
    -   分析快：一秒内，复杂分析（5个维度聚合+GroupBy）可聚合亿级别数据。
-   对接广
    -   与阿里云各类产品无缝打通。
    -   各种开源格式存储、计算、可视化系统完美兼容。

![C-Producer解决方案](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13213/156863309532657_zh-CN.png)

## 下载与使用 {#section_lvy_svj_wfb .section}

下载地址：[Github](https://github.com/aliyun/aliyun-log-c-sdk?spm=a2c4g.11186623.2.29.2dfc505eazEyHL)

一个应用可创建多个producer，每个producer可包含多个client，每个client可单独配置目的地址、日志level、是否本地调试、缓存大小、自定义标识、topic等信息。

详细安装方式及操作步骤，请参考Github页面的[README](https://github.com/aliyun/aliyun-log-c-sdk?spm=a2c4g.11186623.2.30.2dfc505eazEyHL)。

![下载与使用](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13213/156863309532658_zh-CN.png)

## 性能测试 {#section_e1k_xvj_wfb .section}

环境配置

-   高性能场景：传统X86服务器。
-   低性能场景：树莓派（低功耗环境）。

配置分别如下：

![环境配置](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13213/156863309532659_zh-CN.png)

C-Producer配置

-   ARM（树莓派）

    -   缓存：10MB
    -   聚合时间：3秒 （聚合时间、聚合数据包大小、聚合日志数任一满足即打包发送）
    -   聚合数据包大小：1MB
    -   聚合日志数：1000
    -   发送线程：1
    -   自定义tag：5
-   X86

    -   缓存：10MB
    -   聚合时间：3秒 （聚合时间、聚合数据包大小、聚合日志数任一满足即打包发送）
    -   聚合数据包大小：3MB
    -   聚合日志数：4096
    -   发送线程：4
    -   自定义tag：5

日志样例

1.  10个键值对，总数据量约为600字节。
2.  9个键值对，数据量约为350字节。

``` {#codeblock_ued_t4j_tm8}
__source__: 11.164.233.187
__tag__:1: 2
__tag__:5: 6
__tag__:a: b
__tag__:c: d
__tag__:tag_key: tag_value
__topic__: topic_test
_file_: /disk1/workspace/tools/aliyun-log-c-sdk/sample/log_producer_sample.c
_function_: log_producer_post_logs
_level_: LOG_PRODUCER_LEVEL_WARN
_line_: 248
_thread_: 40978304
LogHub: Real-time log collection and consumption
Search/Analytics: Query and real-time analysis
Interconnection: Grafana and JDBC/SQL92
Visualized: dashboard and report functions
```

测试结果

-   X86平台结果

    -   C Producer可以轻松到达90M/s的发送速度，每秒上传日志20W，占用CPU只有70%，内存140M。
    -   服务器在200条/s，发送数据对于cpu基本无影响（降低到0.01%以内）。
    -   客户线程发送一条数据（输出一条log）的平均耗时为：1.2us。
    ![X86平台结果](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13213/156863309532660_zh-CN.png)

-   树莓派平台结果

    -   在树莓派的测试中，由于CPU的频率只有600MHz，性能差不多是服务器的1/10左右，每秒可发送最多2W条日志。
    -   树莓派在20条/s的时候，发送数据对于cpu基本无影响（降低到0.01%以内）。
    -   客户线程发送一条数据（输出一条log）的平均耗时为：12us左右（树莓派通过USB连接到PC共享网络）。
    ![树莓派平台结果](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13213/156863309532661_zh-CN.png)


更多日志服务典型场景可以参见[云栖论坛](https://yq.aliyun.com/teams/4/type_blog-cid_8?spm=a2c4g.11186623.2.35.2dfc505eazEyHL) 和[最佳实践](intl.zh-CN/最佳实践/典型使用场景.md#)。

