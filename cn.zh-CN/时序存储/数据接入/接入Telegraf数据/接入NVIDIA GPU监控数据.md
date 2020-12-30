# 接入NVIDIA GPU监控数据

您可使用Telegraf采集NVIDIA GPU监控数据，再通过日志服务Logtail将Telegraf数据上传到MetricStore中，搭建NVIDIA GPU可视化监控方案。本文介绍如何通过日志服务来完成NVIDIA GPU监控数据的采集和可视化。

已在Linux服务器上安装Logtail（Linux Logtail 0.16.50及以上版本）。更多信息，请参见[安装Logtail（Linux系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)。

## 步骤1：安装NVIDIA GPU驱动

日志服务使用[nvidia\_smi](https://developer.nvidia.com/nvidia-system-management-interface)命令采集GPU信息，该命令包含在GPU驱动程序中，因此需要先安装驱动程序。具体操作，请参见[手动安装GPU驱动](/cn.zh-CN/实例/选择实例规格/GPU计算型/手动安装GPU驱动.md)。如果您使用阿里云ECS的GPU实例，则GPU实例中已默认安装驱动，可跳过此步骤。

## 步骤2：创建Logtail采集配置

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在接入数据区域，选择**NVIDIA GPU监控**。

3.  在选择日志空间页签中，选择目标Project和MetricStore，单击**下一步**。

    您也可以单击**立即创建**，重新创建Project和MetricStore。更多信息，请参见[创建Project](/cn.zh-CN/数据采集/准备工作/管理Project.md)和[创建MetricStore](/cn.zh-CN/时序存储/管理MetricStore.md)。

4.  在创建机器组页签中，创建机器组。

    -   如果您已有可用的机器组，请单击**使用现有机器组**。
    -   如果您还没有可用的机器组，请执行以下操作（以ECS为例）：
        1.  选择ECS实例安装Logtail。更多信息，请参见[安装Logtail（ECS实例）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（ECS实例）.md)。

            如果已在ECS上安装Logtail，请单击**确认安装完毕**。

            **说明：** 如果是自建集群、其他云厂商服务器，需要手动安装Logtail。更多信息，请参见[安装Logtail（Linux系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md#)。

        2.  安装完成后，单击**确认安装完毕**。
        3.  创建机器组。

            如何创建机器组，请参见[创建IP地址机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建IP地址机器组.md)或[创建用户自定义标识机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建用户自定义标识机器组.md)。

5.  选中目标机器组，将该机器组从**源机器组**移动到**应用机器组**，单击**下一步**。

    **说明：** 如果创建机器组后立刻应用，可能因为连接未生效，导致心跳为**FAIL**，您可单击**自动重试**。如果还未解决，请参见[Logtail机器组无心跳]()进行排查。

6.  在数据源设置页签中，配置如下参数。

    |参数名称|说明|
    |----|--|
    |配置名称|Logtail配置名称。|
    |集群名|集群名称。配置该参数后，日志服务会为您的数据添加cluster=集群名的标签。**说明：** 请确保该集群名称唯一，否则将出现数据冲突。 |
    |nvidia-smi路径|安装Nvidia-smi命令的路径。默认不需要填写。|
    |自定义标签|一个MetricStore下可创建多个Logtail配置，您可以使用**自定义标签**为通过该Logtail配置采集到的数据添加标签。单击**+**，添加自定义标签，支持添加多个标签。添加的标签将加入到每一条数据中。 |


## 常见问题

如何查看Telegraf采集是否正常？

您可以在服务器上查看/etc/ilogtail/telegraf/telegraf.log文件中记录的日志进行判断，还可以将该日志采集到日志服务中进行查询。

-   查询分析

    配置完成后，Telegraf将采集到的监控数据通过Logtail上传到日志服务MetricStore中。您可以在MetricStore查询分析页面进行查询分析操作，详情请参见[查询分析时序数据](/cn.zh-CN/时序存储/查询与分析/查询分析时序数据.md)。

-   可视化

    完成NVIDIA GPU监控相关配置后，日志服务自动在对应Project中生成名为NVIDIA\_GPU监控\_集群名称的仪表盘，您可以直接使用该仪表盘，还可以进行告警设置等操作。


