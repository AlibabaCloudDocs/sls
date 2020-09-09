# Logtail limits

|Item|Capability and limit|
|:---|:-------------------|
|File encoding|Log files encoded in UTF-8 and GBK are supported. We recommend that you use UTF-8 encoding for better processing performance. If log files are encoded in other formats, errors such as garbled characters and data losses may occur.|
|Log file size|Unlimited.|
|Log file rotation|Supported. Both `.log*` and `.log` are supported for file names.|
|Log collection behavior when log parsing is blocked|When log parsing is blocked, Logtail keeps the log file descriptor \(FD\) in the open state. If log file rotation occurs multiple times during the block, Logtail attempts to keep the log parsing sequence of each rotation. If more than 20 unparsed logs are rotated, Logtail does not process subsequent log files. For more information, see the related technical document.|
|Symbolic link|Monitored directories can be symbolic links.|
|Single log size|The maximum size of a single log is 512 KB. If cross-line logs are divided by the regular expression for specifying the starting header of a cross-line log, the maximum size of each log is still 512 KB. If the size of a log exceeds 512 KB, the log is forcibly split into multiple parts for collection. For example, if the size of a log is 1,025 KB, the first 512 KB, the subsequent 512 KB, and the last 1 KB are processed sequentially.|
|Regular expression|Regular expressions can be Perl-compatible regular expressions.|
|Multiple collection configurations for the same file|Not supported. We recommend that you collect only one copy of log files to a Logstore and configure multiple subscriptions. If you need to collect multiple copies of log files, configure symbolic links for log files to bypass this limit.|
|File opening behavior|Logtail keeps a file to be collected in the open state. Logtail closes the file if the file is not modified within 5 minutes \(when rotation does not occur\).|
|First log collection behavior|Logtail collects only incremental log files. If modifications are found in a file for the first time and the file size exceeds 1 MB, Logtail collects the logs from the last 1 MB. Otherwise, Logtail collects the logs from the beginning. If a log file is not modified after the configuration is issued, Logtail does not collect this file.|
|Non-standard text log|If a log contains the '\\0' lines, the log is truncated at the position of the first '\\0' line.|

|Item|Capability and limit|
|:---|:-------------------|
|Checkpoint timeout period|If a file is not modified for more than 30 days, the checkpoint is deleted.|
|Checkpoint storage policy|Checkpoints are regularly saved \(every 15 minutes\) and are automatically saved when you exit the application.|
|Checkpoint storage path|By default, checkpoints are stored in the `/tmp/logtail_checkpoint` directory. For more information about how to modify the directory, see [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md).|

|Item|Capability and limit|
|:---|:-------------------|
|Configuration update|Your updated configuration takes effect with a latency of about 30 seconds.|
|Dynamic configuration loading|Supported. The update of a Logtail configuration does not affect other Logtail configurations.|
|Number of configurations|Theoretically unlimited. We recommend that you create a maximum of 100 collection configurations on a server.|
|Multi-tenant data segregation|Collection configurations for different tenants are isolated.|

|Item|Capability and limit|
|:---|:-------------------|
|Log processing throughput|The default traffic of raw logs is limited to 2 Mbit/s. \(Data is uploaded after it is encoded and compressed, with a general compression ratio of 5 to 10 times.\) Logs may be lost if the log traffic exceeds the limit. For more information about how to modify the parameters, see [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md) .|
|Maximum performance|Single-core capability: The maximum processing capability is 100 Mbit/s for logs in simple mode, 20 Mbit/s by default for logs in full regex mode \(depending on the complexity of regular expressions\), 40 Mbit/s for logs in delimiter mode, and 30 Mbit/s for logs in JSON mode. After multiple processing threads are enabled, the performance can be improved by 1.5 to 3 times.|
|Number of monitored directories|Logtail limits the depth of monitored directories to conserve your resources. If the upper limit is reached, Logtail stops monitoring more directories and log files. Logtail monitors a maximum of 3,000 directories \(including subdirectories\).|
|Number of monitored files|A Logtail configuration on each server monitors a maximum of 10,000 files, and the Logtail client on each server monitors a maximum of 100,000 files. Excessive files are not monitored.When the upper limit is reached, you can:

-   Improve the depth of the monitored directory in each Logtail configuration.
-   Modify the value of the mem\_usage\_limit parameter to increase the Logtail memory usage threshold. For more information, see [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md).

The Logtail memory usage threshold can be set to a maximum of 2 GB, indicating that each Logtail configuration can monitor a maximum of 100,000 files, and each Logtail client can monitor a maximum of 1,000,000 files. |
|Default resources|By default, Logtail occupies up to 40% of CPU usage and 256 MB of memory. If logs are generated at a high speed, you can modify relevant parameters. For more information, see [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md).|
|Resource out-of-limit processing policy|If the resources occupied by Logtail within 3 minutes exceed the upper limit, Logtail is forcibly restarted, which may cause data loss or duplication.|

|Item|Capability and limit|
|:---|:-------------------|
|Network error handling|If the network is disconnected, Logtail retries and automatically adjusts the retry interval.|
|Resource quota out-of-limit processing|If the data transmission rate exceeds the quota of the Logstore, Logtail blocks log collection and automatically retries.|
|Maximum retry period before timeout|If data transmission fails for more than 6 successive hours, Logtail discards the data.|
|Status self-check|Logtail automatically restarts if an exception occurs, for example, an application unexpectedly exits or resource usage exceeds the quota.|

|Item|Capability and limit|
|:---|:-------------------|
|Log collection latency|Except for the block state, the latency in log collection by Logtail does not exceed 1 second after logs are flushed to a disk.|
|Log upload policy|Logtail automatically aggregates logs in the same file before uploading the logs. Logs are uploaded if the number of logs exceeds 2,000, the total size of the log file exceeds 2 MB, or the log collection duration exceeds 3 seconds.|

