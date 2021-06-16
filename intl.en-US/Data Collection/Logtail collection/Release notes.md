# Release notes

This topic describes the release notes of Log Service Logtail.

## 0.16.64

-   Optimized features

    The timeout period for requesting a container engine is increased from 3 seconds to 30 seconds. The DOCKER\_CLIENT\_REQUEST\_TIMEOUT environment variable is added to specify the timeout period for requesting a container engine.

-   Fixed issues
    -   The ID of a parent span in the service\_skywalking plug-in has errors. This issue is fixed.
    -   The code used to create a Logtail configuration file based on environment variables may stop if the related container engine has an exception. This issue is fixed.

## 0.16.62

**Note:** If you are using Logtail V0.16.58 or Logtail V0.16.60, we recommend that you upgrade Logtail to V0.16.62.

-   Fixed issues

    Data may fail to be sent due to out-of-order data. This issue is fixed.


## 0.16.60

-   New features

    Container data in the containerd environment can be collected.


## 0.16.56

-   Optimized features

    The net\_err\_stat metric in service logs applies to sending errors that are caused only by network issues.


## 0.16.54

-   New features

    The net\_err\_stat metric is added in service logs. The number of sending errors that occur in the last 1, 5, or 15 minutes can be recorded.


## 0.16.52

If a large number of Logtail configuration files are created to collect container data \(standard outputs or container files\), we recommend that you upgrade Logtail to V0.16.52 or later. This helps you reduce the CPU overhead.

-   Optimized features

    The CPU overhead during container data collection is optimized.


## 0.16.50

-   New features

    The service\_telegraf plug-in can be installed as required at runtime. This feature is applicable only to users who use Elastic Compute Service \(ECS\).


## 0.16.48

-   Optimized features

    The service\_telegraf plug-in is optimized. Multiple configuration files are supported on a single machine.


## 0.16.46

**Note:** If the endpoint of Log Service is in the China \(Hangzhou\), China \(Shanghai\), or China \(Beijing\) region, you can upgrade Logtail to V0.16.46 or later. This prevents Logtail from switching endpoints when network jitter occurs.

-   Optimized features

    The network types that can be used in Logtail are strictly limited.


## 0.16.44

-   New features

    The service\_telegraf plug-in is added to collect metric data.


## 0.16.42

-   New features

    Multi-level matching is supported when you use a blacklist to filter data. Example: /path/\*\*/log.

-   Optimized features

    The policy used to obtain local IP addresses is optimized. If a policy is invalid, the first IP address in the IP address list is obtained.


## 0.16.40

-   New features
    -   The metric\_system\_v2 plug-in is added to collect the data of host statuses.
    -   The max\_docker\_config\_update\_times parameter is added in the ALIYUN\_LOGTAIL\_MAX\_DOCKER\_CONFIG\_UPDATE\_TIMES environment variable. The added parameter is applicable to scenarios where you need to frequently create short-lived jobs in a Kubernetes environment.
-   Optimized features

    The performance of the CPU overhead is optimized for scenarios where a large number of Logtail configuration files are created to collect container data.

-   Fixed issues

    The issue that the processor\_split\_log\_string plug-in generates blank rows is fixed.


## 0.16.38

-   New features
    -   A custom name of the time field is supported in full regex mode.
    -   The KeepSourceIfParseError parameter is added in the processor\_json, processor\_regex, and processor\_split\_char plug-ins. If raw data fails to be parsed, the raw data can be retained. For more information, see [Customize Logtail plug-ins to process data](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Overview.md).

## 0.16.36

-   New features

    The processor\_encrypt plug-in is added for encryption.


## 0.16.34

-   New features

    An HTTP probe is added for ‎Kubernetes health check.

-   Fixed issues
    -   Core dumps caused by libcurl in some environments are fixed.
    -   The issue that GNU Libidn is missing when you install Logtail in CentOS 8 is fixed.

## 0.16.32

-   New features

    The IgnoreFirstConnector parameter is added in the processor\_json plug-in. For more information, see [Expand JSON fields](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Expand JSON fields.md).


## 0.16.30

**Note:** If you run Logtail V0.16.30 for a long period of time, files may fail to be opened. We recommend that you update Logtail to the latest version.

-   New features

    Kubernetes clusters can be filtered by using whitelists or blacklists when you collect Docker standard outputs or files from the clusters.

-   Optimized features

    The concurrency competition among Logstores in the same region due to poor network connections is optimized.

-   Fixed issues

    The checkpoint loss issue that may be caused by logic errors when a file is opened is fixed.


## 0.16.28

-   New features

    A parameter is added to specify a tail size for the data that is collected for the first time.

-   Optimized features

    The logic of container metadata collection is optimized to reduce the impact of faulty containers.

-   Fixed issues
    -   The issue that results in memory leaks in complex environments is fixed for docker\_stdout.
    -   The issue that complete millisecond timestamps are not supported in JSON mode is fixed.

## 0.16.26

-   New features

    Containerd logs can be collected.

-   Fixed issues
    -   The issue that results in checkpoint loss of rotated files is fixed.
    -   The issue that the on-premises Logtail configuration file /etc/ilogtail/user\_config.d is not loaded when the /usr/local/ilogtail/user\_log\_config.json file does not exist is fixed.

## 0.16.24

-   New features
    -   The working\_ip and working\_hostname parameters can be set by using environment variables.
    -   The force\_quit\_read\_timeout parameter is added to specify the timeout period before you forcibly quit Logtail if events are blocked and fail to be read.
    -   Tags such as path and topic can be passed to plug-ins.
    -   The aggregator\_shardhash plug-in is added and the shardhash parameter can be set in the plug-in.
    -   The processor\_gotime, processor\_rename, processor\_add\_fields, processor\_json, and processor\_packjson plug-ins are added for data processing. For more information, see [Customize Logtail plug-ins to process data](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Overview.md).
    -   LogtailInsight is updated. The progress viewing feature is added. This feature can be used only after you set the mark\_offset\_global\_flag or customized\_fields.mark\_offset parameter.
-   Optimized features
    -   The issue that the memory usage is high when the Journal plug-in runs for a long period of time is fixed. The memory is released at the earliest opportunity.
    -   The interval at which configurations are obtained when no on-premises configuration files exist is optimized.
-   Fixed issues
    -   The issue that duplicate data is collected because multiple Logtail configuration files exist is fixed.
    -   The issue that millisecond and microsecond timestamps do not support JSON int64 is fixed.

## 0.16.23

-   New features
    -   Millisecond and microsecond timestamps \(%s\) are supported.
    -   Multiple on-premises Logtail configuration files can be loaded. File path: /etc/ilogtail/config.d/.
    -   Multiple on-premises user configuration files can be loaded. File path: /etc/ilogtail/user\_config.d/.
    -   The processor\_split\_key\_value and processor\_strptime plug-ins are added for data processing. For more information, see [Extract log fields by splitting key-value pairs](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Extract fields.md) and [Extract log time](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Extract log time.md).
    -   The oas\_connect\_timeout and oas\_request\_timeout parameters are added to support the scenarios of slow networks.
-   Optimized features

    The limits on the inputs parameter in hybrid configurations \(file + plug-in\) are removed.


## 0.16.21

-   New features
    -   Custom static themes can be configured.
    -   The blacklist filtering feature is supported.
    -   The EnableEventMeta parameter is added in the service\_canal plug-in to allow you to collect the header information that corresponds to MySQL binary logs.
-   Optimized features

    The stop mechanism of plug-ins is optimized.

-   Fixed issues

    The issue that results in the potential memory leak of GBK logs is fixed.


## 0.16.18

-   New features
    -   Docker events can be collected. For more information, see [Collect Docker events](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Collect Docker events.md).
    -   Systemd-journald logs can be collected. For more information, see [Collect systemd-journald logs](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Collect systemd-journald logs.md).
    -   The processor\_pick\_key and processor\_drop\_last\_key plug-ins are added for data processing.
-   Optimized features
    -   The memory usage for container logs and plug-ins is optimized.
    -   The performance of multi-line log collection is optimized for container standard outputs.

## 0.16.16

-   New features
    -   The resources of Kubernetes audit logs can be automatically generated.
    -   The startup parameters can be set by using environment variables. The startup parameters include CPU, memory, and sending concurrency.
    -   The automatic upload of custom tags can be configured by using environment variables.
    -   Configuration files can be automatically created in sidecar mode. For more information, see [Use CRDs to collect Kubernetes container logs in the Sidecar mode](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use CRDs to collect Kubernetes container logs in the Sidecar mode.md).
-   Optimized features

    AliUid files can be automatically saved to local files.

-   Fixed issues
    -   The issue that crashes may occur when Logtail collects Docker files is fixed.
    -   The issue that the `IncludeLabel` parameter of a configuration file created by using environment variables does not take effect in Kubernetes is fixed.

## 0.16.15

-   New features
    -   The global transaction identifier \(GTID\) mode can be enabled for MySQL binary logs. If a MySQL server supports the GTID mode, this mode is automatically enabled.
    -   Specified wildcards are supported for file names when you import historical data.
    -   Indexes can be automatically generated for Kubernetes logs.
-   Optimized features
    -   The `discardUnMatch` parameter can be checked and logs that fail to be split into multiple lines are reported.
    -   Logtail retries to collect data if an unknown sending error occurs. This prevents data loss in some cases, such as when a packet is modified during the sending process.

## 0.16.14

-   New features
    -   The wildcard mode is supported when you import historical data.
    -   The `default_tail_limit_kb` startup parameter is added to specify the size of the data collected before a specified checkpoint if you collect files for the first time. The default value is 1024 KB.
    -   The `batch_send_seconds` parameter is added to specify the interval at which data packets are sent during log collection.
    -   The `batch_send_bytes` parameter is added to specify the size of the data packet to be sent during log collection.
-   Optimized features

    The logs that are split by Docker Engine can be automatically merged when Logtail collects container standard outputs.


## 0.16.13

New features

-   Log collection can be configured by using environment variables.
-   The metadata of MySQL binary logs can be collected, such as the `_filename_` and `_offset_` fields.
-   The parameters in the installation script can be automatically selected in a virtual private cloud \(VPC\).
-   The global acceleration feature can be configured when Logtail is installed For more information, see [Configure log collection acceleration for Logtail](/intl.en-US/Data Collection/Collection acceleration/Configure log collection acceleration for Logtail.md).

## 0.16.11

-   Optimized features

    The collected filename and offset information can be added to collect MySQL binary logs.

-   Fixed issues

    The issue that results in exceptions when you collect container standard outputs in multi-line mode is fixed.


## 0.16.10

-   Optimized features

    The method used to collect container standard outputs is upgraded.


## 0.16.9

-   Fixed issues
    -   The issue that results in the leak of socket fd is fixed.
    -   The limits on the update frequency of Logtail configuration files are added for container files.

## 0.16.8

-   New features
    -   The input source Lumberjack protocol is added to collect data from Logstash and Beats. For more information, see [Collect data from Beats and Logstash](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Collect data from Beats and Logstash.md).
    -   The inotify blacklist feature is added.
-   Fixed issues
    -   The inconsistent parameters in earlier installation packages are revised.
    -   The issue that the operating system version cannot be correctly obtained when you install Logtail in some systems is fixed.

## 0.16.6

-   New features
    -   Host monitoring data can be collected.
    -   Redis monitoring data can be collected.
    -   Data definition language \(DDL\) statements in MySQL binary logs can be collected.
    -   Container standard outputs and files can be filtered by using Docker environment variables.
-   Fixed issues
    -   Data can be collected from a MySQL table that does not have a primary key.
    -   The issue that the event loss is caused due to unstable subscription channels of container engines in container collection mode is fixed.

## 0.16.5

-   New features

    The multi-line mode is supported to collect container standard outputs. For more information, see [Configuration examples of multi-line log collection](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use the console to collect Kubernetes stdout and stderr logs in DaemonSet mode.mdsection_gf2_p8y_qsi).


## 0.16.4

-   New features
    -   The deployment scenarios of Docker and Kubernetes are supported.
    -   Container standard outputs and container files can be collected. For more information, see [Use the console to collect Kubernetes stdout and stderr logs in DaemonSet mode](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use the console to collect Kubernetes stdout and stderr logs in DaemonSet mode.md) and [Use the console to collect Kubernetes text logs in DaemonSet mode](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use the console to collect Kubernetes text logs in DaemonSet mode.md).

## 0.16.2

-   New features

    The processor\_geoip plug-in is added. For more information, see [Transform IP addresses](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Transform IP addresses.md).


## 0.16.0

-   New features
    -   MySQL binary logs, MySQL query results, and HTTP input sources are supported. For more information, see [Customize Logtail plug-ins to process data](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Overview.md).
    -   Data analysis can be performed by combining regular expressions, anchors, delimiters, and filters.

