# 采集标准Docker容器日志

本文介绍如何部署Logtail容器及创建Logtail配置，采集标准Docker容器日志。

## 步骤一：部署Logtail容器

1.  拉取Logtail镜像。

    ```
    docker pull registry.cn-hangzhou.aliyuncs.com/log-service/logtail
    ```

    其中，registry.cn-hangzhou.aliyuncs.com请根据实际情况替换，地域信息请参见[表 1](/intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)。如果您的服务器处于阿里云VPC网络中，需将registry修改为registry-vpc。

2.  启动Logtail容器。

    **说明：** 请在配置参数前执行以下任意一种配置，否则删除其他container时可能出现错误`container text file busy`。

    -   Centos 7.4及以上版本设置fs.may\_detach\_mounts=1，相关说明请参见[Bug 1468249](https://bugzilla.redhat.com/show_bug.cgi?id=1468249)、[Bug 1441737](https://bugzilla.redhat.com/show_bug.cgi?id=1441737)和[issue 34538](https://github.com/moby/moby/issues/34538)。
    -   为Logtail授予`privileged`权限，启动参数中添加`--privileged`。更多信息，请参见[docker run命令](https://docs.docker.com/engine/reference/run/)。
    根据实际情况替换模板中的3个参数：`${your_region_name}`、`${your_aliyun_user_id}`和`${your_machine_group_user_defined_id}`。

    ```
    docker run -d -v /:/logtail_host:ro -v /var/run:/var/run --env ALIYUN_LOGTAIL_CONFIG=/etc/ilogtail/conf/${your_region_name}/ilogtail_config.json --env ALIYUN_LOGTAIL_USER_ID=${your_aliyun_user_id} --env ALIYUN_LOGTAIL_USER_DEFINED_ID=${your_machine_group_user_defined_id} registry.cn-hangzhou.aliyuncs.com/log-service/logtail
    ```

    |参数|参数说明|
    |--|----|
    |`${your_region_name}`|请根据日志服务Project所在地及网络类型填写。其中，地域信息请参见[表 1](/intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)。     -   如果为公网，格式为：`region-internet`，例如：**华东 1（杭州）**为**cn-hangzhou-internet**。
    -   如果为阿里云内网，格式为`region`。例如：**华东 1（杭州）**为**cn-hangzhou**。 |
    |`${your_aliyun_user_id}`|您的阿里云主账号ID。更多信息，请参见[配置用户标识](/intl.zh-CN/数据采集/Logtail采集/机器组/配置用户标识.md)。|
    |`${your_machine_group_user_defined_id}`|您机器组的自定义标识，请确保该标识在您的Project所在地域内唯一。更多信息，请参见[创建用户自定义标识机器组](/intl.zh-CN/数据采集/Logtail采集/机器组/创建用户自定义标识机器组.md)。|

    **说明：**

    您可以自定义配置Logtail容器的启动参数，只需保证以下前提条件。

    1.  启动时，必须配置3个环境变量：`ALIYUN_LOGTAIL_USER_DEFINED_ID`、`ALIYUN_LOGTAIL_USER_ID`、`ALIYUN_LOGTAIL_CONFIG`。
    2.  必须宿主机将/var/run挂载到Logtail容器的/var/run目录。
    3.  将宿主机根目录挂载到Logtail容器的`/logtail_host`目录。
    4.  如果Logtail日志/usr/local/ilogtail/ilogtail.LOG中出现`The parameter is invalid : uuid=none`的错误日志，请在宿主机上创建一个product\_uuid文件，在其中输入任意合法UUID（例如`169E98C9-ABC0-4A92-B1D2-AA6239C0D261`），并把该文件挂载到Logtail容器的/sys/class/dmi/id/product\_uuid目录。

## 步骤二：创建采集配置

请根据您的需求在控制台上创建采集配置。

-   如果您需要采集Docker文件，操作步骤与采集Kubernetes文件类似。更多信息，请参见[通过DaemonSet-控制台方式采集文本文件](/intl.zh-CN/数据采集/Logtail采集/采集容器日志/通过DaemonSet-控制台方式采集Kubernetes文件.md)。
-   如果您需要采集Docker标准输出，操作步骤与采集Kubernetes标准输出类似。更多信息，请参见[通过DaemonSet-控制台方式采集标准输出](/intl.zh-CN/数据采集/Logtail采集/采集容器日志/通过DaemonSet-控制台方式采集Kubernetes标准输出.md)。
-   如果您需要采集宿主机文本文件。更多信息，请参见[采集宿主机文本文件](/intl.zh-CN/数据采集/Logtail采集/采集文本日志/概述.md)。

    默认将宿主机根目录挂载到Logtail容器的`/logtail_host`目录。配置路径时，您需要加上此前缀。例如需要采集宿主机上`/home/logs/app_log/`目录下的日志，配置页面中日志路径设置为`/logtail_host/home/logs/app_log/`。


其中，在创建机器组时，请在**用户自定义标识**中输入[步骤一：部署Logtail容器](#section_yqz_nfq_pdb)时配置的`ALIYUN_LOGTAIL_USER_DEFINED_ID`。

![配置机器组](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1625287951/p2677.png)

## 默认字段

-   Docker标准输出

    每条日志默认上传字段如下所示。

    |字段名|说明|
    |:--|:-|
    |`_time_`|数据上传时间，例如：`2018-02-02T02:18:41.979147844Z`|
    |`_source_`|输入源类型，stdout或stderr|
    |`_image_name_`|镜像名|
    |`_container_name_`|容器名|
    |`_container_ip_`|容器IP地址|

-   Docker文件

    默认每条日志上传的字段如下所示。

    |字段名|说明|
    |:--|:-|
    |`_image_name_`|镜像名|
    |`_container_name_`|容器名|
    |`_container_ip_`|容器IP地址|


## 其他操作

-   查看Logtail容器运行状态。

    您可以执行命令`docker exec ${logtail_container_id} /etc/init.d/ilogtaild status`查看Logtail运行状态。

-   查看Logtail的版本号信息、IP、启动时间等。

    您可以执行命令`docker exec ${logtail_container_id} cat /usr/local/ilogtail/app_info.json`查看Logtail相关信息。

-   查看Logtail的运行日志。

    Logtail运行日志保存在`/usr/local/ilogtail/`目录下，文件名为`ilogtail.LOG`，轮转文件会压缩存储为`ilogtail.LOG.x.gz`。

    示例如下：

    ```
    [root@iZbp17enxc2us3624wexh2Z ilogtail]# docker exec a287de895e40 tail -n 5 /usr/local/ilogtail/ilogtail.LOG
    [2018-02-06 08:13:35.721864]    [INFO]    [8]    [build/release64/sls/ilogtail/LogtailPlugin.cpp:104]    logtail plugin Resume:start
    [2018-02-06 08:13:35.722135]    [INFO]    [8]    [build/release64/sls/ilogtail/LogtailPlugin.cpp:106]    logtail plugin Resume:success
    [2018-02-06 08:13:35.722149]    [INFO]    [8]    [build/release64/sls/ilogtail/EventDispatcher.cpp:369]    start add existed check point events, size:0
    [2018-02-06 08:13:35.722155]    [INFO]    [8]    [build/release64/sls/ilogtail/EventDispatcher.cpp:511]    add existed check point events, size:0    cache size:0    event size:0    success count:0
    [2018-02-06 08:13:39.725417]    [INFO]    [8]    [build/release64/sls/ilogtail/ConfigManager.cpp:3776]    check container path update flag:0    size:1
    ```

    容器stdout并不具备参考意义，请忽略以下stdout输出。

    ```
    
    start umount useless mount points, /shm$|/merged$|/mqueue$
    umount: /logtail_host/var/lib/docker/overlay2/3fd0043af174cb0273c3c7869500fbe2bdb95d13b1e110172ef57fe840c82155/merged: must be superuser to unmount
    umount: /logtail_host/var/lib/docker/overlay2/d5b10aa19399992755de1f85d25009528daa749c1bf8c16edff44beab6e69718/merged: must be superuser to unmount
    umount: /logtail_host/var/lib/docker/overlay2/5c3125daddacedec29df72ad0c52fac800cd56c6e880dc4e8a640b1e16c22dbe/merged: must be superuser to unmount
    ......
    xargs: umount: exited with status 255; aborting
    umount done
    start logtail
    ilogtail is running
    logtail status:
    ilogtail is running
    ```

-   重启Logtail。

    请参考以下示例重启Logtail。

    ```
    [root@iZbp17enxc2us3624wexh2Z ilogtail]# docker exec a287de895e40 /etc/init.d/ilogtaild stop
    kill process Name: ilogtail pid: 7
    kill process Name: ilogtail pid: 8
    stop success
    [root@iZbp17enxc2us3624wexh2Z ilogtail]# docker exec a287de895e40 /etc/init.d/ilogtaild start
    ilogtail is running
    ```


