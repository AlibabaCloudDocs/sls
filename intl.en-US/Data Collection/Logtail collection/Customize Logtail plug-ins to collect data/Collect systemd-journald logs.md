# Collect systemd-journald logs

You can use Logtail to collect Linux systemd-journald logs from binary files. This topic describes how to configure Logtail in the Log Service console to collect systemd-journald logs.

Logtail is installed on the server that you use to collect systemd-journald logs. For more information, see [Install Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md).

**Note:** Only Linux servers that run Logtail 0.16.18 or later are supported.

## Overview

systemd is a system and service manager for Linux. When systemd runs as an initialization process \(PID = 1\), it boots and maintains the services of user spaces. systemd manages kernel and application logs in a centralized manner. The configuration file that is used to manage these logs is /etc/systemd/journald.conf.

**Note:** The operating system on which systemd runs must support the journal log format.

## Features

-   Allows you to set the initial collection position. Checkpoints are automatically saved for subsequent data collection. The process is not affected when applications are restarted.
-   Allows you to filter specified applications.
-   Allows you to collect kernel logs.
-   Supports automatic parsing of log severity.
-   Allows you to run systemd as a container to collect journal logs from hosts. This feature is applicable when you collect logs from Docker and Kubernetes clusters.

## Scenarios

-   Monitor kernel events, and trigger alerts when exceptions occur.
-   Collect system logs for long-term storage and release disk space.
-   Collect application logs for analysis or alerting.
-   Collect all journal logs and query log data with higher efficiency than journalctl.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, select **Custom Data Plug-in**.

3.  Select a destination project and Logstore, and then click **Next**.

    You can also click **Create Now** to create a project and a Logstore.

4.  In the Create Machine Group step, create a machine group.

    -   If a machine group is available, click **Using Existing Machine Groups**.
    -   If no machine groups are available, perform the following steps to create a machine group. ECS instances are used as an example.
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
          "detail": {
            "JournalPaths": [
              "/var/log/journal"
            ],
            "Kernel": true,
            "ParsePriority": true,
            "ParseSyslogFacility": true
          },
          "type": "service_journal"
        }
      ]
    }
    ```

    |Parameter|Type|Required|Description|
    |:--------|:---|:-------|:----------|
    |type|string|Yes|The type of the data source. Set the value to service\_journal.|
    |JournalPaths|String array|Yes|The path of journal logs. We recommend that you set the value to the directory of journal logs, for example, /var/log/journal.|
    |SeekPosition|string|No|The method that is used to collect logs for the first time. Valid values: head and tail. Default value: tail.     -   If you set the value to head, all data is collected.
    -   If you set the value to tail, only the data that is generated after the Logtail configuration file takes effect is collected. |
    |Kernel|bool|No|Specifies whether to collect kernel logs. Default value: true. This value indicates that kernel logs are collected.|
    |Units|String array|No|The applications from which logs are collected. Default value: null. This value indicates that logs from all applications are collected.|
    |ParseSyslogFacility|bool|No|Specifies whether to parse the facility field of syslog logs. Default value: false. This value indicates that the facility field of syslog logs is not parsed.|
    |ParsePriority|bool|No|Specifies whether to parse the priority field of syslog logs. Default value: false. This value indicates that the priority field of syslog logs is not parsed. If you set the value to true, the priority field of syslog logs is parsed based on the following mapping relationships:

    ```
"0": "emergency"
  "1": "alert"
  "2": "critical"
  "3": "error"
  "4": "warning"
  "5": "notice"
  "6": "informational"
  "7": "debug"
    ``` |
    |UseJournalEventTime|bool|No|Specifies whether to use the field of journal logs as the timestamp of the log. Default value: false. This value indicates that the time when a log entry is collected is recorded as the timestamp of the log entry. In most cases, the difference between the log collection time and the log timestamp is less than 3 seconds.|

7.  Preview the data and click **Next**.

    By default, Log Service enables the Full Text Index to query and analyze logs. For more information, see [Enable and configure the indexing feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

    **Note:**

    -   The index is applicable only to the log data that is newly written.
    -   To query and analyze logs, you must enable the Full Text Index or Field Search. If you enable both of them, the settings of Field Search prevail.

## Examples

-   Example 1

    To collect journal logs from the `/var/log/journal` directory, you can use the following configuration file:

    ```
    {
      "inputs": [
        {
          "detail": {
            "JournalPaths": [
              "/var/log/journal"
            ]
          },
          "type": "service_journal"
        }
      ]
    }
    ```

    Sample log entry:

    ```
    MESSAGE:  rejected connection from "192.168.0.250:43936" (error "EOF", ServerName "")
    PACKAGE:  embed
    PRIORITY:  6
    SYSLOG_IDENTIFIER:  etcd
    _BOOT_ID:  fe919cd1268f4721bd87b5c18afe59c3
    _CAP_EFFECTIVE:  0
    _CMDLINE:  /usr/bin/etcd --election-timeout=3000 --heartbeat-interval=500 --snapshot-count=50000 --data-dir=data.etcd --name 192.168.0.251-name-3 --client-cert-auth --trusted-ca-file=/var/lib/etcd/cert/ca.pem --cert-file=/var/lib/etcd/cert/etcd-server.pem --key-file=/var/lib/etcd/cert/etcd-server-key.pem --peer-client-cert-auth --peer-trusted-ca-file=/var/lib/etcd/cert/peer-ca.pem --peer-cert-file=/var/lib/etcd/cert/192.168.0.251-name-3.pem --peer-key-file=/var/lib/etcd/cert/192.168.0.251-name-3-key.pem --initial-advertise-peer-urls https://192.168.0.251:2380 --listen-peer-urls https://192.168.0.251:2380 --advertise-client-urls https://192.168.0.251:2379 --listen-client-urls https://192.168.0.251:2379 --initial-cluster 192.168.0.249-name-1=https://192.168.0.249:2380,192.168.0.250-name-2=https://192.168.0.250:2380,192.168.0.251-name-3=https://192.168.0.251:2380 --initial-cluster-state new --initial-cluster-token abac64c8-baab-4ae6-8412-4253d3cfb0cf
    _COMM:  etcd
    _EXE:  /opt/etcd-v3.3.8/etcd
    _GID:  995
    _HOSTNAME:  iZbp1f7y2ikfe4l8nx95amZ
    _MACHINE_ID:  f0f31005fb5a436d88e3c6cbf54e25aa
    _PID:  10926
    _SOURCE_REALTIME_TIMESTAMP:  1546854068863857
    _SYSTEMD_CGROUP:  /system.slice/etcd.service
    _SYSTEMD_SLICE:  system.slice
    _SYSTEMD_UNIT:  etcd.service
    _TRANSPORT:  journal
    _UID:  997
    __source__:  172.16.1.4
    __tag__:__hostname__:  logtail-ds-8kqb9
    __topic__:  
    _monotonic_timestamp_:  1467135144311
    _realtime_timestamp_:  1546854068864309
    ```

-   Example 2

    To collect important fields from system logs of Kubernetes hosts in DaemonSet mode, you can use the following configuration file:

    ```
    {
      "inputs": [
        {
          "detail": {
            "JournalPaths": [
              "/logtail_host/var/log/journal"
            ],
            "ParsePriority": true,
            "ParseSyslogFacility": true
          },
          "type": "service_journal"
        }
      ],
      "processors": [
        {
          "detail": {
            "Exclude": {
              "UNIT": "^libcontainer.*test"
            }
          },
          "type": "processor_filter_regex"
        },
        {
          "detail": {
            "Include": [
              "MESSAGE",
              "PRIORITY",
              "_EXE",
              "_PID",
              "_SYSTEMD_UNIT",
              "_realtime_timestamp_",
              "_HOSTNAME",
              "UNIT",
              "SYSLOG_FACILITY",
              "SYSLOG_IDENTIFIER"
            ]
          },
          "type": "processor_pick_key"
        }
      ]
    }
    ```

    Sample log entry:

    ```
    MESSAGE:  rejected connection from "192.168.0.251:48914" (error "EOF", ServerName "")
    PRIORITY:  informational
    SYSLOG_IDENTIFIER:  etcd
    _EXE:  /opt/etcd-v3.3.8/etcd
    _HOSTNAME:  iZbp1i0czq3zgvxlx7u8ueZ
    _PID:  10590
    _SYSTEMD_UNIT:  etcd.service
    __source__:  172.16.0.141
    __tag__:__hostname__:  logtail-ds-dp48x
    __topic__:  
    _realtime_timestamp_:  1547975837008708
    ```


