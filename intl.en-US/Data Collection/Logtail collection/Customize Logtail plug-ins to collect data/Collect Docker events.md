# Collect Docker events

Docker events include all interactive events of objects such as containers, images, plug-ins, networks, and volumes. This topic describes how to configure Logtail in the Log Service console to collect Docker events.

Logtail is installed on the server that you use to collect Docker events. For more information, see [Install Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md).

**Note:** Only Linux servers that run Logtail 0.16.18 or later are supported.

## Limits

-   Logtail that runs on containers or hosts must be authorized to access the `/var/run/docker.sock` file.

    For information about how to use Logtail to collect Kubernetes logs, see [Collect Kubernetes logs](/intl.en-US/Data Collection/Logtail collection/Container log collection/Overview.md). For information about how to collect standard container logs, see [Collect logs from standard Docker containers](/intl.en-US/Data Collection/Logtail collection/Container log collection/Collect logs from standard Docker containers.md).

-   When Logtail is restarted or stopped, container events are not collected.

## Scenarios

-   Monitor the start and stop events of all containers, and trigger alerts when core containers stop running.
-   Collect all container events for auditing, security analysis, and troubleshooting.
-   Monitor all image pulling events, and trigger an alert if an image is pulled from an invalid path.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, select **Custom Data Plug-in**.

3.  Select a destination project and Logstore, and then click **Next**.

    You can also click **Create Now** to create a project and a Logstore.

4.  In the Create Machine Group step, create a machine group.

    -   If a machine group is available, click **Using Existing Machine Groups**.
    -   If no machine groups are available, perform the following steps to create a machine group. In this example, ECS instances are used.
        1.  Install Logtail on ECS instances. For more information, see [Install Logtail on ECS instances](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail on ECS instances.md).

            If Logtail is installed on the ECS instances, click **Complete Installation**.

            **Note:** If you want to collect logs from self-managed clusters or servers of third-party cloud service providers, you must install Logtail on these servers. For more information, see [Install Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md#).

        2.  After you install Logtail, click **Complete Installation**.
        3.  On the page that appears, set related parameters for the machine group. For more information, see [Create an IP address-based machine group](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create an IP address-based machine group.md) or [Create a custom ID-based machine group](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create a custom ID-based machine group.md).
5.  Select and move the destination machine group from **Source Server Groups** to **Applied Server Groups**, and then click **Next**.

    **Note:** If you want to apply a machine group immediately after it is created, the heartbeat status of the machine group may be **FAIL**. This is because the machine group has not been connected to Log Service. In this case, you can click **Automatic Retry**. If the problem persists, see [What can I do if the Logtail client has no heartbeat?]()

6.  In the Specify Data Source step, set the **Config Name** and **Plug-in Config** parameters.

    -   inputs: Required. The Logtail configurations for log collection.

        **Note:** You can configure only one type of data source in the inputs field.

    -   processors: Optional. The Logtail configurations for data processing. You can configure one or more processing methods in the processors field. For more information, see [Overview](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Overview.md).
    ```
    {
      "inputs": [
        {
          "detail": {},
          "type": "service_docker_event"
        }
      ]
    }
    ```

    |Parameter|Type|Required|Description|
    |:--------|:---|:-------|:----------|
    |type|string|Yes|The type of the data source. Set the value to service\_docker\_event.|
    |EventQueueSize|int|No|The maximum number of events in the event queue. Default value: 10.|

7.  Preview the data and click **Next**.

    By default, Log Service enables the Full Text Index to query and analyze logs. For more information, see [Enable and configure the indexing feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

    **Note:**

    -   The index is applicable only to the log data that is newly written.
    -   To query and analyze logs, you must enable the Full Text Index or Field Search. If you enable both of them, the settings of Field Search prevail.

After Logtail collects Docker events and uploads the events to Log Service, you can view the events in the Log Service console. The following examples show multiple event log entries.

-   Example 1: image pulling event

    ```
    __source__:  10.10.10.10
    __tag__:__hostname__:  logtail-ds-77brr
    __topic__:  
    _action_:  pull
    _id_:  registry.cn-hangzhou.aliyuncs.com/ringtail/eventer:v1.6.1.3
    _time_nano_:  1547910184047414271
    _type_:  image
    name:  registry.cn-hangzhou.aliyuncs.com/ringtail/eventer
    ```

-   Example 2: container destruction event in Kubernetes

    ```
    __source__:  10.10.10.10
    __tag__:__hostname__:  logtail-ds-xnvz2
    __topic__:  
    _action_:  destroy
    _id_:  af61340b0ac19e6f5f32be672d81a33fc4d3d247bf7dbd4d3b2c030b8bec4a03
    _time_nano_:  1547968139380572119
    _type_:  container
    annotation.kubernetes.io/config.seen:  2019-01-20T15:03:03.114145184+08:00
    annotation.kubernetes.io/config.source:  api
    annotation.scheduler.alpha.kubernetes.io/critical-pod:  
    controller-revision-hash:  2630731929
    image:  registry-vpc.cn-hangzhou.aliyuncs.com/acs/pause-amd64:3.0
    io.kubernetes.container.name:  POD
    io.kubernetes.docker.type:  podsandbox
    io.kubernetes.pod.name:  logtail-ds-44jbg
    io.kubernetes.pod.namespace:  kube-system
    io.kubernetes.pod.uid:  6ddcf598-1c81-11e9-9ddf-00163e0c7cbe
    k8s-app:  logtail-ds
    kubernetes.io/cluster-service:  true
    name:  k8s_POD_logtail-ds-44jbg_kube-system_6ddcf598-1c81-11e9-9ddf-00163e0c7cbe_0
    pod-template-generation:  9
    version:  v1.0
    ```


The following table describes the log fields of Docker events. For more information, see [Docker events](https://docs.docker.com/engine/reference/commandline/events/).

|Log field|Description|
|:--------|:----------|
|\_type\_|The type of a resource, for example, container or image.|
|\_action\_|The type of an action, for example, destroy or status.|
|\_id\_|The unique ID of an event.|
|\_time\_nano\_|The timestamp of an event.|

