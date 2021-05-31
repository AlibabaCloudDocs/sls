# Troubleshoot log collection exceptions in containers

This topic provides solutions to exceptions that may occur when you use a Logtail container \(a common container or Kubernetes\) to collect logs.

Troubleshooting operations:

-   [Troubleshoot heartbeat exceptions in a machine group](#section_azl_rfh_1fb)
-   [Troubleshoot log collection exceptions in a container](#section_cps_vch_1fb)

Other O&M operations:

-   [Log on to the Logtail container](#section_hg2_hsb_dfb)
-   [View Logtail operational logs](#section_frc_wjh_1fb)
-   [View Logtail standard output \(stdout\)](#section_g1k_5sb_dfb)
-   [View the status of log-related components in a Kubernetes cluster](#section_xls_blh_1fb)
-   [View the version information, IP address, and time of Logtail](#section_f52_2lh_1fb)
-   [What do I do if I mistakenly delete a Logstore that is created through CRD?](#section_uwm_hlh_1fb)

## Troubleshoot heartbeat exceptions in a machine group

You can determine whether the Logtail on a container is correctly installed by checking the heartbeat status of a machine group.

1.  Check the heartbeat status of the machine group.
    1.  Log on to the [Log Service console](https://sls.console.aliyun.com), and then click the target project name.
    2.  In the left-side navigation pane, click **Logtail Machine Group**.
    3.  Find the target machine group and click **Status**.

        Record the number of nodes for which heartbeat status is **OK**.

2.  Check the number of Worker nodes in the cluster.

    Run `kubectl get node | grep -v master` to view the number of Worker nodes.

    ```
    $kubectl get node | grep -v master
    NAME                                 STATUS    ROLES     AGE       VERSION
    cn-hangzhou.i-bp17enxc2us3624wexh2   Ready     <none>    238d      v1.10.4
    cn-hangzhou.i-bp1ad2b02jtqd1shi2ut   Ready     <none>    220d      v1.10.4
    ```

3.  Compare whether the number of the nodes with heartbeat status of **OK** is the same as the number of Worker nodes. Then, use an appropriate troubleshooting method according to the following possible comparison results:
    -   The heartbeat status of all nodes is **Failed**.
        -   If you use [standard Docker logs](/intl.en-US/Data Collection/Logtail collection/Container log collection/Collect logs from standard Docker containers.md), check whether $\{your\_region\_name\}, $\{your\_aliyun\_user\_id\}, and $\{your\_machine\_group\_user\_defined\_id\} are correct by following the instructions provided in [parameter description](/intl.en-US/Data Collection/Logtail collection/Container log collection/Collect logs from standard Docker containers.md).
        -   If you use [installation for Kubernetes on Alibaba Cloud Container Service](/intl.en-US/Data Collection/Logtail collection/Container log collection/Overview.md), open a ticket.
        -   If you use [self-built Kubernetes installation](/intl.en-US/Data Collection/Logtail collection/Container log collection/Overview.md), check whether \{your-project-suffix\}, \{regionId\}, \{aliuid\}, \{access-key-id\}, and \{access-key-secret\} are correct by following the instructions provided in [parameter description](/intl.en-US/Data Collection/Logtail collection/Container log collection/Overview.md). If the parameters are incorrect, run `helm del --purge alibaba-log-controller` to delete the installation package and reinstall Kubernetes.
    -   The number of nodes for which the heartbeat status is OK is smaller than the number of Worker nodes.

        1.  Determine whether to use the yaml file to manually deploy DaemonSet.

            Run `kubectl get po -n kube-system -l k8s-app=logtail`. If any result is returned, you have manually deployed DaemonSet by using the yaml file.

        2.  Download the latest [DaemonSet template](http://logtail-release.oss-cn-hangzhou.aliyuncs.com/docker/k8s/logtail-daemonset.yaml).
        3.  Set $\{your\_region\_name\}, $\{your\_aliyun\_user\_id\}, and $\{your\_machine\_group\_name\} as needed.
        4.  Run `kubectl apply -f ./logtail-daemonset.yaml` to update the DaemonSet yaml file.
        For other comparison results, open a ticket.


## Troubleshoot log collection exceptions in a container

If you cannot find any log on the preview or query page in the console, Log Service has not collected any log from your container. In this case, check the container status and perform the following steps:

1.  [Check whether the machine group status is normal](#section_azl_rfh_1fb).
2.  Check whether the Config identifier is correct.

    Check whether IncludeLabel, ExcludeLabel, IncludeEnv, and ExcludeEnv in the Config match the configurations of the target container.

    **Note:** `Label` indicates the container label \(label information in docker inspect\) instead of the one defined in Kubernetes. You can temporarily remove the parameters and check whether any log can be collected. If yes, the exception is caused by an incorrect Config identifier.

3.  Check other items.

    If you want to collect files from your container, note that:

    -   Logtail does not collect any file if there are no modified files in your container.
    -   Only the files that are stored by default or mounted to your local PC can be collected.

## Log on to the Logtail container

-   **Common Docker**

    1.  On the host, run `docker ps | grep logtail` to search for the Logtail container.
    2.  Run `docker exec -it ****** bash` to log on to the Logtail container.
    ```
    $docker ps | grep logtail
    223fbd3ed2a6e        registry.cn-hangzhou.aliyuncs.com/log-service/logtail                             "/usr/local/ilogta..."   8 days ago          Up 8 days                               logtail-iba
    $docker exec -it 223fbd3ed2a6e bash
    ```

-   **Kubernetes**

    1.  Run `kubectl get po -n kube-system | grep logtail` to search for the Logtail Pod.
    2.  Run `kubectl exec -it -n kube-system ****** bash` to log on to the Pod.
    ```
    $kubectl get po -n kube-system | grep logtail
    logtail-ds-g5wgd                                             1/1       Running    0          8d
    logtail-ds-slpn8                                             1/1       Running    0          8d
    $kubectl exec -it -n kube-system logtail-ds-g5wgd bash
    ```


## View Logtail operational logs

Logtail logs named ilogtail.LOG and logtail\_plugin.LOG are stored in the /usr/local/ilogtail/ directory.

1.  [Log on to the Logtail container](#section_hg2_hsb_dfb).
2.  Open the /usr/local/ilogtail/ directory.

    ```
    cd /usr/local/ilogtail
    ```

3.  View the ilogtail.LOG and logtail\_plugin.LOG files.

    ```
    cat ilogtail.LOG
    cat logtail_plugin.LOG
    ```


## View Logtail standard output \(stdout\)

You can ignore the following stdout because the container stdout has no reference for application.

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

## View the status of log-related components in a Kubernetes cluster

To view the status of log-related components in a Kubernetes cluster, you can run `helm status alibaba-log-controller`.

## View the version information, IP address, and time of Logtail

The related information is stored in the app\_info.json file under the /usr/local/ilogtail/ directory in the Logtail container. The following is an example:

```
kubectl exec logtail-ds-gb92k -n kube-system cat /usr/local/ilogtail/app_info.json
{
   "UUID" : "",
   "hostname" : "logtail-gb92k",
   "instance_id" : "0EBB2B0E-0A3B-11E8-B0CE-0A58AC140402_172.20.4.2_1517810940",
   "ip" : "172.20.4.2",
   "logtail_version" : "0.16.2",
   "os" : "Linux; 3.10.0-693.2.2.el7.x86_64; #1 SMP Tue Sep 12 22:26:13 UTC 2017; x86_64",
   "update_time" : "2018-02-05 06:09:01"
}
```

## What do I do if I mistakenly delete a Logstore that is created through CRD?

If you delete a Logstore that is automatically created through CRD, the collected data cannot be recovered, and the CRD configurations of the Logstore become invalid. In this case, you can use either of the following methods to prevent possible log collection exceptions:

-   Use another CRD-created Logstore and take care to name the Logstore with a different name to the Logstore that was mistakenly deleted.
-   Restart the alibaba-log-controller Pod. You can run `kubectl get po -n kube-system | grep alibaba-log-controller` to search for the Pod.

