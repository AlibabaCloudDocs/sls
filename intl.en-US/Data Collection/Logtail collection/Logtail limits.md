# Logtail limits

This topic describes the limits of Logtail. These limits apply when you collect files, manage checkpoints, manage resources, monitor performance metrics, and resolve errors.

|Item|Description|
|:---|:----------|
|File encoding|Log files encoded in UTF-8 and GBK are supported. We recommend that you use UTF-8 encoding for better processing performance. If log files are encoded in other formats, errors such as garbled characters and data loss may occur.|
|Log file size|Unlimited.|
|Log file rotation|Supported. Both `.log*` and `.log` are supported for file names.|
|Log collection behavior when log parsing is restricted|When log parsing is restricted, Logtail keeps the log file descriptor \(FD\) in the open state. If log file rotation occurs multiple times during the restriction, Logtail attempts to keep the log parsing sequence of each rotation. If the number of rotated logs to be parsed exceeds 20, Logtail does not process subsequent log files.|
|Symbolic link|Monitored directories can be symbolic links.|
|Size of a single log entry|The maximum size of a single log entry is 512 KB. If a multi-line log entry is divided by using a regular expression to match the first line, the maximum size of each log entry after division is still 512 KB. If the size of a log entry exceeds 512 KB, the log entry is forced to be separated into multiple parts for collection. For example, if the size of a log entry is 1,025 KB, it will be split into three parts: 512 KB, 512 KB, and 1 KB. These log parts are collected in sequence.|
|Regular expression|Perl-based regular expressions can be used.|
|Multiple Logtail configuration files for the same log file|Not supported. We recommend that you collect and store log files to one Logstore, and configure multiple subscriptions. If this feature is required, configure symbolic links for log files to bypass this limit.|
|File opening behavior|When Logtail collects a log file, Logtail opens the log file. If the log file is not modified for more than 5 minutes and log rotation does not occur, Logtail closes the log file.|
|First log collection behavior|Logtail collects only incremental log files. When a log file is modified for the first time and the log file size exceeds 1 MB, Logtail collects the logs from the last 1 MB. Otherwise, Logtail collects the logs from the beginning of the log file. If the log file is not modified after the Logtail configuration file is sent to the server where the log file resides, Logtail does not collect the log file.|
|Non-standard text logs|If a log entry contains \\0 lines, the log entry is truncated at the position of the first \\0 line.|

|Item|Description|
|:---|:----------|
|Checkpoint timeout period|If a log file is not modified for more than 30 days, the checkpoint of the log file is deleted.|
|Checkpoint storage policy|Checkpoints are saved every 15 minutes and are automatically saved when you exit Logtail.|
|Checkpoint storage path|By default, checkpoints are stored in the `/tmp/logtail_checkpoint` directory. For more information about how to modify the values of the related parameters, see [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md).|

|Item|Description|
|:---|:----------|
|Configuration update|A custom configuration update requires about 30 seconds to take effect.|
|Dynamic loading of Logtail configuration files|Supported. The update of a Logtail configuration file does not affect other Logtail configuration files.|
|Number of Logtail configuration files|Unlimited. However, we recommend that you create a maximum of 100 Logtail configuration files on a server.|
|Multi-tenant isolation|Logtail configuration files for different tenants are isolated.|

|Item|Description|
|:---|:----------|
|Throughput for log processing|The default traffic of raw logs is limited to 20 MB/s. Data is uploaded after it is encoded and compressed. The general compression ratio ranges from 5:1 to 10:1. Logs may be lost if the traffic exceeds the limit. For more information about how to modify the values of the related parameters, see [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md).|
|Maximum processing speed|Single-core processing speed: The maximum processing speed is 100 MB/s for logs in simple mode, 40 MB/s for logs in delimiter mode, and 30 MB/s for logs in JSON mode. By default, the maximum processing speed is 20 MB/s for logs in full regex mode based on the complexity of regular expressions. If multiple processing threads are enabled, the performance can be improved by 1.5 to 3 times.|
|Number of monitored directories|Logtail limits the depth of monitored directories to reduce the consumption of your resources. If the upper limit is reached, Logtail stops monitoring additional directories and log files. Logtail monitors a maximum of 3,000 directories, including subdirectories.|
|Number of monitored files|A Logtail configuration file on each server can be used to monitor a maximum of 10,000 files by default. A Logtail client on each server can monitor a maximum of 100,000 files by default. Excessive files are not monitored. If the upper limit is reached, you can perform the following operations:

-   Improve the depth of the monitored directory in each Logtail configuration file.
-   Modify the value of the mem\_usage\_limit parameter to increase the Logtail memory usage threshold. For more information, see [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md).

You can set a memory usage threshold of 2 GB for Logtail. In this case, the maximum number of files that each Logtail configuration file can be used to monitor is increased to 100,000. The maximum number of files that each Logtail client can monitor is increased to 1,000,000. |
|Default resources|By default, Logtail occupies up to 40% of CPU usage and 256 MB of memory. If logs are generated at a high speed, you can modify the values of the related parameters. For more information, see [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md).|
|Processing policy of threshold-crossing resources|If the resources occupied by Logtail exceed the upper limit and this issue lasts for five or more minutes, Logtail is forcibly restarted. The restart may cause data loss or duplication.|

|Item|Description|
|:---|:----------|
|Network error handling|If a network error occurs, Logtail retries and adjusts the retry interval.|
|Processing policy of threshold-crossing resources|If the data transmission speed exceeds the quota of the Logstore, Logtail restricts log collection speed and retries the log collection.|
|Maximum retry period before timeout|If data fails to be transmitted and the failure lasts for more than six successive hours, Logtail discards the data.|
|Status self-check|Logtail restarts if an exception occurs, for example, an application unexpectedly exits or the resource usage exceeds the quota.|

|Item|Description|
|:---|:----------|
|Log collection latency|A latency of less than 1 second exists between the time a log is written to a disk and the time when Logtail collects the log. However, if the log collection speed is restricted, the latency increases.|
|Log upload policy|Before Logtail uploads logs, it aggregates the logs in the same file. The log upload starts if the number of logs exceeds 2,000, the total size of logs exceeds 2 MB, or the log collection duration exceeds 3 seconds.|

