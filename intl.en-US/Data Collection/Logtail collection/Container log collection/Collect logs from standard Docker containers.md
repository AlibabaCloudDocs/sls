# Collect logs from standard Docker containers

This topic describes how to deploy a Logtail container and create a Logtail configuration file to collect logs from standard Docker containers.

## Step 1: Deploy a Logtail container

1.  Run the following command to pull the Logtail image:

    ```
    docker pull registry.cn-hangzhou.aliyuncs.com/log-service/logtail
    ```

2.  Start a Logtail container.

    **Note:** Before you set the parameters, you must complete one of the following configurations. Otherwise, the `container text file busy` error may occur when you delete another container.

    -   For CentOS 7.4 and later, set fs.may\_detach\_mounts to 1. For more information, see [Bug 1468249](https://bugzilla.redhat.com/show_bug.cgi?id=1468249), [Bug 1441737](https://bugzilla.redhat.com/show_bug.cgi?id=1441737), and [Issue 34538](https://github.com/moby/moby/issues/34538).
    -   Add `--privileged` to the startup parameters to grant Logtail the `privileged` permission. For more information, see [Docker run reference](https://docs.docker.com/engine/reference/run/).
    Replace the `${your_region_name}`, `${your_aliyun_user_id}`, and `${your_machine_group_user_defined_id}` parameters in the following command based on the actual scenario:

    ```
    docker run -d -v /:/logtail_host:ro -v /var/run:/var/run --env ALIYUN_LOGTAIL_CONFIG=/etc/ilogtail/conf/${your_region_name}/ilogtail_config.json --env ALIYUN_LOGTAIL_USER_ID=${your_aliyun_user_id} --env ALIYUN_LOGTAIL_USER_DEFINED_ID=${your_machine_group_user_defined_id} registry.cn-hangzhou.aliyuncs.com/log-service/logtail
    ```

    |Parameter|Description|
    |---------|-----------|
    |`${your_region_name}`|The ID of the region where your project resides and the type of the network that your project uses. For more information, see [Table 1](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md).     -   If your project uses the Internet, set the value in the `region-internet` format, for example, **cn-hangzhou-Internet**.
    -   If your project uses the Alibaba Cloud internal network, set the value in the `region` format, for example, **cn-hangzhou**. |
    |`${your_aliyun_user_id}`|The unique ID of your Alibaba Cloud account. For more information, see [Configure an account ID for a server](/intl.en-US/Data Collection/Logtail collection/Machine Group/Configure an account ID for a server.md).|
    |`${your_machine_group_user_defined_id}`|The custom identifier of your server group. The identifier must be unique in the region where your project resides. For more information, see [Create a custom ID-based machine group](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create a custom ID-based machine group.md).|

    **Note:**

    You can customize the startup parameters of the Logtail container if the following conditions are met:

    1.  The following environment variables are configured: `ALIYUN_LOGTAIL_USER_DEFINED_ID`, `ALIYUN_LOGTAIL_USER_ID`, and `ALIYUN_LOGTAIL_CONFIG`.
    2.  The /var/run directory of the host is mounted on the /var/run directory of the Logtail container.
    3.  The root directory of the host is mounted on the `/logtail_host` directory of the Logtail container.
    4.  If the `The parameter is invalid : uuid=none` error is returned in the /usr/local/ilogtail/ilogtail.LOG log file, you must create a file named product\_uuid on the host. Then, you must enter a valid UUID, such as`169E98C9-ABC0-4A92-B1D2-AA6239C0D261` in the file and mount the file on the /sys/class/dmi/id/product\_uuid directory of the Logtail container.

## Step 2: Create a collection configuration file

Configure log collection in the console based on your business requirements.

-   To collect Docker text logs, follow the steps that you perform to collect Kubernetes text logs. For more information, see [Use the console to collect Kubernetes text logs in DaemonSet mode](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use the console to collect Kubernetes text logs in DaemonSet mode.md).
-   To collect Docker stdout and stderr logs, follow the steps that you perform to collect Kubernetes stdout and stderr logs. For more information, see [Use the console to collect Kubernetes stdout and stderr logs in DaemonSet mode](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use the console to collect Kubernetes stdout and stderr logs in DaemonSet mode.md).
-   To collect host text logs, follow the steps provided in [Collect text logs](/intl.en-US/Data Collection/Logtail collection/Text logs/Overview.md).

    By default, the root directory of the host is mounted on the `/logtail_host` directory of the Logtail container. When you configure the directory for log collection, you must add the container directory as the prefix to the log path. For example, to collect data from the `/home/logs/app_log/` directory of the host, you must set the log path to `/logtail_host/home/logs/app_log/`.


When you create a server group, enter the value of the `ALIYUN_LOGTAIL_USER_DEFINED_ID` parameter in the **Custom Identifier** field. This value is specified in [Step 1: Deploy a Logtail container](#section_yqz_nfq_pdb).

![Select a server group](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8996549951/p2677.png)

## Default fields

-   Docker stdout and stderr logs

    The following table describes the fields that are uploaded by default for each log entry.

    |Log field|Description|
    |:--------|:----------|
    |`_time_`|The data upload time, for example, `2018-02-02T02:18:41.979147844Z`.|
    |`_source_`|The type of a data source. Valid values: stdout and stderr.|
    |`_image_name_`|The name of an image.|
    |`_container_name_`|The name of a container.|
    |`_container_ip_`|The IP address assigned to the pod where a container resides.|

-   Docker text logs

    The following table describes the fields that are uploaded by default for each log entry.

    |Log field|Description|
    |:--------|:----------|
    |`_image_name_`|The name of an image.|
    |`_container_name_`|The name of a container.|
    |`_container_ip_`|The IP address assigned to the pod where a container resides.|


## Related operations

-   View the status of the Logtail container.

    You can run the `docker exec ${logtail_container_id} /etc/init.d/ilogtaild status` command to view the status of Logtail.

-   View the version number, IP address, and startup time of Logtail.

    You can run the `docker exec ${logtail_container_id} cat /usr/local/ilogtail/app_info.json` command to view Logtail information.

-   View the operations logs of Logtail.

    The operations logs of Logtail are stored in the `ilogtail.LOG` file in the `/usr/local/ilogtail/` directory. If the log file is rotated and compressed, it is stored as a file named `ilogtail.LOG.x.gz`.

    Example:

    ```
    [root@iZbp17enxc2us3624wexh2Z ilogtail]# docker exec a287de895e40 tail -n 5 /usr/local/ilogtail/ilogtail.LOG
    [2018-02-06 08:13:35.721864]    [INFO]    [8]    [build/release64/sls/ilogtail/LogtailPlugin.cpp:104]    logtail plugin Resume:start
    [2018-02-06 08:13:35.722135]    [INFO]    [8]    [build/release64/sls/ilogtail/LogtailPlugin.cpp:106]    logtail plugin Resume:success
    [2018-02-06 08:13:35.722149]    [INFO]    [8]    [build/release64/sls/ilogtail/EventDispatcher.cpp:369]    start add existed check point events, size:0
    [2018-02-06 08:13:35.722155]    [INFO]    [8]    [build/release64/sls/ilogtail/EventDispatcher.cpp:511]    add existed check point events, size:0    cache size:0    event size:0    success count:0
    [2018-02-06 08:13:39.725417]    [INFO]    [8]    [build/release64/sls/ilogtail/ConfigManager.cpp:3776]    check container path update flag:0    size:1
    ```

    The standard output of the container is irrelevant to this case. Ignore the following standard output:

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

-   Restart Logtail.

    To restart Logtail, use the following sample code:

    ```
    [root@iZbp17enxc2us3624wexh2Z ilogtail]# docker exec a287de895e40 /etc/init.d/ilogtaild stop
    kill process Name: ilogtail pid: 7
    kill process Name: ilogtail pid: 8
    stop success
    [root@iZbp17enxc2us3624wexh2Z ilogtail]# docker exec a287de895e40 /etc/init.d/ilogtaild start
    ilogtail is running
    ```


