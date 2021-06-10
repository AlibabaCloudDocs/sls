# 通过Logtail插件接入Prometheus监控数据

日志服务Logtail支持采集Prometheus格式的各类指标数据，例如Node Exporter、Kafka Exporter及应用所涉及的Prometheus指标等。本文介绍如何通过日志服务控制台创建Logtail采集配置来采集Prometheus监控数据。

已创建MetricStore。具体操作，请参见[创建MetricStore](/cn.zh-CN/时序存储/管理MetricStore.md)。

## 操作步骤

**说明：** 一个Logtail上只能同时存在一个Prometheus的Logtail配置。如果同时存在多个，则随机生效一个。

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在接入数据区域，选择**抓取Prometheus格式指标**。

3.  在选择日志空间向导中，选择目标Project和MetricStore，单击**下一步**。

4.  创建机器组。

    -   如果您已有可用的机器组，请单击**使用现有机器组**。
    -   如果您还没有可用的机器组，请执行以下操作（以ECS为例）。
        1.  在ECS机器页签中，通过手动选择实例方式选择目标ECS实例，单击**立即执行**。

            更多信息，请参见[安装Logtail（ECS实例）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（ECS实例）.md)。

            **说明：** 如果是自建集群、其他云厂商服务器，需要手动安装Linux Logtail 0.16.66及以上版本。更多信息，请参见[安装Logtail（Linux系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)。

        2.  安装完成后，单击**确认安装完毕**。
        3.  在创建机器组页面，输入**名称**，单击**下一步**。

            日志服务支持创建IP地址机器组和用户自定义标识机器组，详细参数说明请参见[创建IP地址机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建IP地址机器组.md)和[创建用户自定义标识机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建用户自定义标识机器组.md)。

5.  选中目标机器组，将该机器组从**源机器组**移动到**应用机器组**，单击**下一步**。

    **说明：** 如果创建机器组后立刻应用，可能因为连接未生效，导致心跳为**FAIL**，您可单击**自动重试**。如果还未解决，请参见[Logtail机器组无心跳](/cn.zh-CN/数据采集/FAQ/日志服务控制台中Logtail机器组无心跳的排查思路.md)进行排查。

6.  在数据源设置向导中，设置**配置名称**和**插件配置**，单击**下一步**。

    **插件配置**包括inputs和processors。日志服务已提供inputs模板，包括global和scrape\_configs两个节点。

    -   inputs为Logtail采集配置，必选项，请根据您的数据源配置。

        **说明：**

        -   Prometheus格式指标的抓取配置和Prometheus本身的抓取配置规则一致，只支持global和scrape\_configs两个节点的配置。更多信息，请参见[Prometheus抓取配置规则](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config)。
        -   一个inputs中只允许配置一个类型的数据源。
    -   processors为Logtail处理配置，可选项。更多信息，请参见[追加字段](/cn.zh-CN/数据采集/Logtail采集/使用Logtail插件处理数据/追加字段.md)。

-   查询和分析

    采集到数据后，您可以在MetricStore查询和分析页面进行查询和分析操作。具体操作，请参见[查询和分析时序数据](/cn.zh-CN/时序存储/查询与分析/查询和分析时序数据.md)。

-   日志服务可视化

    日志服务自动在对应Project中生成主机监控仪表盘，您可以直接使用该仪表盘查看查询分析结果，及进行告警等相关操作。具体操作，请参见[仪表盘](/cn.zh-CN/可视化/仪表盘/简介.md)。

-   Grafana可视化

    日志服务的时序数据支持直接对接Grafana进行可视化。具体操作，请参见[时序数据对接Grafana](/cn.zh-CN/时序存储/可视化/时序数据对接Grafana.md)。


