# Linux {#concept_u5y_3lv_vdb .concept}

Logtail客户端是日志服务提供的日志采集客户端，请参考本文档，在Linux服务器上安装Logtail客户端。

## 支持的系统 {#section_tjc_wlv_vdb .section}

支持如下版本的Linux x86-64（64位）服务器：

-   Aliyun Linux
-   Ubuntu
-   Debian
-   CentOS
-   OpenSUSE

## 前提条件 {#section_t52_5zm_1fb .section}

1.  已拥有一台及以上的服务器。
2.  已根据服务器类型和所在Region，确定日志采集流量的网络类型。详细说明请参考[选择网络](intl.zh-CN/用户指南/Logtail采集/选择网络.md)。

    ![](images/12057_zh-CN.png "选择网络")


## 注意事项 {#section_wgl_xvn_1fb .section}

-   Logtail采用覆盖安装模式，若您之前已安装过Logtail，那么安装器会先执行卸载、删除`/usr/local/ilogtail` 目录后再重新安装。安装后默认启动Logtail并注册开机启动。

-   Docker和Kubernetes安装中的`${your_region_name}` 即为[安装参数](#)中的参数，请直接拷贝。

-   如果安装失败，单击[工单系统](https://selfservice.console.aliyun.com/ticket/category/sls/today)提交工单。


## 安装方式 {#section_h2x_fmv_vdb .section}

[选择网络](intl.zh-CN/用户指南/Logtail采集/选择网络.md)后，请根据您的网络类型选择对应的安装命令。

-   [阿里云内网（经典网络、VPC）](#)
-   [公网](#)
-   [全球加速](#)

执行安装命令之前，您需要将安装命令中的$\{your\_region\_name\}替换为您的区域名称。各区域的安装参数如下，您也可以直接拷贝执行对应区域和网络类型的安装命令。

|区域|安装参数|区域|安装参数|
|:-|:---|:-|:---|
|**华东 1（杭州）**|cn-hangzhou|**亚太东南 1（新加坡）**|ap-southeast-1|
|**华东 2（上海）**|cn-shanghai|**亚太东南 2（悉尼）**|ap-southeast-2|
|**华北 1（青岛）**|cn-qingdao|**亚太东南 3（吉隆坡）**|ap-southeast-3|
|**华北 2（北京）**|cn-beijing|**亚太东南 5（雅加达）**|ap-southeast-5|
|**华北 3（张家口）**|cn-zhangjiakou|**亚太南部 1（孟买）**|ap-south-1|
|**华北 5（呼和浩特）**|cn-huhehaote|**亚太东北 1（日本）**|ap-northeast-1|
|**华南 1（深圳）**|cn-shenzhen|**欧洲中部 1（法兰克福）**|eu-central-1|
|**西南 1（成都）**|cn-chengdu|**中东东部 1（迪拜）**|me-east-1|
|**香港**|cn-hongkong|**-**|**-**|
|**美国西部 1（硅谷）**|us-west-1|**-**|**-**|
|**美国东部 1（弗吉尼亚）**|us-east-1|**-**|**-**|

## 阿里云内网（经典网络、VPC） {#section_inb_fbn_1fb .section}

阿里云内网为千兆共享网络，日志数据通过阿里云内网传输比公网传输更快速、稳定，且不消耗公网带宽。

可以使用阿里云内网的场景：

-   服务器为阿里云ECS。
-   ECS和日志服务Project位于同一区域。

执行安装命令时，需要根据区域选择安装参数，您可以选择**自动选择安装参数**和**手动安装**两种方式。

-   **自动选择安装参数**

    如果您无法确定ECS所在的区域，可以使用Logtail安装器的auto参数进行安装，当指定该参数后，Logtail 安装器会通过服务器获取您的[实例元数据](../../../../intl.zh-CN/用户指南/实例/实例自定义数据和元数据/实例元数据.md)，自动确定ECS所在区域。

    1.  通过公网下载Logtail 安装器。该步骤涉及访问公网，会消耗公网流量，约10KB左右。

        ```
        wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh;chmod 755 logtail.sh
        ```

    2.  使用auto参数进行安装。该步骤会自动下载对应区域的安装程序，不消耗公网流量。

        ```
        ./logtail.sh install auto
        ```

-   **手动安装**

    您也可以选择手动安装Logtail。通过内网下载Logtail安装器，不消耗公网流量。

    1.  **根据日志服务Project所在区域选择安装参数。**

        安装命令中的`${your_region_name}`表示日志服务Project所在区域，根据[安装参数](#)选择正确参数，如华东一区域的安装参数为`cn-hangzhou`。

    2.  **替换参数后执行安装命令。**

        替换参数`${your_region_name}`后，执行安装命令。

        ```
        wget http://logtail-release-$\{your\_region\_name\}.oss-$\{your\_region\_name\}-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install $\{your\_region\_name\}
        ```

        **您也可以根据日志服务Project所在的区域直接执行下述对应的命令进行安装**：

        |日志服务Project所在的区域|安装命令|
        |:---------------|:---|
        |**华东 1（杭州）**|         ```
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hangzhou
        ```

 |
        |**华东 2（上海）**|         ```
wget http://logtail-release-cn-shanghai.oss-cn-shanghai-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shanghai
        ```

 |
        |**华北 1（青岛）**|         ```
wget http://logtail-release-cn-qingdao.oss-cn-qingdao-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-qingdao
        ```

 |
        |**华北 2（北京）**|         ```
wget http://logtail-release-cn-beijing.oss-cn-beijing-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-beijing
        ```

 |
        |**华北 3 （张家口）**|         ```
wget http://logtail-release-cn-zhangjiakou.oss-cn-zhangjiakou-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-zhangjiakou
        ```

 |
        |**华北 5 （呼和浩特）**|         ```
wget http://logtail-release-cn-huhehaote.oss-cn-huhehaote-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-huhehaote
        ```

 |
        |**华南 1（深圳）**|         ```
wget http://logtail-release-cn-shenzhen.oss-cn-shenzhen-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shenzhen
        ```

 |
        |**西南 1（成都）**|         ```
wget http://logtail-release-cn-chengdu.oss-cn-chengdu-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-chengdu
        ```

 |
        |**香港**|         ```
wget http://logtail-release-cn-hongkong.oss-cn-hongkong-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hongkong
        ```

 |
        |**美国西部 1 （硅谷）**|         ```
wget http://logtail-release-us-west-1.oss-us-west-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-west-1
        ```

 |
        |**美国东部 1（弗吉尼亚）**|         ```
wget http://logtail-release-us-east-1.oss-us-east-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-east-1
        ```

 |
        |**亚太东南 1 （新加坡）**|         ```
wget http://logtail-release-ap-southeast-1.oss-ap-southeast-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-1
        ```

 |
        |**亚太东南 2 （悉尼）**|         ```
wget http://logtail-release-ap-southeast-2.oss-ap-southeast-2-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-2
        ```

 |
        |**亚太东南 3 （吉隆坡）**|         ```
wget http://logtail-release-ap-southeast-3.oss-ap-southeast-3-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-3
        ```

 |
        |**亚太东南 5（雅加达）**|         ```
wget http://logtail-release-ap-southeast-5.oss-ap-southeast-5-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-5
        ```

 |
        |**亚太东北 1 （日本）**|         ```
wget http://logtail-release-ap-northeast-1.oss-ap-northeast-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-northeast-1
        ```

 |
        |**亚太南部 1（孟买）**|         ```
wget http://logtail-release-ap-south-1.oss-ap-south-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-south-1
        ```

 |
        |**欧洲中部 1 （法兰克福）**|         ```
wget http://logtail-release-eu-central-1.oss-eu-central-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-central-1
        ```

 |
        |**中东东部 1 （迪拜）**|         ```
wget http://logtail-release-me-east-1.oss-me-east-1-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install me-east-1
        ```

 |


## 公网 {#section_lhn_hbn_1fb .section}

数据通过公网写入日志服务，占用公网带宽。**适用于自建IDC和其他云厂商服务器**。

**说明：** 日志服务无法获取ECS之外服务器的属主信息，请在安装Logtail后手动配置用户标识（AliUid，参见 [为非本账号ECS、自建IDC配置AliUid](intl.zh-CN/用户指南/Logtail采集/机器组/为非本账号ECS、自建IDC配置AliUid.md)），否则 Logtail心跳异常、无法收集日志。

1.  **根据日志服务Project所在区域选择安装参数。**

    安装命令中的`${your_region_name}`表示日志服务Project所在区域，根据[安装参数](#)选择正确参数，如华东一区域的安装参数为`cn-hangzhou`。

2.  **替换参数后执行安装命令。**

    替换参数`${your_region_name}`后，执行安装命令。

    ```
    wget http://logtail-release-$\{your\_region\_name\}.oss-$\{your\_region\_name\}.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install $\{your\_region\_name\}-internet
    ```

    **您也可以根据日志服务Project所在的区域直接执行下述对应的命令进行安装**：

    |日志服务Project所在的区域|安装命令|
    |:---------------|:---|
    |**华东 1（杭州）**|     ```
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hangzhou-internet
    ```

 |
    |**华东 2（上海）**|     ```
wget http://logtail-release-cn-shanghai.oss-cn-shanghai.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shanghai-internet
    ```

 |
    |**华北 1（青岛）**|     ```
wget http://logtail-release-cn-qingdao.oss-cn-qingdao.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-qingdao-internet
    ```

 |
    |**华北 2（北京）**|     ```
wget http://logtail-release-cn-beijing.oss-cn-beijing.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-beijing-internet
    ```

 |
    |**华北 3 （张家口）**|     ```
wget http://logtail-release-cn-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-zhangjiakou-internet
    ```

 |
    |**华北 5 （呼和浩特）**|     ```
wget http://logtail-release-cn-huhehaote.oss-cn-huhehaote.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-huhehaote-internet
    ```

 |
    |**华南 1（深圳）**|     ```
wget http://logtail-release-cn-shenzhen.oss-cn-shenzhen.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shenzhen-internet
    ```

 |
    |**西南 1 （成都）**|     ```
wget http://logtail-release-cn-chengdu.oss-cn-chengdu.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-chengdu-internet
    ```

 |
    |**香港**|     ```
wget http://logtail-release-cn-hongkong.oss-cn-hongkong.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hongkong-internet
    ```

 |
    |**美国西部 1 （硅谷）**|     ```
wget http://logtail-release-us-west-1.oss-us-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-west-1-internet
    ```

 |
    |**美国东部 1 （弗吉尼亚）**|     ```
wget http://logtail-release-us-east-1.oss-us-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-east-1-internet
    ```

 |
    |**亚太东南 1 （新加坡）**|     ```
wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; sh logtail.sh install ap-southeast-1-internet
    ```

 |
    |**亚太东南 2 （悉尼）**|     ```
wget http://logtail-release-ap-southeast-2.oss-ap-southeast-2.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-2-internet
    ```

 |
    |**亚太东南 3 （吉隆坡）**|     ```
wget http://logtail-release-ap-southeast-3.oss-ap-southeast-3.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-3-internet
    ```

 |
    |**亚太东南 5 （雅加达）**|     ```
wget http://logtail-release-ap-southeast-5.oss-ap-southeast-5.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-5-internet
    ```

 |
    |**亚太东北 1 （日本）**|     ```
wget http://logtail-release-ap-northeast-1.oss-ap-northeast-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-northeast-1-internet
    ```

 |
    |**欧洲中部 1 （法兰克福）**|     ```
wget http://logtail-release-eu-central-1.oss-eu-central-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-central-1-internet
    ```

 |
    |**中东东部 1 （迪拜）**|     ```
wget http://logtail-release-me-east-1.oss-me-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install me-east-1-internet
    ```

 |
    |**亚太南部 1 （孟买）**|     ```
wget http://logtail-release-ap-south-1.oss-ap-south-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-south-1-internet
    ```

 |


## 全球加速 {#section_xpy_tvs_s2b .section}

如果您的服务器分布在海外各地的自建机房、或者来自海外云厂商，使用公网传输数据可能会出现网络延迟高、传输不稳定等问题，可以通过[全球加速](intl.zh-CN/用户指南/数据采集/采集加速/简介.md)传输数据。[全球加速](intl.zh-CN/用户指南/数据采集/采集加速/简介.md)利用阿里云CDN边缘节点进行日志采集加速，相对公网采集在网络延迟、稳定性上具有很大优势。

1.  **根据日志服务Project所在区域选择安装参数。**

    安装命令中的$\{your\_region\_name\}表示日志服务Project所在区域，根据[安装参数](#)选择正确参数，如华东一区域的安装参数为`cn-hangzhou`。

2.  **替换参数后执行安装命令。**

    替换参数`${your_region_name}`后，执行安装命令。

    ```
    wget http://logtail-release-$\{your\_region\_name\}.oss-$\{your\_region\_name\}.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install $\{your\_region\_name\}-acceleration
    ```

    **您也可以根据日志服务Project所在的区域直接执行下述对应的命令进行安装**：

    | | |
    |:-|:-|
    |**华北 2（北京）**|     ```
wget http://logtail-release-cn-beijing.oss-cn-beijing.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-beijing-acceleration
    ```

 |
    |**华北 1（青岛）**|     ```
wget http://logtail-release-cn-qingdao.oss-cn-qingdao.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-qingdao-acceleration
    ```

 |
    |**华东 1（杭州）**|     ```
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hangzhou-acceleration
    ```

 |
    |**华东 2（上海）**|     ```
wget http://logtail-release-cn-shanghai.oss-cn-shanghai.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shanghai-acceleration
    ```

 |
    |**华南 1（深圳）**|     ```
wget http://logtail-release-cn-shenzhen.oss-cn-shenzhen.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-shenzhen-acceleration
    ```

 |
    |**华北 3 （张家口）**|     ```
wget http://logtail-release-cn-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-zhangjiakou-acceleration
    ```

 |
    |**华北 5 （呼和浩特）**|     ```
wget http://logtail-release-cn-huhehaote.oss-cn-huhehaote.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-huhehaote-acceleration
    ```

 |
    |**西南 1 （成都）**|     ```
wget http://logtail-release-cn-chengdu.oss-cn-chengdu.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-chengdu-acceleration
    ```

 |
    |**香港**|     ```
wget http://logtail-release-cn-hongkong.oss-cn-hongkong.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install cn-hongkong-acceleration
    ```

 |
    |**美国西部 1 （硅谷）**|     ```
wget http://logtail-release-us-west-1.oss-us-west-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-west-1-acceleration
    ```

 |
    |**美国东部 1 （弗吉尼亚）**|     ```
wget http://logtail-release-us-east-1.oss-us-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install us-east-1-acceleration
    ```

 |
    |**亚太东南 1 （新加坡）**|     ```
wget http://logtail-release-ap-southeast-1.oss-ap-southeast-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-1-acceleration
    ```

 |
    |**亚太东南 2 （悉尼）**|     ```
wget http://logtail-release-ap-southeast-2.oss-ap-southeast-2.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-2-acceleration
    ```

 |
    |**亚太东南 3 （吉隆坡）**|     ```
wget http://logtail-release-ap-southeast-3.oss-ap-southeast-3.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-3-acceleration
    ```

 |
    |**亚太东南 5 （雅加达）**|     ```
wget http://logtail-release-ap-southeast-5.oss-ap-southeast-5.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-southeast-5-acceleration
    ```

 |
    |**亚太东北 1 （日本）**|     ```
wget http://logtail-release-ap-northeast-1.oss-ap-northeast-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-northeast-1-acceleration
    ```

 |
    |**欧洲中部 1 （法兰克福）**|     ```
wget http://logtail-release-eu-central-1.oss-eu-central-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install eu-central-1-acceleration
    ```

 |
    |**中东东部 1 （迪拜）**|     ```
wget http://logtail-release-me-east-1.oss-me-east-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install me-east-1-acceleration
    ```

 |
    |**亚太南部 1 （孟买）**|     ```
wget http://logtail-release-ap-south-1.oss-ap-south-1.aliyuncs.com/linux64/logtail.sh -O logtail.sh; chmod 755 logtail.sh; ./logtail.sh install ap-south-1-acceleration
    ```

 |


## 查看Logtail版本 {#section_t5v_xyq_zdb .section}

Logtail会将版本信息记录在`/usr/local/ilogtail/app_info.json`中的`logtail_version`字段，例如：

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

## 升级Logtail {#section_jcz_xmv_vdb .section}

升级Logtail和安装的操作步骤一致，如果您已经安装Logtail，再次执行安装命令时将自动卸载并安装最新版本的Logtail。

## 手动启动和停止Logtail {#section_fvl_zmv_vdb .section}

-   启动

    以管理员身份执行：

    ```
    /etc/init.d/ilogtaild start
    ```

-   停止

    以管理员身份执行：

    ```
    /etc/init.d/ilogtaild stop
    ```


## 卸载Logtail {#section_c1m_1nv_vdb .section}

执行以下命令，下载安装器**logtail.sh**并卸载Logtail。

```
wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh -O logtail.sh
chmod 755 logtail.sh; ./logtail.sh uninstall
```

