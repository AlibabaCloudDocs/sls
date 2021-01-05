# FAQ about Logtail

## What is Logtail?

Logtail is a log collection client provided by Log Service to facilitate your access to logs. After you install Logtail on the server from which you want to collect logs, Logtail listens to specified log files and uploads newly written logs to the specified Logstore.

## Can Logtail collect static logs?

No, Logtail cannot collect static logs. Logtail monitors the changes of log files based on whether log events are modified. If log events are modified, Logtail sends the generated logs to Log Service in real time. If log events are not modified, Logtail does not collect those logs.

## Which operating systems does Logtail support?

-   Linux

    The following versions of Linux x86 \(64-bit\) servers are supported:

    -   Alibaba Cloud Linux 2.1903
    -   Red Hat Enterprise Linux 6, Red Hat Enterprise Linux 7, and Red Hat Enterprise Linux 8
    -   CentOS 6, CentOS 7, and CentOS 8
    -   Debian GNU/Linux 8, Debian GNU/Linux 9, and Debian GNU/Linux 10
    -   Ubuntu 14.04, Ubuntu 16.04, Ubuntu 18.04, and Ubuntu 20.04
    -   SUSE Linux Enterprise Server 11, SUSE Linux Enterprise Server 12, and SUSE Linux Enterprise Server 15
    -   openSUSE Leap 15.1, openSUSE Leap 15.2, and openSUSE Leap 42.3
    -   Glibc 2.5 or later
-   Windows

    Logtail supports x86 and x86\_64 for Microsoft Windows Server 2008 and Microsoft Windows 7. Logtail supports only x86\_64 for other Windows operating systems.

    -   Microsoft Windows Server 2008
    -   Microsoft Windows Server 2012
    -   Microsoft Windows Server 2016
    -   Microsoft Windows Server 2019
    -   Microsoft Windows 7
    -   Microsoft Windows 10
    -   Microsoft Windows Server Version 1909
    -   Microsoft Windows Server Version 2004

## How do I install and upgrade Logtail?

-   Install Logtail: For more information, see [Install Logtail on ECS instances](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail on ECS instances.md), [Install Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md), or [Install Logtail in Windows](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md).
-   Upgrade Logtail: For more information, see [Upgrade Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md#section_jcz_xmv_vdb) or [Upgrade Logtail in Windows](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md#section_fks_lwg_cfb).

    **Note:** You can only manually upgrade Logtail if it is running.


## How do I configure Logtail to collect logs?

Log Service allows you to collect text logs and container logs by using Logtail. You can also collect logs by using Logtail plug-ins. For more information, see the following topics:

-   [Collect text logs](/intl.en-US/Data Collection/Logtail collection/Text logs/Overview.md)
-   [Collect container logs](/intl.en-US/Data Collection/Logtail collection/Container log collection/Overview.md)
-   [Collect logs by using Logtail plug-ins](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Overview.md)

## How does Logtail collect logs?

Logtail collects logs by using a process that includes log monitoring, reading, processing, filtering, aggregating, and uploading. For more information, see [Log collection process of Logtail](/intl.en-US/Data Collection/Logtail collection/Overview/Log collection process of Logtail.md).

## Does Logtail support log rotation?

Yes, Logtail supports log rotation. For example, assume that the app.LOG file generates app.LOG.1 and app.LOG.2 during log rotation. Logtail automatically detects the LOG rotation process and ensures that no logs are lost during this process.

## How does Logtail handle network exceptions?

If a network exception occurs or the write quota is reached, Logtail caches the collected logs to the local disk and retries later.

**Note:**

-   The maximum disk cache size is 500 MB. The new disk cache overwrites the old disk cache.
-   Cached files that have not been sent for more than 24 hours will be automatically deleted.

## What is the latency of log collection when Logtail is used?

Logtail collects logs based on the modification of log events and sends the newly generated logs to Log Service within 3 seconds.

## How do I collect historical logs?

If the interval from the time when a log is generated to the system time when Logtail processes the log exceeds 5 minutes, the log is considered as a historical log. By default, Logtail collects only incremental logs. To collect historical logs, you can use the historical log import feature that is provided by Logtail. For more information, see [Import historical logs](/intl.en-US/Data Collection/Logtail collection/Text logs/Import historical logs.md).

## When does a Logtail configuration file take effect after I modify it?

After you modify a Logtail configuration file in the Log Service console, the new configurations take effect within 3 minutes.

## How do I resolve issues that occur when Logtail collects logs?

You can use three methods to resolve log collection issues. For more information, see [Troubleshoot collection errors]().

1.  Check whether the heartbeat status of Logtail is OK.
2.  Check whether the logs in the specified log files are generated in real time.
3.  Check whether the regular expression in the Logtail configuration file matches the log content.

