# 安装Logtail日志组件

本文介绍如何在Kubernetes集群上安装Logtail日志组件。

采集Kubernetes集群的容器日志时，需先安装Logtail日志组件，即安装alibaba-log-controller Helm。

在安装alibaba-log-controller Helm过程中自动完成以下操作：

1.  创建aliyunlogconfigs CRD（Custom Resource Definition）。
2.  部署alibaba-log-controller的Deployment。
3.  以DaemonSet模式安装Logtail。

## 在阿里云Kubernetes集群上安装

1.  登录[容器服务管理控制台](https://cs.console.aliyun.com)。

2.  在左侧导航栏中，单击**集群** \> **集群**，进入集群列表页面。

3.  单击目标集群右侧的**管理**，进入基本信息页面。

4.  在左侧导航栏中，单击**组件管理**，并在**可选组件**区域找到**logtail-ds**。

5.  在**logtail-ds**组件右侧，单击**安装**。

    ![logtail安装](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8885659951/p83541.png)


## 在自建Kubernetes集群上安装

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  创建一个以`k8s-log-custom-`开头的Project，详情请参见[创建Project](/intl.zh-CN/数据采集/准备工作/管理Project.md)。

3.  登录您的Kubernetes集群。

4.  执行以下命令安装alibaba-log-controller Helm。

    **说明：** 安装alibaba-log-controller Helm前，请确保已在Kubernetes集群中安装Helm命令。

    ```
    wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/kubernetes/alicloud-log-k8s-custom-install.sh; chmod 744 ./alicloud-log-k8s-custom-install.sh; sh ./alicloud-log-k8s-custom-install.sh {your-project-suffix} {region-id} {aliuid} {access-key-id} {access-key-secret}
    ```

    命令中各参数说明如下所示，请根据实际情况替换。

    |参数|说明|
    |:-|:-|
    |\{your-project-suffix\}|您的Project `k8s-log-custom-`的后面部分。例如Project名称为`k8s-log-custom-xxxx`，则此处填写`xxxx`。|
    |\{regionId\}|您的Project所在的区域ID，详情请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。例如`华东 1 (杭州)`的Region Id为`cn-hangzhou`。|
    |\{aliuid\}|您的阿里云主账号ID，详情请参见[获取阿里云账号ID](/intl.zh-CN/数据采集/Logtail采集/机器组/配置用户标识.md)。|
    |\{access-key-id\}|您的阿里云账号的AccessKey ID。推荐使用子账号的AccessKey，并授予子账号AliyunLogFullAccess权限，详情请参见[RAM简介](/intl.zh-CN/开发指南/访问控制RAM/简介.md)。|
    |\{access-key-secret\}|您的阿里云账号的AccessKey Secret。推荐使用子账号的AccessKey并授予子账号AliyunLogFullAccess权限，具体设置请参见[RAM简介](/intl.zh-CN/开发指南/访问控制RAM/简介.md)。|

    安装完成后，在该Project下自动创建名为`k8s-group-${your_k8s_cluster_id}`的机器组和名为`config-operation-log`的Logstore。

    **说明：**

    -   请勿删除名为`config-operation-log`的Logstore。
    -   在自建Kubernetes集群上安装时，默认为Logtail授予`privileged`权限，主要为避免删除其他Pod时可能出现的错误`container text file busy`。相关说明请参见[Bug 1468249](https://bugzilla.redhat.com/show_bug.cgi?spm=a2c4g.11186623.2.10.QhaVGc&id=1468249)、[Bug 1441737](https://bugzilla.redhat.com/show_bug.cgi?spm=a2c4g.11186623.2.11.QhaVGc&id=1441737)和 [issue 34538](https://github.com/moby/moby/issues/34538?spm=a2c4g.11186623.2.12.QhaVGc)。

## 常见问题

-   多集群如何使用同一个日志服务Project？

    如果您希望将多个集群的日志采集到同一个日志服务Project中，您可以在安装其他集群日志服务组件时，将上述安装参数中的\{your-project-suffix\}与您第一次安装集群日志服务组件时配置相同。

    **说明：** 此方式不支持跨region的Kubernetes多集群共享。

-   如何查看Logtail日志？

    Logtail日志存储在Logtail容器中的`/usr/local/ilogtail/`目录中，文件名为`ilogtail.LOG`以及`logtail_plugin.LOG`，容器stdout并不具备参考意义，请忽略以下stdout输出。

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

-   如何查看Kubernetes集群中日志相关组件的状态？

    ```
    helm status alibaba-log-controller
    ```

-   alibaba-log-controller启动失败，该怎么处理？

    请确认您是否按照以下方式进行安装。

    -   安装命令在Kubernetes集群的master节点执行。
    -   安装命令参数输入的是您的集群ID。
    若由于以上问题安装失败，请使用`helm del --purge alibaba-log-controller`删除安装包并重新执行安装命令。

-   如何查看Kubernetes集群中Logtail DaemonSet状态？

    执行命令`kubectl get ds -n kube-system`查看Logtail运行状态。

    **说明：** Logtail默认的namespace为`kube-system`。

-   如何查看Logtail的版本号、IP、启动时间等信息？

    示例如下：

    ```
    [root@iZbp1dsu6v77zfb40qfbiaZ ~]# kubectl get po -n kube-system | grep logtail
    NAME            READY     STATUS    RESTARTS   AGE
    logtail-ds-gb92k   1/1       Running   0          2h
    logtail-ds-wm7lw   1/1       Running   0          4d
    [root@iZbp1dsu6v77zfb40qfbiaZ ~]# kubectl exec logtail-ds-gb92k -n kube-system cat /usr/local/ilogtail/app_info.json
    {
       "UUID" : "",
       "hostname" : "logtail-ds-gb92k",
       "instance_id" : "0EBB2B0E-0A3B-11E8-B0CE-0A58AC140402_172.20.4.2_1517810940",
       "ip" : "172.20.4.2",
       "logtail_version" : "0.16.2",
       "os" : "Linux; 3.10.0-693.2.2.el7.x86_64; #1 SMP Tue Sep 12 22:26:13 UTC 2017; x86_64",
       "update_time" : "2018-02-05 06:09:01"
    }
    ```

-   如何查看Logtail的运行日志？

    Logtail运行日志保存在`/usr/local/ilogtail/`目录下，文件名为`ilogtail.LOG`，轮转文件会压缩存储为`ilogtail.LOG.x.gz`。

    示例如下：

    ```
    [root@iZbp1dsu6v77zfb40qfbiaZ ~]# kubectl exec logtail-ds-gb92k -n kube-system tail /usr/local/ilogtail/ilogtail.LOG
    [2018-02-05 06:09:02.168693] [INFO] [9] [build/release64/sls/ilogtail/LogtailPlugin.cpp:104] logtail plugin Resume:start
    [2018-02-05 06:09:02.168807] [INFO] [9] [build/release64/sls/ilogtail/LogtailPlugin.cpp:106] logtail plugin Resume:success
    [2018-02-05 06:09:02.168822] [INFO] [9] [build/release64/sls/ilogtail/EventDispatcher.cpp:369] start add existed check point events, size:0
    [2018-02-05 06:09:02.168827] [INFO] [9] [build/release64/sls/ilogtail/EventDispatcher.cpp:511] add existed check point events, size:0 cache size:0 event size:0 success count:0
    ```

-   如何重启某个Pod的Logtail？

    示例如下：

    ```
    [root@iZbp1dsu6v77zfb40qfbiaZ ~]# kubectl exec logtail-ds-gb92k -n kube-system /etc/init.d/ilogtaild stop
    kill process Name: ilogtail pid: 7
    kill process Name: ilogtail pid: 9
    stop success
    [root@iZbp1dsu6v77zfb40qfbiaZ ~]# kubectl exec logtail-ds-gb92k -n kube-system /etc/init.d/ilogtaild start
    ilogtail is running      
    ```


创建采集配置，完成Kubernetes集群的容器日志采集。

-   DaemonSet方式
    -   如果您需要通过CRD方式采集日志，请参见[通过DaemonSet-CRD方式采集日志](/intl.zh-CN/数据采集/Logtail采集/采集容器日志/通过DaemonSet-CRD方式采集日志.md)。
    -   如果您需要通过控制台方式采集Kubernetes标准输出，请参见[通过DaemonSet-控制台方式采集Kubernetes标准输出](/intl.zh-CN/数据采集/Logtail采集/采集容器日志/通过DaemonSet-控制台方式采集Kubernetes标准输出.md)。
    -   如果您需要通过控制台方式采集Kubernetes文件，请参见[通过日志服务采集Kubernetes容器日志](/intl.zh-CN/Kubernetes集群用户指南/可观测性/日志管理/通过日志服务采集Kubernetes容器日志.md)。
-   Sidecar方式
    -   如果您需要通过CRD方式采集日志，请参见[通过Sidecar-CRD方式采集容器日志](/intl.zh-CN/数据采集/Logtail采集/采集容器日志/通过Sidecar-CRD方式采集容器日志.md)。
    -   如果您需要通过控制台方式采集日志，请参见[通过Sidecar-控制台方式采集容器日志](/intl.zh-CN/数据采集/Logtail采集/采集容器日志/通过Sidecar-控制台方式采集容器日志.md)。

