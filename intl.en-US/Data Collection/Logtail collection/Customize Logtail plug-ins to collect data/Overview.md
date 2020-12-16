# Overview

This topic describes Logtail plug-ins and their configuration methods. The plug-ins are used for data collection.

## Background information

**Note:** Logtail plug-ins do not support Linux kernel versions that are earlier than 2.6.32.

You can customize plug-ins for Logtail to collect various types of data. For example:

-   You can configure Logtail plug-ins to collect HTTP data and upload processed data to Log Service. This way, you can monitor the availability of your services in real time.
-   You can configure Logtail plug-ins to collect the data of MySQL databases. You can synchronize incremental data based on the auto-increment ID of the database or the time when the data is generated.
-   You can configure Logtail plug-ins to collect the binary logs of MySQL databases, and search and analyze binary logs in real time.

## Configuration procedure

![Configuration procedure](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0007549951/p2926.png)

1.  Configure a collection method.

    For information about how to configure collection methods for various data sources, see the following topics:

    -   [Collect MySQL binary logs](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Collect MySQL binary logs.md)
    -   [Collect MySQL query results](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Collect MySQL query results.md)
    -   [Collect HTTP data](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Collect HTTP data.md)
    -   [Collect container standard outputs](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use the console to collect Kubernetes stdout and stderr logs in DaemonSet mode.md)
    -   [Collect data from Beats and Logstash](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Collect data from Beats and Logstash.md)
    -   [Collect syslog logs](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Collect syslog logs.md)
    -   [Collect Windows event logs](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Collect Windows event logs.md)
    -   [Collect Docker events](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Collect Docker events.md)
    -   [Collect systemd-journald logs](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Collect systemd-journald logs.md)
2.  Configure a data processing method.

    You can configure multiple processing methods for a data source. Logtail processes data by using these methods in sequence. You can configure all available processing methods for each type of data source. For more information, see [Process data](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Process data.md).

3.  Apply the configurations to the specified machine group.

    Apply the log collection configurations and processing configurations to the specified machine group. Then, Logtail automatically pulls the configurations and starts to collect logs.


