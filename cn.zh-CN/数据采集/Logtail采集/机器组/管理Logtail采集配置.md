# 管理Logtail采集配置

本文介绍如何在日志服务控制台上创建、查看、修改及删除Logtail采集配置等操作。

## 创建Logtail采集配置

在日志服务控制台上创建Logtail采集配置，详情请参见[采集文本日志](/cn.zh-CN/数据采集/Logtail采集/采集文本日志/概述.md)。

## 查看Logtail采集配置

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在Project列表区域，单击目标Project。

3.  在**日志存储** \> **日志库**页签中，单击目标日志库前面的**\>**，依次选择**数据接入** \> **Logtail配置**。

4.  单击目标Logtail采集配置，查看Logtail采集配置详情。


## 修改Logtail采集配置

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在Project列表区域，单击目标Project。

3.  在**日志存储** \> **日志库**页签中，单击目标日志库前面的**\>**，依次选择**数据接入** \> **Logtail配置**。

4.  在**Logtail配置**列表中，单击目标Logtail采集配置。

5.  在Logtail配置页面，单击**修改**。

6.  根据需求，修改相关配置，并单击**保存**。

    详细参数说明请参见[采集文本日志](/cn.zh-CN/数据采集/Logtail采集/采集文本日志/极简模式.md)。


## 删除Logtail采集配置

1.  在**Logtail配置**列表中，单击目标Logtail采集配置右侧的![Logtail配置管理](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0130559951/p52766.png)图标，选择**删除**。

2.  在删除确认框中，单击**确认**。

    删除成功后，该Logtail采集配置与机器组解除绑定，Logtail停止采集该Logtail采集配置对应的日志。

    **说明：** 删除logstore前，必须删除其对应的所有Logtail采集配置。


