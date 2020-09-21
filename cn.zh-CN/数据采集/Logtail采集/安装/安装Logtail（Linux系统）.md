# 安装Logtail（Linux系统）

本文介绍如何在Linux服务器上安装、升级及卸载Logtail等操作。

## 前提条件

-   已拥有一台及以上的服务器。
-   已根据服务器类型和所在地域，确定采集日志时所需的网络类型，详情请参见[选择网络](/cn.zh-CN/数据采集/Logtail采集/选择网络.md)。

## 支持的系统

支持如下版本的Linux x86-64（64位）服务器：

-   Aliyun Linux 2
-   RedHat Enterprise 6、7、8
-   CentOS Linux 6、7、8
-   Debian GNU/Linux 8、9、10
-   Ubuntu 14.04、16.04、18.04、20.04
-   SUSE Linux Enterprise Server 11、12、15
-   OpenSUSE 15.1、15.2、42.3
-   其他基于glibc 2.5及以上版本的Linux操作系统

## 注意事项

-   Logtail采用覆盖安装模式，如果您已安装过Logtail，那么重新安装Logtail时会先执行卸载、删除/usr/local/ilogtail目录操作。安装后默认启动Logtail并注册开机启动。
-   如果安装失败，请提[工单](https://selfservice.console.aliyun.com/ticket/category/sls/today)。
-   安装Logtail后，如果ECS的网络由经典网络切换至VPC，则需要更新logtail配置，详情请参见[经典网络切换为VPC后，是否需要更新Logtail配置]()。

## 安装方式

请根据您的网络类型选择对应的安装命令。

-   [阿里云内网（经典网络、VPC）](#section_inb_fbn_1fb)
-   [公网](#section_lhn_hbn_1fb)
-   [全球加速](#section_xpy_tvs_s2b)
-   [ECS金融云](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)

执行安装命令之前，您需要根据Project所在地域替换安装命令中的$\{your\_region\_name\}参数，各地域对应的$\{your\_region\_name\}参数如下所示。

|地域|$\{your\_region\_name\}|地域|$\{your\_region\_name\}|
|:-|:----------------------|:-|:----------------------|
|**华东1（杭州）**|cn-hangzhou|**美国（弗吉尼亚）**|us-east-1|
|**华东2（上海）**|cn-shanghai|**新加坡**|ap-southeast-1|
|**华北1（青岛）**|cn-qingdao|**澳大利亚（悉尼）**|ap-southeast-2|
|**华北2（北京）**|cn-beijing|**马来西亚（吉隆坡）**|ap-southeast-3|
|**华北3（张家口）**|cn-zhangjiakou|**印度尼西亚（雅加达）**|ap-southeast-5|
|**华北5（呼和浩特）**|cn-huhehaote|**印度（孟买）**|ap-south-1|
|**华北6（乌兰察布）**|cn-wulanchabu|**日本（东京）**|ap-northeast-1|
|**华南1（深圳）**|cn-shenzhen|**德国（法兰克福）**|eu-central-1|
|**华南2（河源）**|cn-heyuan|**阿联酋（迪拜）**|me-east-1|
|**华南3（广州）**|cn-guangzhou|**英国（伦敦）**|eu-west-1|
|**西南1（成都）**|cn-chengdu|**华东1金融云（杭州）**|cn-hangzhou-finance|
|**中国（香港）**|cn-hongkong|**华东2金融云（上海）**|cn-shanghai-finance|
|**俄罗斯（莫斯科）**|rus-west-1|**华南1金融云（深圳）**|cn-shenzhen-finance|
|**美国（硅谷）**|us-west-1|无|无|

## 阿里云内网（经典网络、VPC）

-   如果您无法确定ECS所在地域，可使用Logtail安装脚本中的auto参数进行安装。

    在安装命令中指定auto参数后，Logtail安装脚本会通过ECS获取您的实例元数据，自动确定ECS所在地域，实例元数据介绍请参见 [实例元数据概述](/cn.zh-CN/实例/管理实例/使用实例元数据/实例元数据概述.md)。

    1.  通过公网下载Logtail安装脚本。

        此下载消耗公网流量，约10 KB。

        ```
        wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh;chmod 755 logtail.sh
        ```

    2.  使用auto参数安装logtail。

        此步骤自动下载对应地域的安装程序，不消耗公网流量。

        ```
        ./logtail.sh install auto
        ```

-   如果您已确定ECS所在地域，请根据地域选择安装命令。

    通过内网下载Logtail安装脚本，手动安装Logtail，不消耗公网流量。

    1.  根据日志服务Project所在地域，获取对应的`${your_region_name}`。

        各个地域对应的`${your_region_name}`请参见[安装参数](#table_eyz_pmv_vdb)，例如**华东 1（杭州）**对应的`${your_region_name}`为`cn-hangzhou`。

    2.  替换`${your_region_name}`后，执行安装命令。

        ```
        wget http://logtail-release-$\{your\_region\_name\}.oss-$\{your\_region\_name\}-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install $\{your\_region\_name\}
        ```

    您也可以根据日志服务Project所在的地域执行对应的命令进行安装。

    |Project所在地域|安装命令|
    |:----------|:---|
    |**华东1（杭州）**|    ```
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hangzhou
    ``` |
    |**华东2（上海）**|    ```
wget http://logtail-release-cn-shanghai.oss-cn-shanghai-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shanghai
    ``` |
    |**华北1（青岛）**|    ```
wget http://logtail-release-cn-qingdao.oss-cn-qingdao-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-qingdao
    ``` |
    |**华北2（北京）**|    ```
wget http://logtail-release-cn-beijing.oss-cn-beijing-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-beijing
    ``` |
    |**华北3（张家口）**|    ```
wget http://logtail-release-cn-zhangjiakou.oss-cn-zhangjiakou-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-zhangjiakou
    ``` |
    |**华北5（呼和浩特）**|    ```
wget http://logtail-release-cn-huhehaote.oss-cn-huhehaote-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-huhehaote
    ``` |
    |**华北6（乌兰察布）**|    ```
wget http://logtail-release-cn-wulanchabu.oss-cn-wulanchabu-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-wulanchabu
    ``` |
    |**华南1（深圳）**|    ```
wget http://logtail-release-cn-shenzhen.oss-cn-shenzhen-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shenzhen
    ``` |
    |**华南2（河源）**|    ```
wget http://logtail-release-cn-heyuan.oss-cn-heyuan-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-heyuan
    ``` |
    |**华南3（广州）**|    ```
wget http://logtail-release-cn-guangzhou.oss-cn-guangzhou-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-guangzhou
    ``` |
    |**西南1（成都）**|    ```
wget http://logtail-release-cn-chengdu.oss-cn-chengdu-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-chengdu
    ``` |
    |**中国（香港）**|    ```
wget http://logtail-release-cn-hongkong.oss-cn-hongkong-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hongkong
    ``` |
    |**美国（硅谷）**|    ```
wget http://logtail-release-us-west-1.oss-us-west-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-west-1
    ``` |
    |**美国（弗吉尼亚）**|    ```
wget http://logtail-release-us-east-1.oss-us-east-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-east-1
    ``` |
    |**新加坡**|    ```
wget http://logtail-release-ap-southeast-1.oss-ap-southeast-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-1
    ``` |
    |**澳大利亚（悉尼）**|    ```
wget http://logtail-release-ap-southeast-2.oss-ap-southeast-2-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-2
    ``` |
    |**马来西亚（吉隆坡）**|    ```
wget http://logtail-release-ap-southeast-3.oss-ap-southeast-3-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-3
    ``` |
    |**印度尼西亚（雅加达）**|    ```
wget http://logtail-release-ap-southeast-5.oss-ap-southeast-5-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-5
    ``` |
    |**日本（东京）**|    ```
wget http://logtail-release-ap-northeast-1.oss-ap-northeast-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-northeast-1
    ``` |
    |**印度（孟买）**|    ```
wget http://logtail-release-ap-south-1.oss-ap-south-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-south-1
    ``` |
    |**德国（法兰克福）**|    ```
wget http://logtail-release-eu-central-1.oss-eu-central-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-central-1
    ``` |
    |**阿联酋（迪拜）**|    ```
wget http://logtail-release-me-east-1.oss-me-east-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install me-east-1
    ``` |
    |**英国（伦敦）**|    ```
wget http://logtail-release-eu-west-1.oss-eu-west-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-west-1
    ``` |
    |**俄罗斯（莫斯科）**|    ```
wget http://logtail-release-rus-west-1.oss-rus-west-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install rus-west-1
    ``` |


## 公网

1.  根据日志服务Project所在地域，获取对应的`${your_region_name}`。

    各个地域对应的`${your_region_name}`请参见[安装参数](#table_eyz_pmv_vdb)，例如**华东 1（杭州）**对应的`${your_region_name}`为`cn-hangzhou`。

2.  替换`${your_region_name}`后，执行安装命令。

    ```
    wget http://logtail-release-$\{your\_region\_name\}.oss-$\{your\_region\_name\}.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install $\{your\_region\_name\}-internet
    ```


您也可以根据日志服务Project所在地域直接执行对应的命令进行安装。

|Project所在的地域|安装命令|
|:-----------|:---|
|**华东1（杭州）**|```
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hangzhou-internet
``` |
|**华东2（上海）**|```
wget http://logtail-release-cn-shanghai.oss-cn-shanghai.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shanghai-internet
``` |
|**华北1（青岛）**|```
wget http://logtail-release-cn-qingdao.oss-cn-qingdao.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-qingdao-internet
``` |
|**华北2（北京）**|```
wget http://logtail-release-cn-beijing.oss-cn-beijing.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-beijing-internet
``` |
|**华北3（张家口）**|```
wget http://logtail-release-cn-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-zhangjiakou-internet
``` |
|**华北5（呼和浩特）**|```
wget http://logtail-release-cn-huhehaote.oss-cn-huhehaote.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-huhehaote-internet
``` |
|**华北6（乌兰察布）**|```
wget http://logtail-release-cn-wulanchabu.oss-cn-wulanchabu.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-wulanchabu-internet
``` |
|**华南1（深圳）**|```
wget http://logtail-release-cn-shenzhen.oss-cn-shenzhen.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shenzhen-internet
``` |
|**华南2（河源）**|```
wget http://logtail-release-cn-heyuan.oss-cn-heyuan.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-heyuan-internet
``` |
|**华南3（广州）**|```
wget http://logtail-release-cn-guangzhou.oss-cn-guangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-guangzhou-internet
``` |
|**西南1（成都）**|```
wget http://logtail-release-cn-chengdu.oss-cn-chengdu.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-chengdu-internet
``` |
|**中国（香港）**|```
wget http://logtail-release-cn-hongkong.oss-cn-hongkong.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hongkong-internet
``` |
|**美国（硅谷）**|```
wget http://logtail-release-us-west-1.oss-us-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-west-1-internet
``` |
|**美国（弗吉尼亚）**|```
wget http://logtail-release-us-east-1.oss-us-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-east-1-internet
``` |
|**新加坡**|```
wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; sh logtail.sh install ap-southeast-1-internet
``` |
|**澳大利亚（悉尼）**|```
wget http://logtail-release-ap-southeast-2.oss-ap-southeast-2.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-2-internet
``` |
|**马来西亚（吉隆坡）**|```
wget http://logtail-release-ap-southeast-3.oss-ap-southeast-3.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-3-internet
``` |
|**印度尼西亚（雅加达）**|```
wget http://logtail-release-ap-southeast-5.oss-ap-southeast-5.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-5-internet
``` |
|**日本（东京）**|```
wget http://logtail-release-ap-northeast-1.oss-ap-northeast-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-northeast-1-internet
``` |
|**德国（法兰克福）**|```
wget http://logtail-release-eu-central-1.oss-eu-central-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-central-1-internet
``` |
|**阿联酋（迪拜）**|```
wget http://logtail-release-me-east-1.oss-me-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install me-east-1-internet
``` |
|**印度（孟买）**|```
wget http://logtail-release-ap-south-1.oss-ap-south-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-south-1-internet
``` |
|**英国（伦敦）**|```
wget http://logtail-release-eu-west-1.oss-eu-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-west-1-internet
``` |
|**俄罗斯（莫斯科）**|```
wget http://logtail-release-rus-west-1.oss-rus-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install rus-west-1-internet
``` |

## 全球加速

1.  根据日志服务Project所在地域选择安装参数。

    各个地域对应的`${your_region_name}`请参见[安装参数](#table_eyz_pmv_vdb)，例如**华东 1（杭州）**对应的`${your_region_name}`为`cn-hangzhou`。

2.  替换`${your_region_name}`后，执行安装命令。

    ```
    wget http://logtail-release-$\{your\_region\_name\}.oss-$\{your\_region\_name\}.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install $\{your\_region\_name\}-acceleration
    ```


您也可以根据日志服务Project所在地域执行对应的命令进行安装。

|Project所在的地域|安装命令|
|:-----------|:---|
|**华北2（北京）**|```
wget http://logtail-release-cn-beijing.oss-cn-beijing.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-beijing-acceleration
``` |
|**华北1（青岛）**|```
wget http://logtail-release-cn-qingdao.oss-cn-qingdao.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-qingdao-acceleration
``` |
|**华东1（杭州）**|```
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hangzhou-acceleration
``` |
|**华东2（上海）**|```
wget http://logtail-release-cn-shanghai.oss-cn-shanghai.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shanghai-acceleration
``` |
|**华南1（深圳）**|```
wget http://logtail-release-cn-shenzhen.oss-cn-shenzhen.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shenzhen-acceleration
``` |
|**华南2（河源）**|```
wget http://logtail-release-cn-heyuan.oss-cn-heyuan.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-heyuan-acceleration
``` |
|**华南3（广州）**|```
wget http://logtail-release-cn-guangzhou.oss-cn-guangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-guangzhou-acceleration
``` |
|**华北3（张家口）**|```
wget http://logtail-release-cn-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-zhangjiakou-acceleration
``` |
|**华北5（呼和浩特）**|```
wget http://logtail-release-cn-huhehaote.oss-cn-huhehaote.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-huhehaote-acceleration
``` |
|**华北6（乌兰察布）**|```
wget http://logtail-release-cn-wulanchabu.oss-cn-wulanchabu.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-wulanchabu-acceleration
``` |
|**西南1（成都）**|```
wget http://logtail-release-cn-chengdu.oss-cn-chengdu.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-chengdu-acceleration
``` |
|**中国（香港）**|```
wget http://logtail-release-cn-hongkong.oss-cn-hongkong.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hongkong-acceleration
``` |
|**美国（硅谷）**|```
wget http://logtail-release-us-west-1.oss-us-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-west-1-acceleration
``` |
|**美国（弗吉尼亚）**|```
wget http://logtail-release-us-east-1.oss-us-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-east-1-acceleration
``` |
|**新加坡**|```
wget http://logtail-release-ap-southeast-1.oss-ap-southeast-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-1-acceleration
``` |
|**澳大利亚（悉尼）**|```
wget http://logtail-release-ap-southeast-2.oss-ap-southeast-2.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-2-acceleration
``` |
|**马来西亚（吉隆坡）**|```
wget http://logtail-release-ap-southeast-3.oss-ap-southeast-3.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-3-acceleration
``` |
|**印度尼西亚（雅加达）**|```
wget http://logtail-release-ap-southeast-5.oss-ap-southeast-5.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-5-acceleration
``` |
|**日本（东京）**|```
wget http://logtail-release-ap-northeast-1.oss-ap-northeast-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-northeast-1-acceleration
``` |
|**德国（法兰克福）**|```
wget http://logtail-release-eu-central-1.oss-eu-central-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-central-1-acceleration
``` |
|**阿联酋（迪拜）**|```
wget http://logtail-release-me-east-1.oss-me-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install me-east-1-acceleration
``` |
|**印度（孟买）**|```
wget http://logtail-release-ap-south-1.oss-ap-south-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-south-1-acceleration
``` |
|**英国（伦敦）**|```
wget http://logtail-release-eu-west-1.oss-eu-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-west-1-acceleration
``` |
|**俄罗斯（莫斯科）**|```
wget http://logtail-release-rus-west-1.oss-rus-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install rus-west-1-acceleration
``` |

## ECS金融云

请根据Project所在的地域执行对应的安装命令。

|Project所在的地域|安装命令|
|:-----------|:---|
|**华东1金融云（杭州）**|```
wget http://logtail-release-cn-hangzhou-finance-1.oss-cn-hzfinance-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hangzhou-finance
``` |
|**华东1金融云（杭州公网）**|```
wget http://logtail-release-cn-hangzhou-finance-1.oss-cn-hzfinance.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hangzhou-finance-internet
``` |
|**华东2金融云（上海）**|```
wget http://logtail-release-cn-shanghai-finance-1.oss-cn-shanghai-finance-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shanghai-finance
``` |
|**华东2金融云（上海公网）**|```
wget http://logtail-release-cn-shanghai-finance-1.oss-cn-shanghai-finance-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shanghai-finance-internet
``` |
|**华南1金融云（深圳）**|```
wget http://logtail-release-sz-finance.oss-cn-shenzhen-finance-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shenzhen-finance
``` |
|**华南1金融云（深圳公网）**|```
wget http://logtail-release-sz-finance.oss-cn-shenzhen-finance-1.aliyuncs.com/linux64/logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shenzhen-finance-internet
``` |

## 查看Logtail版本

Logtail会将版本信息记录在/usr/local/ilogtail/app\_info.json文件中的`logtail_version`字段。

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

## 升级Logtail

您可以通过Logtail安装脚本（logtail.sh）升级Logtail，Logtail安装脚本会根据已经安装的Logtail配置信息自动选择合适的方式进行升级。

**说明：** 升级过程中会短暂停止Logtail。升级只覆盖必要的文件，配置文件以及Checkpoint文件将会被保留，升级期间日志不会丢失。

1.  执行以下命令升级Logtail。

    ```
    wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh
    sudo ./logtail.sh upgrade
    ```

2.  确认升级结果。

    显示类似信息表示升级成功。

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


## 手动启动和停止Logtail

-   启动

    以root用户执行如下命令。

    ```
    /etc/init.d/ilogtaild start
    ```

-   停止

    以root用户执行如下命令

    ```
    /etc/init.d/ilogtaild stop
    ```


## 卸载Logtail

执行以下命令，下载logtail.sh并卸载Logtail。

```
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh
chmod 755 logtail.sh; ./logtail.sh uninstall
```

