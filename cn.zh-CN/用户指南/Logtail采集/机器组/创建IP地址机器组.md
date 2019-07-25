# 创建IP地址机器组 {#task_wc3_xn1_ry .task}

日志服务支持创建IP地址机器组。将Logtail获取到的服务器IP地址添加进IP地址机器组中，则可以使用同样的Logtail配置采集这些服务器的日志。

-   已创建Project和Logstore。
-   已有一台及以上的服务器，如果是阿里云ECS云服务器，请确保ECS和当前日志服务 Project 在同一阿里云地域下。
-   已为服务器安装了Logtail。详细步骤请参考[安装Logtail（Linux系统）](cn.zh-CN/用户指南/Logtail采集/安装/安装Logtail（Linux系统）.md)和[安装Logtail（Windows系统）](cn.zh-CN/用户指南/Logtail采集/安装/安装Logtail（Windows系统）.md)。
-   其他云厂商服务器、自建IDC、其他阿里云账号下的ECS请先配置AliUid（用户标识），详细步骤请参考[配置主账号AliUid](cn.zh-CN/用户指南/Logtail采集/机器组/配置主账号AliUid.md)。

**数据采集是否使用阿里云内网，与机器组中填写的IP地址是否为私网IP地址无关**。如果您的服务器是阿里云ECS云服务器，ECS和当前日志服务Project在同一阿里云地域，并且安装Logtail时选择了**阿里云内网（经典网络/VPC）**模式，只有这种情况下您的日志数据才可以通过阿里云内网采集到日志服务。

1.  查看服务器IP地址，即Logtail自动获取到的IP地址。 Logtail自动获取到的IP地址记录在文件app\_info.json的ip字段中。

    在安装了Logtail的服务器上查看app\_info.json文件，路径为：

    -   Linux：/usr/local/ilogtail/app\_info.json
    -   Windows x64：C:\\Program Files \(x86\)\\Alibaba\\Logtail\\app\_info.json
    -   Windows x32：C:\\Program Files\\Alibaba\\Logtail\\app\_info.json
    例如，在Linux中查看服务器IP地址：

    ![IP地址](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13080/156401994210497_zh-CN.png)

2.  登录[日志服务控制台](https://sls.console.aliyun.com)，单击Project名称。 
3.  单击左侧导航栏的**机器组**展开机器组列表 。
4.  单击**机器组**后的![机器组](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13080/156401994252484_zh-CN.png)图标，选择**创建机器组**。 您也可以在数据接入向导中创建机器组。
5.  创建机器组。 
    1.  填写**机器组名称**。 名称只能包含小写字母、数字、连字符（-）和下划线（\_）且必须以小写字母或数字开头和结尾，长度为3~128字节。

        **说明：** 不支持修改机器组名称，请谨慎填写。

    2.  选择**机器组标识**为**IP地址**。
    3.  填写**IP地址**。 请填写[1](#ip)中获取到的服务器IP地址。

        **说明：** 

        -   请确保您已按照[1](#ip)获取了服务器IP地址。
        -   机器组中存在多台服务器时，IP地址之间请用换行符分割。
        -   请勿将Windows服务器和Linux服务器添加到同一机器组中。
        ![创建机器组](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13080/15640199425277_zh-CN.png)

6.  填写**机器组Topic**。 机器组Topic的详细信息请参考[生成主题](cn.zh-CN/用户指南/Logtail采集/文本日志/生成主题.md)。
7.  单击 **确认**。

您可以在机器组列表中查看刚创建的机器组。

![机器组列表](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13080/15640199425279_zh-CN.png)

