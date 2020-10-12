# 创建IP地址机器组

日志服务支持使用服务器IP地址定义机器组，本文介绍如何在日志服务控制台上创建IP地址机器组。

-   已创建Project和Logstore，详情请参见[创建Project和Logstore](/cn.zh-CN/快速入门/快速入门.md)。
-   已有一台及以上的服务器。
    -   如果是阿里云ECS，请确保ECS和日志服务Project属于同一账号同一地域。
    -   如果是与日志服务属于不同账号的ECS、其他云厂商的服务器或自建IDC，请先配置用户标识，详情请参见[配置用户标识](/cn.zh-CN/数据采集/Logtail采集/机器组/配置用户标识.md)。
-   已在服务器上安装Logtail，详情请参见[安装Logtail（Linux系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)和[安装Logtail（Windows系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md)。

1.  获取服务器IP地址。

    Logtail自动获取的IP地址记录在app\_info.json文件的ip字段中。

    在已安装Logtail的服务器上查看app\_info.json文件，路径为：

    -   Linux：/usr/local/ilogtail/app\_info.json
    -   Windows x64：C:\\Program Files \(x86\)\\Alibaba\\Logtail\\app\_info.json
    -   Windows x32：C:\\Program Files\\Alibaba\\Logtail\\app\_info.json
    例如，在Linux中查看服务器IP地址，如下图所示。

    ![IP地址](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8030559951/p10497.png)

2.  登录[日志服务控制台](https://sls.console.aliyun.com)。

3.  在Project列表区域，单击目标Project。

4.  在左侧导航栏中，单击**机器组。**

5.  单击**机器组**后的![机器组](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9030559951/p52484.png)图标，选择**创建机器组**。

6.  在创建机器组对话框中，配置如下参数，单击**确定**。

    |参数|说明|
    |--|--|
    |名称|名称只能包含小写字母、数字、连字符（-）和下划线（\_）且必须以小写字母或数字开头和结尾，长度为3~128字节。 **说明：** 创建后，不支持修改机器组名称，请谨慎填写。 |
    |机器组标识|选择**IP地址**。|
    |机器组Topic|机器组Topic用于区分不同服务器产生的日志数据，详情请参见[日志主题](/cn.zh-CN/数据采集/Logtail采集/采集文本日志/日志主题.md)。|
    |IP地址|填写[步骤 1](#ip)中获取到的服务器IP地址。 **说明：**

    -   机器组中存在多台服务器时，IP地址之间请用换行符分割。
    -   请勿将Windows服务器和Linux服务器添加到同一机器组中。 |

7.  查看机器组状态。

    1.  在机器组列表中，单击目标机器组。

    2.  在机器组配置页面，查看服务器及其状态。

        **心跳**为**OK**表示服务器与日志服务的连接正常，如果显示**FAIL**请参见[Logtail 机器无心跳]()。

        ![查看机器组状态](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8030559951/p97821.png)


