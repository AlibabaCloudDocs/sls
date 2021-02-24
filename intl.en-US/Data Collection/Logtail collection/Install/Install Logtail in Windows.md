# Install Logtail in Windows

This topic describes how to install, upgrade, and uninstall Logtail on a Windows server.

1.  At least one Windows server is available.
2.  The network type required for log collection is determined based on the server type and the region where the server resides. For more information, see [Select a network type](/intl.en-US/Data Collection/Logtail collection/Select a network type.md).

## Supported systems

Logtail supports the following Windows operating systems:

**Note:** Logtail supports X86 and X86\_64 for Microsoft Windows Server 2008 and Microsoft Windows 7. Logtail supports only x86\_64 for other Windows operating systems.

-   Microsoft Windows Server 2008
-   Microsoft Windows Server 2012
-   Microsoft Windows Server 2016
-   Microsoft Windows Server 2019
-   Microsoft Windows 7
-   Microsoft Windows 10
-   Microsoft Windows Server Version 1909
-   Microsoft Windows Server Version 2004

## Install Logtail

1.  Download the Logtail installation package.

    -   For the regions inside China: [Logtail installation package](https://logtail-release.oss-cn-hangzhou.aliyuncs.com/win/logtail_installer.zip)
    -   For the regions outside China: [Logtail installation package](https://logtail-release-global.log-global.aliyuncs.com/win/logtail_installer.zip)
2.  Decompress the `logtail_installer.zip` package to the directory where the Logtail installation package is stored.

3.  Run the installation command based on the network type of the server and the region where the Log Service project resides.

    Open Windows PowerShell or Command Prompt as an administrator. Go to the `logtail_installer` directory where you decompressed your installation package, and then run the installation command. The following table lists the installation commands that correspond to each network type and region.

    |Region|Alibaba Cloud internal network|Internet|Global Accelerator endpoint|
    |:-----|:-----------------------------|:-------|:--------------------------|
    |**China \(Qingdao\)**|`.\logtail_installer.exe install cn-qingdao`|`.\logtail_installer.exe install cn-qingdao-internet`|`.\logtail_installer.exe install cn-qingdao-acceleration`|
    |**China \(Beijing\)**|`.\logtail_installer.exe install cn-beijing`|`.\logtail_installer.exe install cn-beijing-internet`|`.\logtail_installer.exe install cn-beijing-acceleration`|
    |**China \(Zhangjiakou-Beijing Winter Olympics\)**|`.\logtail_installer.exe install cn-zhangjiakou`|`.\logtail_installer.exe install cn-zhangjiakou-internet`|`.\logtail_installer.exe install cn-zhangjiakou-acceleration`|
    |**China \(Hohhot\)**|`.\logtail_installer.exe install cn-huhehaote`|`.\logtail_installer.exe install cn-huhehaote-internet`|`.\logtail_installer.exe install cn-huhehaote-acceleration`|
    |**China \(Ulanqab\)**|`.\logtail_installer.exe install cn-wulanchabu`|`.\logtail_installer.exe install cn-wulanchabu-internet`|`.\logtail_installer.exe install cn-wulanchabu-acceleration`|
    |**China \(Hangzhou\)**|`.\logtail_installer.exe install cn-hangzhou`|`.\logtail_installer.exe install cn-hangzhou-internet`|`.\logtail_installer.exe install cn-hangzhou-acceleration`|
    |**China \(Shanghai\)**|`.\logtail_installer.exe install cn-shanghai`|`.\logtail_installer.exe install cn-shanghai-internet`|`.\logtail_installer.exe install cn-shanghai-acceleration`|
    |**China \(Shenzhen\)**|`.\logtail_installer.exe install cn-shenzhen`|`.\logtail_installer.exe install cn-shenzhen-internet`|`.\logtail_installer.exe install cn-shenzhen-acceleration`|
    |**China \(Heyuan\)**|`.\logtail_installer.exe install cn-heyuan`|`.\logtail_installer.exe install cn-heyuan-internet`|`.\logtail_installer.exe install cn-heyuan-acceleration`|
    |**China \(Guangzhou\)**|`.\logtail_installer.exe install cn-guangzhou`|`.\logtail_installer.exe install cn-guangzhou-internet`|`.\logtail_installer.exe install cn-guangzhou-acceleration`|
    |**China \(Chengdu\)**|`.\logtail_installer.exe install cn-chengdu`|`.\logtail_installer.exe install cn-chengdu-internet`|`.\logtail_installer.exe install cn-chengdu-acceleration`|
    |**China \(Hong Kong\)**|`.\logtail_installer.exe install cn-hongkong`|`.\logtail_installer.exe install cn-hongkong-internet`|`.\logtail_installer.exe install cn-hongkong-acceleration`|
    |**US \(Silicon Valley\)**|`.\logtail_installer.exe install us-west-1`|`.\logtail_installer.exe install us-west-1-internet`|`.\logtail_installer.exe install us-west-1-acceleration`|
    |**US \(Virginia\)**|`.\logtail_installer.exe install us-east-1`|`.\logtail_installer.exe install us-east-1-internet`|`.\logtail_installer.exe install us-east-1-acceleration`|
    |**Singapore \(Singapore\)**|`.\logtail_installer.exe install ap-southeast-1`|`.\logtail_installer.exe install ap-southeast-1-internet`|`.\logtail_installer.exe install ap-southeast-1-acceleration`|
    |**Australia \(Sydney\)**|`.\logtail_installer.exe install ap-southeast-2`|`.\logtail_installer.exe install ap-southeast-2-internet`|`.\logtail_installer.exe install ap-southeast-2-acceleration`|
    |**Malaysia \(Kuala Lumpur\)**|`.\logtail_installer.exe install ap-southeast-3`|`.\logtail_installer.exe install ap-southeast-3-internet`|`.\logtail_installer.exe install ap-southeast-3-acceleration`|
    |**Indonesia \(Jakarta\)**|`.\logtail_installer.exe install ap-southeast-5`|`.\logtail_installer.exe install ap-southeast-5-internet`|`.\logtail_installer.exe install ap-southeast-5-acceleration`|
    |**India \(Mumbai\)**|`.\logtail_installer.exe install ap-south-1`|`.\logtail_installer.exe install ap-south-1-internet`|`.\logtail_installer.exe install ap-south-1-acceleration`|
    |**Japan \(Tokyo\)**|`.\logtail_installer.exe install ap-northeast-1`|`.\logtail_installer.exe install ap-northeast-1-internet`|`.\logtail_installer.exe install ap-northeast-1-acceleration`|
    |**Germany \(Frankfurt\)**|`.\logtail_installer.exe install eu-central-1`|`.\logtail_installer.exe install eu-central-1-internet`|`.\logtail_installer.exe install eu-central-1-acceleration`|
    |**UAE \(Dubai\)**|`.\logtail_installer.exe install me-east-1`|`.\logtail_installer.exe install me-east-1-internet`|`.\logtail_installer.exe install me-east-1-acceleration`|
    |**UK \(London\)**|`.\logtail_installer.exe install eu-west-1`|`.\logtail_installer.exe install eu-west-1-internet`|`.\logtail_installer.exe install eu-west-1-acceleration`|
    |**Russia \(Moscow\)**|`.\logtail_installer.exe install rus-west-1`|`.\logtail_installer.exe install rus-west-1-internet`|`.\logtail_installer.exe install rus-west-1-acceleration`|

    **Note:** Log Service cannot obtain the owner information of ECS instances that do not belong to your current Alibaba Cloud account. In addition, Log Service cannot obtain the owner information of on-premises servers or servers that are provided by third-party cloud service providers. For more information, see [Configure an account ID for a server](/intl.en-US/Data Collection/Logtail collection/Machine Group/Configure an account ID for a server.md).


## Installation directory

After you run the installation command, Logtail is installed in the following directory by default. The directory cannot be modified.

-   32-bit Windows: C:\\Program Files\\Alibaba\\Logtail
-   64-bit Windows: C:\\Program Files \(x86\)\\Alibaba\\Logtail

**Note:** You can run 32-bit and 64-bit applications in a 64-bit Windows operating system. However, the operating system stores 32-bit applications in separate x86 folders to ensure compatibility.

Logtail for Windows is a 32-bit application. Therefore, it is installed in the Program Files \(x86\) folder in 64-bit Windows. If Logtail for 64-bit Windows becomes available in the future, Logtail will be installed in the Program Files folder.

## View the Logtail version

Open the app\_info.json file in the installation directory. The `logtail_version` field shows the version of Logtail.

In the following example, the version of Logtail is 1.0.0.0.

```
{
    "logtail_version" : "1.0.0.0"
}
```

## Upgrade Logtail

-   Automatic upgrade

    Logtail for Windows supports automatic upgrade. However, Logtail earlier than V1.0.0.0 must be manually upgraded.

-   Manual upgrade

    You can follow the procedure of Logtail installation to manually upgrade Logtail. You only need to download and decompress the latest installation package, and then install Logtail. For more information, see [Install Logtail](#section_es8_d9v_gr9).

    **Note:** During manual upgrade, the files in the original installation directory are deleted. We recommend that you back up the files before you perform the manual upgrade.


## Start and stop Logtail

1.  Choose **Start** \> **Control Panel** \> **Administrative Tools** \> **Services**.

2.  In the Services window, select the required service.

    -   For Logtail V0.x.x.x, select LogtailWorker.
    -   For Logtail V1.0.0.0 and later, select LogtailDaemon.
3.  You can right-click the corresponding service and select **Start**, **Stop**, or **Restart** from the shortcut menu.


## Uninstall Logtail

Open Windows PowerShell or Command Prompt as an administrator. Go to the `logtail_installer` directory where you decompressed your installation package, and then run the following installation command:

```
.\logtail_installer.exe uninstall
```

After Logtail is uninstalled, the directory of Logtail is deleted. However, some residual configuration data is retained in the C:\\LogtailData directory. You can manually delete the data. The residual configuration data includes the following information:

-   checkpoint: contains checkpoints of all plug-ins, such as Windows Event Log.
-   logtail\_check\_point: contains checkpoints of Logtail.
-   users: contains the IDs of Alibaba Cloud accounts.

