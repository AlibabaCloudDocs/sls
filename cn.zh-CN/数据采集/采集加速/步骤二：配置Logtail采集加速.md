# 步骤二：配置Logtail采集加速

本文介绍在开启日志服务的全球加速服务前已安装了Logtail，则如何将Logtail采集模式切换为全球加速，实现采集加速。

已开启日志服务的全球加速功能。更多信息，请参见[步骤一：开启全球加速服务](/cn.zh-CN/数据采集/采集加速/步骤一：开启全球加速服务.md)。

## 配置说明

-   如果开启全球加速后再安装Logtail，则在安装Logtail时将安装模式配置为**全球加速**，即可采用全球加速模式采集日志。更多信息，请参见[安装Logtail（Linux系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)。
-   如果开启全球加速前已安装Logtail，则请参见本文档手动切换Logtail采集模式为**全球加速**。

## 操作步骤

1.  停止Logtail。

    -   Linux系统

        以root用户执行`/etc/init.d/ilogtaild stop`命令。

    -   Windows系统
        1.  选择**开始** \> **控制面板** \> **管理工具** \> **服务**。
        2.  在服务对话框中，找到LogtailWorker，右键单击**停止**。
2.  修改Logtail启动配置文件ilogtail\_config.json。

    将参数data\_server\_list中的endpoint一行替换为`log-global.aliyuncs.com`。更多信息，请参见[启动参数配置文件（ilogtail\_config.json）](/cn.zh-CN/数据采集/Logtail采集/简介/Logtail配置文件和记录文件.md)。

3.  启动Logtail。

    -   Linux系统

        以root用户执行`/etc/init.d/ilogtaild start`命令。

    -   Windows系统
        1.  选择**开始** \> **控制面板** \> **管理工具** \> **服务**。
        2.  在服务对话框中，找到LogtailWorker，右键单击**启动**。

