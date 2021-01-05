# Logtail基本问题

## 什么是Logtail？

Logtail是日志服务提供的一种便于日志接入的日志采集客户端。通过在您的机器上安装Logtail来监听指定的日志文件并自动把新写入到文件的日志上传到您所指定的日志库。

## Logtail是否可以采集静态不变的日志文件？

Logtail基于文件系统的修改事件来监听文件的变化，并将实时产生的日志发送到日志服务。如果日志文件没有发生任何修改行为，将不会被Logtail采集。

## Logtail支持哪些平台？

-   Linux

    支持如下版本的Linux x86-64（64位）服务器

    -   Alibaba Cloud Linux 2.1903
    -   RedHat Enterprise 6、7、8
    -   CentOS Linux 6、7、8
    -   Debian GNU/Linux 8、9、10
    -   Ubuntu 14.04、16.04、18.04、20.04
    -   SUSE Linux Enterprise Server 11、12、15
    -   OpenSUSE 15.1、15.2、42.3
    -   其他基于glibc 2.5及以上版本的Linux操作系统
-   Windows

    Microsoft Windows Server 2008和Microsoft Windows 7支持X86和X86\_64，其他版本仅支持X86\_64。

    -   Microsoft Windows Server 2008
    -   Microsoft Windows Server 2012
    -   Microsoft Windows Server 2016
    -   Microsoft Windows Server 2019
    -   Microsoft Windows 7
    -   Microsoft Windows 10
    -   Microsoft Windows Server Version 1909
    -   Microsoft Windows Server Version 2004

## 如何安装、升级Logtail客户端？

-   安装：请参见[安装Logtail（ECS实例）](/intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（ECS实例）.md)、[安装Logtail（Linux系统）](/intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)或[安装Logtail（Windows系统）](/intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md)。
-   升级：请参见[升级Logtail（Linux）](/intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md#section_jcz_xmv_vdb)或[升级Logtail（Windows）](/intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md#section_fks_lwg_cfb)。

    **说明：** 正在使用中的Logtail，只能通过手动升级。


## 如何配置Logtail采集日志

日志服务支持通过Logtail采集文本日志和容器日志，还支持通过Logtail插件采集日志，具体操作请参见如下链接：

-   [采集文本日志](/intl.zh-CN/数据采集/Logtail采集/采集文本日志/概述.md)
-   [采集容器日志](/intl.zh-CN/数据采集/Logtail采集/采集容器日志/概述.md)
-   [使用Logtail插件采集日志](/intl.zh-CN/数据采集/Logtail采集/使用Logtail插件采集数据/概述.md)

## Logtail如何工作？

Logtail采集原理包括监听文件、读取文件、处理日志、过滤日志、聚合日志和发送数据等过程。更多信息，请参见[Logtail采集原理](/intl.zh-CN/数据采集/Logtail采集/简介/Logtail采集原理.md)。

## Logtail是否支持日志轮转？

Logtail支持日志轮转。例如：app.LOG文件通过日志轮转生成app.LOG.1、app.LOG.2等，Logtail会自动检测到日志轮转过程，并保证这个过程中不会丢失日志。

## Logtail如何处理网络异常？

网络异常、写入Quota满时，Logtail会将采集到的日志写入本地磁盘缓存，并在稍后进行重试。

**说明：**

-   磁盘缓存最大支持500 MB，新缓存会覆盖旧缓存。
-   超过24小时未发送成功的缓存文件将被自动删除。

## Logtail日志采集延时如何？

Logtail基于事件进行日志采集，一般会在3秒内将日志发往日志服务。

## 如何采集历史日志？

如果日志的时间与Logtail处理该日志的系统时间相差5分钟以上，即被认为是历史日志。Logtail默认只采集增量的日志文件，如果您需要采集历史日志文件，可使用Logtail自带的导入历史文件功能。更多信息，请参见[导入历史日志文件](/intl.zh-CN/数据采集/Logtail采集/采集文本日志/导入历史日志文件.md)。

## 修改Logtail配置后多久生效？

您在控制台上修改Logtail配置后，Logtail在3分钟之内加载新配置并生效。

## 如何排查Logtail采集日志问题？

Logtail采集问题排查思路如下所示，更多信息，请参见[Logtail排查简介]()。

1.  确认Logtail心跳状态为OK。
2.  确认日志文件中的日志在实时生成。
3.  确认Logtail配置中的正则表达式与日志内容相匹配。

