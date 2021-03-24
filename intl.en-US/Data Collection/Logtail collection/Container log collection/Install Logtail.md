# Install Logtail

This topic describes how to install the Logtail agent in a Kubernetes cluster.

Before you collect logs from Kubernetes containers, you must install alibaba-log-controller Helm.

The following operations are automatically completed when you install alibaba-log-controller Helm:

1.  Create an aliyunlogconfigs Custom Resource Definition \(CRD\).
2.  Create a Deployment controller named alibaba-log-controller.
3.  Install Logtail in DaemonSet mode.

## Install Logtail on a Container Service for Kubernetes \(ACK\) cluster

1.  Log on to the [Container Service for Kubernetes \(ACK\) console](https://cs.console.aliyun.com).

2.  In the left-side navigation pane, choose **Cluster** \> **Cluster** to open the Clusters page.

3.  Find the cluster and click **Manage** in the Actions column to open the Basic Information page.

4.  In the left-side navigation pane, click **Add-ons**. In the **Logs and Monitoring** section, find the **logtail-ds** component.

5.  Click **Install** on the **logtail-ds** card.

    ![Install logtail-ds](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1996549951/p83541.png)


## Install Logtail on a self-managed Kubernetes cluster

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Create a project whose name starts with `k8s-log-custom-`. For more information, see [Create a project](/intl.en-US/Data Collection/Preparation/Manage a project.md).

3.  Log on to your Kubernetes cluster.

4.  Run the following command to install alibaba-log-controller Helm.

    **Note:** Before you install alibaba-log-controller Helm, make sure that you have installed Helm commands in the Kubernetes cluster.

    ```
    wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou.aliyuncs.com/kubernetes/alicloud-log-k8s-custom-install.sh; chmod 744 ./alicloud-log-k8s-custom-install.sh; sh ./alicloud-log-k8s-custom-install.sh {your-project-suffix} {region-id} {aliuid} {access-key-id} {access-key-secret}
    ```

    The following table describes the related parameters in the command. Replace the parameters with the actual values.

    |Parameter|Description|
    |:--------|:----------|
    |\{your-project-suffix\}|The part of the project name that follows `k8s-log-custom-`. For example, if the project name is `k8s-log-custom-xxxx`, you must replace the parameter with `xxxx`.|
    |\{regionId\}|The ID of the region where your project resides. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). For example, the region ID of the `China (Hangzhou)` region is `cn-hangzhou`.|
    |\{aliuid\}|The unique ID of your Alibaba Cloud account. For more information, see [Obtain the account ID](/intl.en-US/Data Collection/Logtail collection/Machine Group/Configure an account ID for a server.md).|
    |\{access-key-id\}|The AccessKey ID of your Alibaba Cloud account. We recommend that you use an AccessKey pair of a RAM user and attach the AliyunLogFullAccess permission policy to the RAM user. For more information, see [Overview](/intl.en-US/Developer Guide/Access control RAM/Overview.md).|
    |\{access-key-secret\}|The AccessKey secret of your Alibaba Cloud account. We recommend that you use an AccessKey pair of a RAM user and attach the AliyunLogFullAccess permission policy to the RAM user. For more information, see [Overview](/intl.en-US/Developer Guide/Access control RAM/Overview.md).|

    After you install Logtail, a server group named `k8s-group-${your_k8s_cluster_id}` and a Logstore named `config-operation-log` are automatically created for the project.

    **Note:**

    -   You cannot delete the `config-operation-log` Logstore.
    -   If you install Logtail on a self-managed Kubernetes cluster, Logtail is granted the `privileged` permissions by default. This avoids the `container text file busy` error if other pods are deleted. For more information, see [Bug 1468249](https://bugzilla.redhat.com/show_bug.cgi?spm=a2c4g.11186623.2.10.QhaVGc&id=1468249), [Bug 1441737](https://bugzilla.redhat.com/show_bug.cgi?spm=a2c4g.11186623.2.11.QhaVGc&id=1441737), and [Issue 34538](https://github.com/moby/moby/issues/34538?spm=a2c4g.11186623.2.12.QhaVGc).

## FAQ

-   How can I create one project in Log Service for multiple Kubernetes clusters?

    You can set the \{your-project-suffix\} parameter to the same value when you run the command to install Log Service components for these Kubernetes clusters.

    **Note:** However, this method does not apply to the logs from multiple Kubernetes clusters that reside in different regions.

-   How can I view the logs of a Logtail container?

    Logtail log files named `ilogtail.LOG` and `logtail_plugin.LOG` are stored in the `/usr/local/ilogtail/` directory of a Logtail container. In this case, the container standard output is not required. Ignore the following standard output:

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

-   How can I view the status of Log Service components in Kubernetes clusters?

    ```
    helm status alibaba-log-controller
    ```

-   What can I do if I cannot start alibaba-log-controller?

    Check whether the command that is used to install alibaba-log-controller is valid:

    -   The command is run on the master node of the Kubernetes cluster.
    -   The cluster ID specified in the command is the ID of your Kubernetes cluster.
    If alibaba-log-controller fails to be installed because one of the preceding requirements is not met, run the `helm del --purge alibaba-log-controller` command to delete alibaba-log-controller. Then, you can run the command to install alibaba-log-controller again.

-   How can I view the status of Logtail DaemonSets in a Kubernetes cluster?

    Run the `kubectl get ds -n kube-system` command to view the status of Logtail in the cluster.

    **Note:** The default namespace of Logtail is `kube-system`.

-   How can I view the version number, IP address, and startup time of Logtail?

    Example:

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

-   How can I view the operational logs of Logtail?

    The operational logs of Logtail are stored in the `ilogtail.LOG` file in the `/usr/local/ilogtail/` directory. If the log file is rotated, it is compressed and stored as `ilogtail.LOG.x.gz`.

    Example:

    ```
    [root@iZbp1dsu6v77zfb40qfbiaZ ~]# kubectl exec logtail-ds-gb92k -n kube-system tail /usr/local/ilogtail/ilogtail.LOG
    [2018-02-05 06:09:02.168693] [INFO] [9] [build/release64/sls/ilogtail/LogtailPlugin.cpp:104] logtail plugin Resume:start
    [2018-02-05 06:09:02.168807] [INFO] [9] [build/release64/sls/ilogtail/LogtailPlugin.cpp:106] logtail plugin Resume:success
    [2018-02-05 06:09:02.168822] [INFO] [9] [build/release64/sls/ilogtail/EventDispatcher.cpp:369] start add existed check point events, size:0
    [2018-02-05 06:09:02.168827] [INFO] [9] [build/release64/sls/ilogtail/EventDispatcher.cpp:511] add existed check point events, size:0 cache size:0 event size:0 success count:0
    ```

-   How can I restart Logtail that is installed on a pod?

    Example:

    ```
    [root@iZbp1dsu6v77zfb40qfbiaZ ~]# kubectl exec logtail-ds-gb92k -n kube-system /etc/init.d/ilogtaild stop
    kill process Name: ilogtail pid: 7
    kill process Name: ilogtail pid: 9
    stop success
    [root@iZbp1dsu6v77zfb40qfbiaZ ~]# kubectl exec logtail-ds-gb92k -n kube-system /etc/init.d/ilogtaild start
    ilogtail is running      
    ```


Create Logtail configuration files to collect logs from Kubernetes containers.

-   DaemonSet
    -   For information about how to collect logs by using CRDs, see [Use CRDs to collect Kubernetes container logs in the DaemonSet mode](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use CRDs to collect Kubernetes container logs in the DaemonSet mode.md).
    -   For information about how to collect stdout or stderr logs from a Kubernetes cluster by using the console, see [Use the console to collect Kubernetes stdout and stderr logs in DaemonSet mode](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use the console to collect Kubernetes stdout and stderr logs in DaemonSet mode.md).
    -   For information about how to collect Kubernetes text logs by using the console, see [Use Log Service to collect container logs](/intl.en-US/User Guide for Kubernetes Clusters/Observability/Log management/Use Log Service to collect container logs.md).
-   Sidecar
    -   For information about how to collect logs by using CRDs, see [Use CRDs to collect Kubernetes container logs in the Sidecar mode](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use CRDs to collect Kubernetes container logs in the Sidecar mode.md).
    -   For information about how to collect logs by using the console, see [Use the console to collect Kubernetes container logs in the Sidecar mode](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use the console to collect Kubernetes container logs in the Sidecar mode.md).

