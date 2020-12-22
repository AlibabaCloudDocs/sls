# 安装Logtail（Windows系统）

本文介绍如何在Windows服务器上安装Logtail。

-   已拥有一台及以上的服务器。
-   已根据服务器类型和所在地域，确定采集日志时所需的网络类型。更多信息，请参见[选择网络](/intl.zh-CN/数据采集/Logtail采集/选择网络.md)。

## 支持的系统

支持如下Windows操作系统：

**说明：** 其中Microsoft Windows Server 2008和Microsoft Windows 7支持X86和X86\_64，其他版本仅支持X86\_64。

-   Microsoft Windows Server 2008
-   Microsoft Windows Server 2012
-   Microsoft Windows Server 2016
-   Microsoft Windows Server 2019
-   Microsoft Windows 7
-   Microsoft Windows 10
-   Microsoft Windows Server Version 1909
-   Microsoft Windows Server Version 2004

## 安装Logtail

1.  下载安装包。

    -   中国地域的下载地址：[Logtail安装包](https://logtail-release.oss-cn-hangzhou.aliyuncs.com/win/logtail_installer.zip)。
    -   海外地域的下载地址：[Logtail安装包](https://logtail-release-global.log-global.aliyuncs.com/win/logtail_installer.zip)。
2.  解压缩`logtail_installer.zip`到当前目录。

3.  根据网络类型和日志服务Project所在地域选择并执行安装命令。

    以管理员身份运行Windows Powershell或cmd，进入`logtail_installer`目录（您的安装包的解压目录），执行安装命令。

    |地域|阿里云内网（经典网络、VPC）|公网|全球加速|
    |:-|:--------------|:-|:---|
    |**华北 1（青岛）**|`.\logtail_installer.exe install cn-qingdao`|`.\logtail_installer.exe install cn-qingdao-internet`|`.\logtail_installer.exe install cn-qingdao-acceleration`|
    |**华北 2（北京）**|`.\logtail_installer.exe install cn-beijing`|`.\logtail_installer.exe install cn-beijing-internet`|`.\logtail_installer.exe install cn-beijing-acceleration`|
    |**华北 3 （张家口）**|`.\logtail_installer.exe install cn-zhangjiakou`|`.\logtail_installer.exe install cn-zhangjiakou-internet`|`.\logtail_installer.exe install cn-zhangjiakou-acceleration`|
    |**华北 5 （呼和浩特）**|`.\logtail_installer.exe install cn-huhehaote`|`.\logtail_installer.exe install cn-huhehaote-internet`|`.\logtail_installer.exe install cn-huhehaote-acceleration`|
    |**华北6（乌兰察布）**|`.\logtail_installer.exe install cn-wulanchabu`|`.\logtail_installer.exe install cn-wulanchabu-internet`|`.\logtail_installer.exe install cn-wulanchabu-acceleration`|
    |**华东 1（杭州）**|`.\logtail_installer.exe install cn-hangzhou`|`.\logtail_installer.exe install cn-hangzhou-internet`|`.\logtail_installer.exe install cn-hangzhou-acceleration`|
    |**华东 2（上海）**|`.\logtail_installer.exe install cn-shanghai`|`.\logtail_installer.exe install cn-shanghai-internet`|`.\logtail_installer.exe install cn-shanghai-acceleration`|
    |**华南 1（深圳）**|`.\logtail_installer.exe install cn-shenzhen`|`.\logtail_installer.exe install cn-shenzhen-internet`|`.\logtail_installer.exe install cn-shenzhen-acceleration`|
    |**华南2（河源）**|`.\logtail_installer.exe install cn-heyuan`|`.\logtail_installer.exe install cn-heyuan-internet`|`.\logtail_installer.exe install cn-heyuan-acceleration`|
    |**华南3（广州）**|`.\logtail_installer.exe install cn-guangzhou`|`.\logtail_installer.exe install cn-guangzhou-internet`|`.\logtail_installer.exe install cn-guangzhou-acceleration`|
    |**西南 1（成都）**|`.\logtail_installer.exe install cn-chengdu`|`.\logtail_installer.exe install cn-chengdu-internet`|`.\logtail_installer.exe install cn-chengdu-acceleration`|
    |**中国（香港）**|`.\logtail_installer.exe install cn-hongkong`|`.\logtail_installer.exe install cn-hongkong-internet`|`.\logtail_installer.exe install cn-hongkong-acceleration`|
    |**美国 （硅谷）**|`.\logtail_installer.exe install us-west-1`|`.\logtail_installer.exe install us-west-1-internet`|`.\logtail_installer.exe install us-west-1-acceleration`|
    |**美国（弗吉尼亚）**|`.\logtail_installer.exe install us-east-1`|`.\logtail_installer.exe install us-east-1-internet`|`.\logtail_installer.exe install us-east-1-acceleration`|
    |**新加坡**|`.\logtail_installer.exe install ap-southeast-1`|`.\logtail_installer.exe install ap-southeast-1-internet`|`.\logtail_installer.exe install ap-southeast-1-acceleration`|
    |**澳大利亚（悉尼）**|`.\logtail_installer.exe install ap-southeast-2`|`.\logtail_installer.exe install ap-southeast-2-internet`|`.\logtail_installer.exe install ap-southeast-2-acceleration`|
    |**马来西亚（吉隆坡）**|`.\logtail_installer.exe install ap-southeast-3`|`.\logtail_installer.exe install ap-southeast-3-internet`|`.\logtail_installer.exe install ap-southeast-3-acceleration`|
    |**印度尼西亚（雅加达）**|`.\logtail_installer.exe install ap-southeast-5`|`.\logtail_installer.exe install ap-southeast-5-internet`|`.\logtail_installer.exe install ap-southeast-5-acceleration`|
    |**印度（孟买）**|`.\logtail_installer.exe install ap-south-1`|`.\logtail_installer.exe install ap-south-1-internet`|`.\logtail_installer.exe install ap-south-1-acceleration`|
    |**日本（东京）**|`.\logtail_installer.exe install ap-northeast-1`|`.\logtail_installer.exe install ap-northeast-1-internet`|`.\logtail_installer.exe install ap-northeast-1-acceleration`|
    |**德国（法兰克福）**|`.\logtail_installer.exe install eu-central-1`|`.\logtail_installer.exe install eu-central-1-internet`|`.\logtail_installer.exe install eu-central-1-acceleration`|
    |**阿联酋（迪拜）**|`.\logtail_installer.exe install me-east-1`|`.\logtail_installer.exe install me-east-1-internet`|`.\logtail_installer.exe install me-east-1-acceleration`|
    |**英国（伦敦）**|`.\logtail_installer.exe install eu-west-1`|`.\logtail_installer.exe install eu-west-1-internet`|`.\logtail_installer.exe install eu-west-1-acceleration`|
    |**俄罗斯（莫斯科）**|`.\logtail_installer.exe install rus-west-1`|`.\logtail_installer.exe install rus-west-1-internet`|`.\logtail_installer.exe install rus-west-1-acceleration`|

    **说明：** 日志服务无法获取非本账号下ECS、自建IDC或其他云厂商服务器的属主信息，在安装Logtail后需手动配置用户标识（AliUid）。更多信息，详情请参见 [配置用户标识](/intl.zh-CN/数据采集/Logtail采集/机器组/配置用户标识.md)。


## 安装路径

执行Logtail安装命令后，默认安装Logtail到指定路径下，不支持修改。

-   32位Windows系统：C:\\Program Files\\Alibaba\\Logtail
-   64位Windows系统：C:\\Program Files \(x86\)\\Alibaba\\Logtail

**说明：** Windows 64位操作系统支持运行32/64位应用程序，但是出于兼容性考虑，在Windows 64位操作系统上，Windows会使用单独的x86目录来存放32位应用程序。

Windows版本的Logtail是32位程序，所以在64位操作系统上的安装目录为Program Files \(x86\)。如果日志服务推出64位的Windows版本Logtail，会自动安装到Program Files目录下。

## 查看Logtail版本

您可以通过安装路径下的app\_info.json文件中的`logtail_version`字段查看Logtail版本。

例如，以下内容表示Logtail的版本号为1.0.0.0。

```
{
    "logtail_version" : "1.0.0.0"
}
```

## 升级Logtail

-   自动升级

    通常情况下，Windows版本的Logtail支持自动升级。但将1.0.0.0之前的旧版升级为1.0.0.0及以上版本时，必须手动升级。

-   手动升级

    手动升级的操作和安装Logtail的操作相同，您只需要下载并解压最新的安装包，然后按照步骤执行安装即可。更多信息，请参见[安装Logtail](#section_es8_d9v_gr9)。

    **说明：** 手动升级相当于自动卸载并重新安装，会删除掉您原先安装目录中的内容，请您在执行手动升级前做好备份工作。


## 手动启动和停止Logtail

1.  选择**开始** \> **控制面板** \> **管理工具** \> **服务**。

2.  在服务对话框中，选择对应的服务。

    -   如果是0.x.x.x版本，选择LogtailWorker服务。
    -   如果是1.0.0.0及以上版本，选择LogtailDaemon服务。
3.  右键选择对应的操作：**启动**、**停止**、**重新启动**。


## 卸载Logtail

以管理员身份运行Windows Powershell或cmd进入`logtail_installer`目录（安装包的解压目录），执行如下命令。

```
.\logtail_installer.exe uninstall
```

卸载成功后，您的Logtail的安装目录会被删除，但仍有部分配置被保留在C:\\LogtailData目录中，您可以根据实际情况进行手动删除。遗留信息包括：

-   checkpoint：保存所有插件（比如Windows event log插件）的checkpoint信息。
-   logtail\_check\_point：保存Logtail主体部分的checkpoint信息。
-   users：保存所配置的用户标识信息。

