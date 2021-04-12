# 通过DaemonSet-控制台方式采集Kubernetes文件

本文介绍如何在控制台上创建Logtail配置，并以DaemonSet方式采集Kubernetes文件。

已安装alibaba-log-controller Helm，详情请参见[安装Logtail日志组件](/intl.zh-CN/数据采集/Logtail采集/采集容器日志/安装Logtail日志组件.md)。

## 功能特点

Logtail支持采集容器内产生的文本日志，并附加容器的相关元数据信息一起上传到日志服务。相对基础的日志文件采集，Kubernetes文件采集具备以下功能特点。

-   只需配置容器内的日志路径，无需关心该路径到宿主机的映射。
-   支持通过Label指定采集的容器。
-   支持通过Label排除特定容器。
-   支持通过环境变量指定采集的容器。
-   支持通过环境变量指定排除的容器。
-   支持多行日志（例如java stack日志）。
-   支持Docker容器数据自动打标签。
-   支持Kubernetes容器数据自动打标签。

**说明：**

-   本文的Label为Docker inspect中的Label，并不是Kubernetes配置中的Label。
-   本文中的环境变量为容器启动中配置的环境变量信息。

## 限制说明

-   采集停止策略：当容器被停止后，Logtail监听到容器`die`的事件后会停止该容器日志的采集，若此时采集出现延迟，则可能丢失停止前的部分日志。
-   Docker存储驱动限制：目前只支持overlay、overlay2，其他存储驱动需将日志所在目录挂载到宿主机上。
-   不支持采集软链接：目前Logtail无法访问业务容器的软链接，请按真实路径配置采集目录。

## 创建采集配置

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在接入数据区域，单击**Kubernetes-文件**。

3.  选择目标Project和Logstore，单击**下一步**。

4.  创建机器组，创建完成后单击**确认安装完毕**。

    根据页面提示创建机器组，如果您已有可用的机器组，可直接单击**使用现有机器组**。

5.  选中目标机器组，将该机器组从**源机器组**移动到**应用机器组**，单击**下一步**。

    **说明：** 如果创建机器组后立刻应用，可能因为连接未生效，导致心跳为**FAIL**，您可单击**自动重试**。如果还未解决，请参见[Logtail机器组无心跳]()进行排查。

6.  设置数据源，单击**下一步**。

    |配置项|是否必选|说明|
    |:--|:---|:-|
    |是否为Docker文件|必须勾选|确认采集的目标文件是否为Docker文件。|
    |Label白名单|可选|如果要设置Label白名单，则LabelKey必填。如果LabelValue不为空，则只采集容器label中包含LabelKey=LabelValue的容器；如果LabelValue为空，则采集所有label中包含LabelKey的容器。 **说明：**

    -   多个键值对之间为或关系，即只要容器的label满足任一键值对即可被采集。
    -   LabelValue默认为字符串匹配，即只有LabelValue和容器名称完全相同才会匹配。如果该值以^开头并且以$结尾，则为正则匹配，例如：LabelValue配置为^\(kube-system\|istio-system\)$，可同时匹配kube-system和istio-system。
    -   请勿设置相同的LabelKey，如果重名只生效一个。 |
    |Label黑名单|可选|如果要设置Label黑名单，则LabelKey必填。如果LabelValue不为空，则只排除容器label中包含LabelKey=LabelValue的容器；如果LabelValue为空，则排除所有label中包含LabelKey的容器。 **说明：**

    -   多个键值对之间为或关系，即只要容器的label满足任一键值对即可被采集。
    -   LabelValue默认为字符串匹配，即只有LabelValue和容器名称完全相同才会匹配。如果该值以^开头并且以$结尾，则为正则匹配，例如：LabelValue配置为^\(kube-system\|istio-system\)$，可同时匹配kube-system和istio-system。
    -   请勿设置相同的LabelKey，如果重名只生效一个。 |
    |环境变量白名单|可选|如果要设置环境变量白名单，则EnvKey必填。如果EnvValue不为空，则只采集容器环境变量中包含EnvKey=EnvValue的容器；如果EnvValue为空，则采集所有环境变量中包含EnvKey的容器。 **说明：**

    -   多个键值对之间为或关系，即只要容器的环境变量满足任一键值对即可被采集。
    -   EnvValue默认为字符串匹配，即只有EnvValue和容器名称完全相同才会匹配。如果该值以^开头并且以$结尾，则为正则匹配，例如：EnvValue配置为^\(kube-system\|istio-system\)$，可同时匹配kube-system和istio-system。 |
    |环境变量黑名单|可选|如果要设置环境变量黑名单，则EnvKey必填。如果EnvValue不为空，则只排除容器环境变量中包含EnvKey=EnvValue的容器；若EnvValue为空，则排除所有环境变量中包含EnvKey的容器。 **说明：**

    -   多个键值对之间为或关系，即只要容器的环境变量满足任一键值对即可被采集。
    -   EnvValue默认为字符串匹配，即只有EnvValue和容器名称完全相同才会匹配。如果该值以^开头并且以$结尾，则为正则匹配，例如：EnvValue配置为^\(kube-system\|istio-system\)$，可同时匹配kube-system和istio-system。 |
    |其他配置项|无|其他采配置项及说明请参见[概述](/intl.zh-CN/数据采集/Logtail采集/采集文本日志/概述.md)。**说明：** 采集宿主机文本文件时，默认将宿主机根目录挂载到Logtail容器的`/logtail_host`目录。所以配置路径时，您需要加上此前缀。例如需要采集宿主机上`/home/logs/app_log/`目录下的日志，配置页面中日志路径设置为`/logtail_host/home/logs/app_log/`。 |

    **说明：**

    -   Kubernetes中的namespace和容器名会映射到Docker的Label中，分别为`io.kubernetes.pod.namespace`和`io.kubernetes.container.name`。例如您创建的Pod所属namespace为backend-prod，容器名为worker-server，则仅需将其中一个配置为Label白名单，以指定只采集该容器的日志，分别为`io.kubernetes.pod.namespace : backend-prod`或者`io.kubernetes.container.name : worker-server`。
    -   Kubernetes中除`io.kubernetes.pod.namespace`和`io.kubernetes.container.name`外不建议使用其他Label。其他情况请使用环境变量白名单或环境变量黑名单。
7.  配置查询分析，单击**下一步**。

    默认已经创建索引，您也可以手动修改。


## 配置示例

-   环境变量配置方式

    例如：采集环境变量为`NGINX_PORT_80_TCP_PORT=80`且不为`POD_NAMESPACE=kube-system`的容器日志，日志文件路径为`/var/log/nginx/access.log`，日志解析方式为极简类型。

    ![environment 配置方式示例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0330559951/p54512.png)

    本示例中数据源配置如下，日志格式解析内容请参见[使用完整正则模式采集日志](/intl.zh-CN/数据采集/Logtail采集/采集文本日志/使用完整正则模式采集日志.md)、[使用Nginx模式采集日志](/intl.zh-CN/数据采集/Logtail采集/采集文本日志/使用Nginx模式采集日志.md)和[使用分隔符模式采集日志](/intl.zh-CN/数据采集/Logtail采集/采集文本日志/使用分隔符模式采集日志.md)。

    ![数据源配置示例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0330559951/p54511.png)

-   Label配置方式

    采集label为`io.kubernetes.container.name=nginx`的容器日志，日志文件路径为`/var/log/nginx/access.log`，日志解析方式为极简类型。

    ![label方式示例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0330559951/p54510.png)

    本示例中数据源配置如下，日志格式解析内容请参见[使用完整正则模式采集日志](/intl.zh-CN/数据采集/Logtail采集/采集文本日志/使用完整正则模式采集日志.md)、[使用Nginx模式采集日志](/intl.zh-CN/数据采集/Logtail采集/采集文本日志/使用Nginx模式采集日志.md)和[使用分隔符模式采集日志](/intl.zh-CN/数据采集/Logtail采集/采集文本日志/使用分隔符模式采集日志.md)。

    ![数据源设置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1330559951/p54509.png)


## 默认字段

每条日志默认上传的字段如下所示。

|字段名|说明|
|:--|:-|
|\_image\_name\_|镜像名|
|\_container\_name\_|容器名|
|\_pod\_name\_|Pod名|
|\_namespace\_|Pod所在命名空间|
|\_pod\_uid\_|Pod的唯一标识|
|\_container\_ip\_|Pod的IP地址|

