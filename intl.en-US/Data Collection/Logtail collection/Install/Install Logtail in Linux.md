# Install Logtail in Linux

This topic describes how to install, upgrade, and uninstall Logtail on a Linux server.

## Prerequisites

-   At least one Linux server is available.
-   The network type required for log collection is determined based on the server type and the region where the server resides. For more information, see [Select a network type](/intl.en-US/Data Collection/Logtail collection/Select a network type.md).

## Supported systems

Logtail supports the following x86-64 \(64-bit\) Linux servers:

-   Aliyun Linux 2
-   Red Hat Enterprise Linux 6, Red Hat Enterprise Linux 7, and Red Hat Enterprise Linux 8
-   CentOS 6, CentOS 7, and CentOS 8
-   Debian GNU/Linux 8, Debian GNU/Linux 9, and Debian GNU/Linux 10
-   Ubuntu 14.04, Ubuntu 16.04, Ubuntu 18.04, and Ubuntu 20.04
-   SUSE Linux Enterprise Server 11, SUSE Linux Enterprise Server 12, SUSE Linux Enterprise Server 15
-   openSUSE Leap 15.1, openSUSE Leap 15.2, and openSUSE Leap 42.3
-   Glibc 2.5 or later

## Precautions

-   If you have installed Logtail, the installer uninstalls the existing version of Logtail, deletes the /usr/local/ilogtail directory, and then reinstalls Logtail. By default, Logtail runs after it is installed and launches when the system reboots.
-   If the installation fails, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm?spm=a2796.7919406.0.dcontactus3.676a2d23RjosdV#/ticket/add/?productId=1210).
-   After Logtail is installed, you must update the Logtail configuration if the network type of the ECS instance is switched from the classic network to Virtual Private Cloud \(VPC\). For more information, see [Do I need to update Logtail settings after the network type is changed?]().

## Installation methods

Select the corresponding installation command based on the network type of the server.

-   [Install Logtail over the Alibaba Cloud internal network](#section_inb_fbn_1fb)
-   [Install Logtail over the Internet](#section_lhn_hbn_1fb)
-   [Install Logtail by using the global acceleration feature](#section_xpy_tvs_s2b)

Before you run the installation command, replace the $\{your\_region\_name\} parameter with the region where the project resides. The following table lists the values of the $\{your\_region\_name\} parameter for each region.

|Region|$\{your\_region\_name\}|Region|$\{your\_region\_name\}|
|:-----|:----------------------|:-----|:----------------------|
|**China \(Hangzhou\)**|cn-hangzhou|**US \(Virginia\)**|us-east-1|
|**China \(Shanghai\)**|cn-shanghai|**Singapore \(Singapore\)**|ap-southeast-1|
|**China \(Qingdao\)**|cn-qingdao|**Australia \(Sydney\)**|ap-southeast-2|
|**China \(Beijing\)**|cn-beijing|**Malaysia \(Kuala Lumpur\)**|ap-southeast-3|
|**China \(Zhangjiakou-Beijing Winter Olympics\)**|cn-zhangjiakou|**Indonesia \(Jakarta\)**|ap-southeast-5|
|**China \(Hohhot\)**|cn-huhehaote|**India \(Mumbai\)**|ap-south-1|
|**China \(Ulanqab\)**|cn-wulanchabu|**Japan \(Tokyo\)**|ap-northeast-1|
|**China \(Shenzhen\)**|cn-shenzhen|**Germany \(Frankfurt\)**|eu-central-1|
|**China \(Heyuan\)**|cn-heyuan|**UAE \(Dubai\)**|me-east-1|
|**China \(Guangzhou\)**|cn-guangzhou|**UK \(London\)**|eu-west-1|
|**China \(Chengdu\)**|cn-chengdu|**N/A**|**N/A**|
|**China \(Hong Kong\)**|cn-hongkong|**N/A**|**N/A**|
|**Russia \(Moscow\)**|rus-west-1|**N/A**|**N/A**|
|**US \(Silicon Valley\)**|us-west-1|N/A|N/A|

## Install Logtail over the Alibaba Cloud internal network

-   If you cannot identify the region where the ECS instance resides, you can use the auto parameter in the Logtail installation script to install Logtail.

    After you specify the auto parameter in the installation command, the Logtail installation script obtains and uses the metadata of the ECS instance to identify the region. For more information about the metadata of the ECS instance, see [Metadata](/intl.en-US/Instance/Manage instances/Metadata/Metadata.md).

    1.  Download the Logtail installation script over the Internet.

        The download generates about 10 KB Internet traffic.

        ```
        wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh;chmod 755 logtail.sh
        ```

    2.  Install Logtail by using the auto parameter.

        You can run the following command in PowerShell or Command Prompt to install Logtail. The Logtail installation package of the corresponding region is automatically downloaded. The download does not generate Internet traffic.

        ```
        ./logtail.sh install auto
        ```

-   If you have identified the region where the ECS instance resides, select the installation command that corresponds to the region.

    Download the Logtail installation script by using the internal network. The download does not generate Internet traffic.

    1.  Obtain the value of the `${your_region_name}` parameter for the region where the project resides.

        For more information about the value of the `${your_region_name}` parameter for each region, see [Region names for Logtail installation](#table_eyz_pmv_vdb). For example, the value of the `${your_region_name}` parameter for the **China \(Hangzhou\)** region is `cn-hangzhou`.

    2.  Replace the `${your_region_name}` parameter with the actual region name, and then run the installation command.

        ```
        wget http://logtail-release-$\{your\_region\_name\}.oss-$\{your\_region\_name\}-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install $\{your\_region\_name\}
        ```

    The following table lists the installation commands that correspond to each region where the project resides.

    |Region|Installation command|
    |:-----|:-------------------|
    |**China \(Hangzhou\)**|    ```
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hangzhou
    ``` |
    |**China \(Shanghai\)**|    ```
wget http://logtail-release-cn-shanghai.oss-cn-shanghai-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shanghai
    ``` |
    |**China \(Qingdao\)**|    ```
wget http://logtail-release-cn-qingdao.oss-cn-qingdao-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-qingdao
    ``` |
    |**China \(Beijing\)**|    ```
wget http://logtail-release-cn-beijing.oss-cn-beijing-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-beijing
    ``` |
    |**China \(Zhangjiakou-Beijing Winter Olympics\)**|    ```
wget http://logtail-release-cn-zhangjiakou.oss-cn-zhangjiakou-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-zhangjiakou
    ``` |
    |**China \(Hohhot\)**|    ```
wget http://logtail-release-cn-huhehaote.oss-cn-huhehaote-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-huhehaote
    ``` |
    |**China \(Ulanqab\)**|    ```
wget http://logtail-release-cn-wulanchabu.oss-cn-wulanchabu-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-wulanchabu
    ``` |
    |**China \(Shenzhen\)**|    ```
wget http://logtail-release-cn-shenzhen.oss-cn-shenzhen-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shenzhen
    ``` |
    |**China \(Heyuan\)**|    ```
wget http://logtail-release-cn-heyuan.oss-cn-heyuan-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-heyuan
    ``` |
    |**China \(Guangzhou\)**|    ```
wget http://logtail-release-cn-guangzhou.oss-cn-guangzhou-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-guangzhou
    ``` |
    |**China \(Chengdu\)**|    ```
wget http://logtail-release-cn-chengdu.oss-cn-chengdu-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-chengdu
    ``` |
    |**China \(Hong Kong\)**|    ```
wget http://logtail-release-cn-hongkong.oss-cn-hongkong-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hongkong
    ``` |
    |**US \(Silicon Valley\)**|    ```
wget http://logtail-release-us-west-1.oss-us-west-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-west-1
    ``` |
    |**US \(Virginia\)**|    ```
wget http://logtail-release-us-east-1.oss-us-east-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-east-1
    ``` |
    |**Singapore \(Singapore\)**|    ```
wget http://logtail-release-ap-southeast-1.oss-ap-southeast-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-1
    ``` |
    |**Australia \(Sydney\)**|    ```
wget http://logtail-release-ap-southeast-2.oss-ap-southeast-2-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-2
    ``` |
    |**Malaysia \(Kuala Lumpur\)**|    ```
wget http://logtail-release-ap-southeast-3.oss-ap-southeast-3-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-3
    ``` |
    |**Indonesia \(Jakarta\)**|    ```
wget http://logtail-release-ap-southeast-5.oss-ap-southeast-5-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-5
    ``` |
    |**Japan \(Tokyo\)**|    ```
wget http://logtail-release-ap-northeast-1.oss-ap-northeast-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-northeast-1
    ``` |
    |**India \(Mumbai\)**|    ```
wget http://logtail-release-ap-south-1.oss-ap-south-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-south-1
    ``` |
    |**Germany \(Frankfurt\)**|    ```
wget http://logtail-release-eu-central-1.oss-eu-central-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-central-1
    ``` |
    |**UAE \(Dubai\)**|    ```
wget http://logtail-release-me-east-1.oss-me-east-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install me-east-1
    ``` |
    |**UK \(London\)**|    ```
wget http://logtail-release-eu-west-1.oss-eu-west-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-west-1
    ``` |
    |**Russia \(Moscow\)**|    ```
wget http://logtail-release-rus-west-1.oss-rus-west-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install rus-west-1
    ``` |


## Install Logtail over the Internet

1.  Obtain the value of the `${your_region_name}` parameter for the region where the project resides.

    For more information about the value of the `${your_region_name}` parameter for each region, see [Region names for Logtail installation](#table_eyz_pmv_vdb). For example, the value of the `${your_region_name}` parameter for the **China \(Hangzhou\)** region is `cn-hangzhou`.

2.  Replace the `${your_region_name}` parameter with the actual region name, and then run the installation command.

    ```
    wget http://logtail-release-$\{your\_region\_name\}.oss-$\{your\_region\_name\}.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install $\{your\_region\_name\}-internet
    ```


The following table lists the installation commands that correspond to each region where the project resides.

|Region|Installation command|
|:-----|:-------------------|
|**China \(Hangzhou\)**|```
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hangzhou-internet
``` |
|**China \(Shanghai\)**|```
wget http://logtail-release-cn-shanghai.oss-cn-shanghai.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shanghai-internet
``` |
|**China \(Qingdao\)**|```
wget http://logtail-release-cn-qingdao.oss-cn-qingdao.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-qingdao-internet
``` |
|**China \(Beijing\)**|```
wget http://logtail-release-cn-beijing.oss-cn-beijing.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-beijing-internet
``` |
|**China \(Zhangjiakou-Beijing Winter Olympics\)**|```
wget http://logtail-release-cn-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-zhangjiakou-internet
``` |
|**China \(Hohhot\)**|```
wget http://logtail-release-cn-huhehaote.oss-cn-huhehaote.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-huhehaote-internet
``` |
|**China \(Ulanqab\)**|```
wget http://logtail-release-cn-wulanchabu.oss-cn-wulanchabu.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-wulanchabu-internet
``` |
|**China \(Shenzhen\)**|```
wget http://logtail-release-cn-shenzhen.oss-cn-shenzhen.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shenzhen-internet
``` |
|**China \(Heyuan\)**|```
wget http://logtail-release-cn-heyuan.oss-cn-heyuan.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-heyuan-internet
``` |
|**China \(Guangzhou\)**|```
wget http://logtail-release-cn-guangzhou.oss-cn-guangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-guangzhou-internet
``` |
|**China \(Chengdu\)**|```
wget http://logtail-release-cn-chengdu.oss-cn-chengdu.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-chengdu-internet
``` |
|**China \(Hong Kong\)**|```
wget http://logtail-release-cn-hongkong.oss-cn-hongkong.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hongkong-internet
``` |
|**US \(Silicon Valley\)**|```
wget http://logtail-release-us-west-1.oss-us-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-west-1-internet
``` |
|**US \(Virginia\)**|```
wget http://logtail-release-us-east-1.oss-us-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-east-1-internet
``` |
|**Singapore \(Singapore\)**|```
wget http://logtail-release-ap-southeast-1.oss-ap-southeast-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-1-acceleration
``` |
|**Australia \(Sydney\)**|```
wget http://logtail-release-ap-southeast-2.oss-ap-southeast-2.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-2-internet
``` |
|**Malaysia \(Kuala Lumpur\)**|```
wget http://logtail-release-ap-southeast-3.oss-ap-southeast-3.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-3-internet
``` |
|**Indonesia \(Jakarta\)**|```
wget http://logtail-release-ap-southeast-5.oss-ap-southeast-5.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-5-internet
``` |
|**Japan \(Tokyo\)**|```
wget http://logtail-release-ap-northeast-1.oss-ap-northeast-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-northeast-1-internet
``` |
|**Germany \(Frankfurt\)**|```
wget http://logtail-release-eu-central-1.oss-eu-central-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-central-1-internet
``` |
|**UAE \(Dubai\)**|```
wget http://logtail-release-me-east-1.oss-me-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install me-east-1-internet
``` |
|**India \(Mumbai\)**|```
wget http://logtail-release-ap-south-1.oss-ap-south-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-south-1-internet
``` |
|**UK \(London\)**|```
wget http://logtail-release-eu-west-1.oss-eu-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-west-1-internet
``` |
|**Russia \(Moscow\)**|```
wget http://logtail-release-rus-west-1.oss-rus-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install rus-west-1-internet
``` |

## Install Logtail by using the global acceleration feature

1.  Obtain the value of the $\{your\_region\_name\} parameter for the region where the Log Service project resides.

    For more information about the value of the `${your_region_name}` parameter for each region, see [Region names for Logtail installation](#table_eyz_pmv_vdb). For example, the value of the `${your_region_name}` parameter for the **China \(Hangzhou\)** region is `cn-hangzhou`.

2.  Replace the `${your_region_name}` parameter with the actual region name, and then run the installation command.

    ```
    wget http://logtail-release-$\{your\_region\_name\}.oss-$\{your\_region\_name\}.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install $\{your\_region\_name\}-acceleration
    ```


The following table lists the installation commands that correspond to each region.

|Region|Installation command|
|:-----|:-------------------|
|**China \(Beijing\)**|```
wget http://logtail-release-cn-beijing.oss-cn-beijing.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-beijing-acceleration
``` |
|**China \(Qingdao\)**|```
wget http://logtail-release-cn-qingdao.oss-cn-qingdao.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-qingdao-acceleration
``` |
|**China \(Hangzhou\)**|```
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hangzhou-acceleration
``` |
|**China \(Shanghai\)**|```
wget http://logtail-release-cn-shanghai.oss-cn-shanghai.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shanghai-acceleration
``` |
|**China \(Shenzhen\)**|```
wget http://logtail-release-cn-shenzhen.oss-cn-shenzhen.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shenzhen-acceleration
``` |
|**China \(Heyuan\)**|```
wget http://logtail-release-cn-heyuan.oss-cn-heyuan.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-heyuan-acceleration
``` |
|**China \(Guangzhou\)**|```
wget http://logtail-release-cn-guangzhou.oss-cn-guangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-guangzhou-acceleration
``` |
|**China \(Zhangjiakou-Beijing Winter Olympics\)**|```
wget http://logtail-release-cn-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-zhangjiakou-acceleration
``` |
|**China \(Hohhot\)**|```
wget http://logtail-release-cn-huhehaote.oss-cn-huhehaote.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-huhehaote-acceleration
``` |
|**China \(Ulanqab\)**|```
wget http://logtail-release-cn-wulanchabu.oss-cn-wulanchabu.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-wulanchabu-acceleration
``` |
|**China \(Chengdu\)**|```
wget http://logtail-release-cn-chengdu.oss-cn-chengdu.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-chengdu-acceleration
``` |
|**China \(Hong Kong\)**|```
wget http://logtail-release-cn-hongkong.oss-cn-hongkong.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hongkong-acceleration
``` |
|**US \(Silicon Valley\)**|```
wget http://logtail-release-us-west-1.oss-us-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-west-1-acceleration
``` |
|**US \(Virginia\)**|```
wget http://logtail-release-us-east-1.oss-us-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-east-1-acceleration
``` |
|**Singapore \(Singapore\)**|```
wget http://logtail-release-ap-southeast-1.oss-ap-southeast-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-1-acceleration
``` |
|**Australia \(Sydney\)**|```
wget http://logtail-release-ap-southeast-2.oss-ap-southeast-2.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-2-acceleration
``` |
|**Malaysia \(Kuala Lumpur\)**|```
wget http://logtail-release-ap-southeast-3.oss-ap-southeast-3.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-3-acceleration
``` |
|**Indonesia \(Jakarta\)**|```
wget http://logtail-release-ap-southeast-5.oss-ap-southeast-5.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-5-acceleration
``` |
|**Japan \(Tokyo\)**|```
wget http://logtail-release-ap-northeast-1.oss-ap-northeast-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-northeast-1-acceleration
``` |
|**Germany \(Frankfurt\)**|```
wget http://logtail-release-eu-central-1.oss-eu-central-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-central-1-acceleration
``` |
|**UAE \(Dubai\)**|```
wget http://logtail-release-me-east-1.oss-me-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install me-east-1-acceleration
``` |
|**India \(Mumbai\)**|```
wget http://logtail-release-ap-south-1.oss-ap-south-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-south-1-acceleration
``` |
|**UK \(London\)**|```
wget http://logtail-release-eu-west-1.oss-eu-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-west-1-acceleration
``` |
|**Russia \(Moscow\)**|```
wget http://logtail-release-rus-west-1.oss-rus-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install rus-west-1-acceleration
``` |

## View the version of Logtail

Open the /usr/local/ilogtail/app\_info.json file in the installation directory. The `logtail_version` field shows the version of Logtail.

```
$cat /usr/local/ilogtail/app_info.json
{
   "UUID" : "0DF18E97-0F2D-486F-B77F-*********",
   "hostname" : "david*******",
   "instance_id" : "F4FAFADA-F1D7-11E7-846C-00163E30349E_*********_1515129548",
   "ip" : "**********",
   "logtail_version" : "0.16.0",
   "os" : "Linux; 2.6.32-220.23.2.ali1113.el5.x86_64; #1 SMP Thu Jul 4 20:09:15 CST 2013; x86_64",
   "update_time" : "2018-01-05 13:19:08"
}
```

## Upgrade Logtail

You can use the Logtail installation script \(logtail.sh\) to upgrade Logtail. The installation script selects an upgrade method based on the configurations of the existing Logtail.

**Note:** Logtail is temporarily stopped during the upgrade, but no log data loss occurs. After Logtail is upgraded, the configuration files and checkpoint files are retained. Other original files are overwritten.

1.  Run the following command to upgrade Logtail:

    ```
    wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh
    sudo ./logtail.sh upgrade
    ```

2.  Check the upgrade result.

    The following sample result shows a successful Logtail upgrade.

    ```
    Stop logtail successfully.
    ilogtail is running
    Upgrade logtail success
    {
       "UUID" : "***",
       "hostname" : "***",
       "instance_id" : "***",
       "ip" : "***",
       "logtail_version" : "0.16.11",
       "os" : "Linux; 3.10.0-693.2.2.el7.x86_64; #1 SMP Tue Sep 12 22:26:13 UTC 2017; x86_64",
       "update_time" : "2018-08-29 15:01:36"
    }
    ```


## Start and stop Logtail

-   Start Logtail

    Run the following command as a root user:

    ```
    /etc/init.d/ilogtaild start
    ```

-   Stop Logtail

    Run the following command as a root user:

    ```
    /etc/init.d/ilogtaild stop
    ```


## Uninstall Logtail

Run the following command to download the Logtail installation script \(logtail.sh\) and uninstall Logtail:

```
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh
chmod 755 logtail.sh; ./logtail.sh uninstall
```

