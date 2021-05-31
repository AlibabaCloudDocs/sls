# Log collection error types

On the Logstores page, you can click **Diagnose** of a Logstore to view all log collection errors about it. This topic describes the specific error types and handling methods.

If you encounter an error not mentioned in this topic, you can [open a ticket](https://selfservice.console.aliyun.com/ticket/category/sls/today)open a ticket and submit error details.

|Error type|Description|Handling method|
|:---------|:----------|:--------------|
|LOGFILE\_PERMINSSION\_ALARM|Logtail has no permission to read the specified file.|Check the Logtail startup account on the server. We recommend that you start Logtail as the root user.|
|SPLIT\_LOG\_FAIL\_ALARM|The line start regular expression cannot match line starts of the log, and the log cannot be split into lines.|Check the correctness of the line start regular expression. If the log contains only one line, you can set the line start regular expression to `.*`.|
|MULTI\_CONFIG\_MATCH\_ALARM|Each file can only be collected by one Logtail Config.|Check whether a file is collected in multiple Configs. If yes, delete unnecessary Configs.|
|REGEX\_MATCH\_ALARM|The log content does not match the regular expression upon regular expression parsing.|Copy a part from the unmatched content as a sample for a rematch, and then generate a new regular expression.|
|PARSE\_LOG\_FAIL\_ALARM|Log parsing fails because the formats of JSON and delimiter logs do not conform to format definitions.|Click the error message to view details.|
|CATEGORY\_CONFIG\_ALARM|The Logtail Config is invalid.|We recommend that you modify the regular expression because generally the process of extracting a file path as the Topic through the regular expression fails. If the error is due to another reason, open a ticket and submit error details.|
|LOGTAIL\_CRASH\_ALARM|Logtail cannot respond because its server resource usage has exceeded the upper limit.|Modify the upper limits of the CPU usage and memory usage by following the instructions provided in [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md). You can also open a ticket for additional support.|
|REGISTER\_INOTIFY\_FAIL\_ALARM|Registering log monitoring has failed on Linux. A possible cause is that Logtail does not have the permission to access the folder or the folder has been deleted.|Check whether Logtail has the permission to access the folder and whether the folder has been deleted.|
|DISCARD\_DATA\_ALARM|The CPU resources for configuring Logtail are insufficient or the incoming traffic to Log Service is restricted.|Modify the upper limit of the CPU usage or limits on concurrent incoming traffic to Log Service by following the instructions provided in [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md). You can also open a ticket for additional support.|
|SEND\_DATA\_FAIL\_ALARM|-   The Alibaba Cloud account have not created any AccessKey \(AK\).
-   The Logtail client cannot connect to the Log Service server, or the connection quality is poor.
-   The writing quota on the server is insufficient.

|-   Use the Alibaba Cloud account to create an AK.
-   Check the local Config file /usr/local/ilogtail/ilogtail\_config.json and run `curl<endpoint>` to check whether any result is returned.
-   Increase the number of Shards for the Logstore so that more data can be written to the Logstore. |
|REGISTER\_INOTIFY\_FAIL\_ALARM|Logtail fails to register inotify watcher for the log directory.|Check whether the directory exists. If yes, check the directory permissions.|
|SEND\_QUOTA\_EXCEED\_ALARM|The log writing amount exceeds the limit.|[Expand the Shard capacity](/intl.en-US/Product Introduction/Basic concepts/Shards.md) in the console.|
|READ\_LOG\_DELAY\_ALARM|Log collection lags behind log generation. In normal cases, this is because the CPU resources for configuring Logtail are insufficient or the incoming traffic to Log Service is restricted.|Modify the upper limit of the CPU usage or limits on concurrent incoming traffic to Log Service by following the instructions provided in [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md). You can also open a ticket for additional support.|
|DROP\_LOG\_ALARM|Log collection lags behind log generation, and unprocessed log rotations outnumbers 20. In normal cases, this is because the CPU resources for configuring Logtail are insufficient or the incoming traffic to Log Service is restricted.|Modify the upper limit of the CPU usage or limits on concurrent incoming traffic to Log Service by following the instructions provided in [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md). You can also open a ticket for additional support.|
|LOGDIR\_PERMINSSION\_ALARM|Logtail has no permission to read the log monitoring directory.|Check whether the log monitoring directory exists. If yes, check the directory permissions.|
|ENCODING\_CONVERT\_ALARM|Code conversion fails.|Check whether the log encoding format conforms to the specified format.|
|OUTDATED\_LOG\_ALARM|The log is outdated because the time when Logtail received the log has exceeded more than 12 hour and the log is expired. Possible causes are as follows: -   Log parsing is more than 12 hours behind schedule.
-   Custom time fields are incorrect.
-   The time of the log recording program is incorrect.

|-   Check whether READ\_LOG\_DELAY\_ALARM exists. If yes, use the method of handling READ\_LOG\_DELAY\_ALARM to handle this error. If no, check the time filed settings.
-   Check the time filed settings. If the time field settings are correct, check whether the time of the log recording program is correct.

 You can also open a ticket for additional support.|
|STAT\_LIMIT\_ALARM|The number of files in the Logtail Config directory exceeds the upper limit.|Check whether the Logtail Config directory contains an excessive number of files and subdirectories. If yes, configure the root monitoring directory and the maximum directory monitoring depth as needed.|
|DROP\_DATA\_ALARM|When the log collection process exits, writing logs to the local disk expires. In this case, the logs that have not been written to the local disk will be discarded.|Modify the upper limit of the CPU usage or limits on concurrent incoming traffic to Log Service by following the instructions provided in [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md). Generally, the error is caused by severe collection blocks. You can also open a ticket for additional support.|
|INPUT\_COLLECT\_ALARM|An error occurs during input source collection.|Handle the error according to the error message.|
|HTTP\_LOAD\_ADDRESS\_ALARM|The address in the HTTP input source is invalid.|Check the validity of the address.|
|HTTP\_COLLECT\_ALARM|An error occurs during HTTP input source collection.|Handle the error according to the error message. In normal cases, the error is caused by expiration.|
|FILTER\_INIT\_ALARM|An error occurs during filer initialization.|Handle the error according to the error message. In normal cases, the error is caused by invalid filter regular expressions.|
|INPUT\_CANAL\_ALARM|An error occurs when MySQL binlogs run.|Handle the error according to the error message. The canal service may restart when Logtail Config is updated. Errors caused by service restart can be ignored.|
|CANAL\_INVALID\_ALARM|The internal state of MySQL binlogs is abnormal.|Check whether the table schema is being modified when the error occurs. This error generally occurs when meta data changes are caused by table schema modifications during binlog running. Open a tick if the cause is another one.|
|MYSQL\_INIT\_ALARM|An error occurs during MySQL initialization.|Handle the error according to the error message.|
|MYSQL\_CHECKPOING\_ALARM|The MySQL checkpoint format is incorrect.|Check whether to modify the checkpoint settings in the current Logtail Config. If the error persists after you modify the checkpoint settings, open a ticket and submit error details.|
|MYSQL\_TIMEOUT\_ALARM|The MySQL query expires.|Check whether the error is caused by MySQL server faults or abnormal network status.|
|MYSQL\_PARSE\_ALARM|Parsing MySQL query results fails.|Check whether the checkpoint format configured in MySQL matches the format of the corresponding field.|
|AGGREGATOR\_ADD\_ALARM|Logtail fails to add data to the queue.|Ignore the error if the actual data amount is large because the error is caused by excessive fast data sending.|
|ANCHOR\_FIND\_ALARM|Possible error causes are anchor plug-in faults, Config faults, or mismatch between the Config and log.|Click the error message to view details, which may contain the following error types. Check whether the corresponding Config encounters faults accordingly. -   anchor cannot find key: The SourceKey is specified in the Config but its corresponding field cannot be found in the log.
-   anchor no start: The keywords specified by Start cannot be found in the value of SourceKey.
-   anchor no stop: The keywords specified by Stop cannot be found in the value of SourceKey. |
|ANCHOR\_JSON\_ALARM|An error occurs when the anchor plug-in performs JSON expansion on the keywords specified by Start and Stop.|Click the error message to view details. Check the keywords and the related Config. Check whether there is any Config fault or invalid log.|
|CANAL\_RUNTIME\_ALARM|An error occurs when the binlog plug-in runs.|Click the error message to view details, and then handle the error accordingly. The error is related to the connected MySQL master database.|
|CHECKPOINT\_INVALID\_ALARM|The plug-in fails to parse the checkpoint.|Click the error message to view details, and then handle the error according to the checkpoint key, checkpoint content \(the first 1,024 bytes\), and other information.|
|DIR\_EXCEED\_LIMIT\_ALARM|The number of directories for simultaneous monitoring exceeds the upper limit.|Check whether the Config of the current Logstore and other Configs applied on Logtail contain excessive directories. If yes, configure the root monitoring directory and the maximum directory monitoring depth as needed.|
|DOCKER\_FILE\_MAPPING\_ALARM|Logtail fails to add Docker file mapping by executing commands.|Click the error message to view details, and then handle the error accordingly.|
|DOCKER\_FILE\_MATCH\_ALARM|The specified file cannot be found in Docker.|Click the error message to view details, and then handle the error according to the container information and file path.|
|DOCKER\_REGEX\_COMPILE\_ALARM|The docker stdout plug-in fails to construct a regular expression based on BeginLineRegex in the Config.|Click the error message to view details, and then check whether the regular expression is correct.|
|DOCKER\_STDOUT\_INIT\_ALARM|The docker stdout collection initialization fails.|Click the error message to view details, which may contain the following error types: -   host... version... error: Check whether the Docker engine specified in the Config is accessible.
-   load checkpoint error: Ignore the error if there is no impact because the error is caused by checkpoint loading failure.
-   container...: Set either stdout or stderr as a label. The error is caused because the specified container has an invalid label value. Handle the error according to the error details. |
|DOCKER\_STDOUT\_START\_ALARM|The stdout file size exceeds the upper limit during docker stdout collection initialization.|Ignore the error because, in normal cases, the stdout file already exists at the first collection.|
|DOCKER\_STDOUT\_STAT\_ALARM|The docker stdout plug-in cannot check the stdout file.|Ignore the error because the container cannot access the stdout file after the container exits.|
|FILE\_READER\_EXCEED\_ALARM|The number of objects opened by Logtail exceeds the upper limit.|Check whether the Config settings are appropriate because the error is caused by excessive files being collected.|
|GEOIP\_ALARM|The geoip plug-in is faulty.|Click the error message to view details, which may contain the following error types: -   invalid ip...: The plug-in fails to obtain the IP address. Check whether SourceKey in the Config is correct or whether an invalid log exists.
-   parse ip...: The plug-in fails to parse the city information based on the obtained IP address. Handle the error according to the error details.
-   cannot find key...: The plug-in cannot find the specified SourceKey from the log. Check whether the Config is faulty or whether an invalid log exists. |
|HTTP\_INIT\_ALARM|The http plug-in incorrectly compiles the ResponseStringMatch regular expression specified in the Config.|Click the error message to view details, and then check whether the regular expression is correct.|
|HTTP\_PARSE\_ALARM|The http fails to receive HTTP responses.|Click the error message to view details, and then check the Config or the requested HTTP server.|
|INIT\_CHECKPOINT\_ALARM|The binlog plug-in fails to load the checkpoint. In this case, the plug-in will ignore the checkpoint and recollect the log.|Click the error message to view details, and then determine whether the error can be ignored.|
|LOAD\_LOCAL\_EVENT\_ALARM|Logtail handles a local event.|Ignore the error if it is caused by manual operations. For other cases, open a ticket and submit error details. Click the error message to view details, and then handle the error according to the file name, Config name, project, Logstore, and other information.|
|LOG\_REGEX\_FIND\_ALARM|The processor\_split\_log\_regex and processor\_split\_log\_string plug-ins cannot obtain the SplitKey specified by the Config from the log.|Click the error message to view details, and then check whether the Config is faulty.|
|LUMBER\_CONNECTION\_ALARM|The server cannot be powered off when the service\_lumberjack plug-in is stopped.|Click the error message to view details, and then handle the error accordingly. In normal cases, this error can be ignored.|
|LUMBER\_LISTEN\_ALARM|An error occurs when the service\_lumberjack plug-in is being initiated for log monitoring.|Click the error message to view details, which may contain the following error types: -   init tls error...: Check whether the TLS configurations are correct.
-   listen init error...: Check whether the address-related settings are correct. |
|LZ4\_COMPRESS\_FAIL\_ALARM|An error occurs when Logtail executes LZ4 compression.|Click the error message to view details, and then handle the error according to the values of log lines, project, category, and region.|
|MYSQL\_CHECKPOINT\_ALARM|The MySQL plug encounters a checkpoint error.|Click the error message to view details, which may contain the following error types: -   init checkpoint error...: Initializing the checkpoint fails. In this case, check whether the checkpoint column specified by the Config and the corresponding values are correct.
-   not matched checkpoint...: The checkpoint information does not match. In this case, check whether the mismatch is caused by manual operations, for example, Config updates. If yes, ignore the error. |
|NGINX\_STATUS\_COLLECT\_ALARM|An error occurs when the nginx\_status plug-in obtains the server status.|Click the error message to view details, and then handle the error according to the URL and other information.|
|NGINX\_STATUS\_INIT\_ALARM|The nginx\_status plug-in fails to initiate and parse the URL specified by the Config.|Click the error message to view details, and then check whether the address is correct according to the URL.|
|OPEN\_FILE\_LIMIT\_ALARM|Logtail cannot open new files because the number of opened files has exceeded the upper limit.|Click the error message to view details, and then handle the error according to the log file path, project, Logstore, and other information.|
|OPEN\_LOGFILE\_FAIL\_ALARM|An error occurs when Logtail opens a file.|Click the error message to view details, and then handle the error according to the log file path, project, Logstore, and other information.|
|PARSE\_DOCKER\_LINE\_ALARM|The service\_docker\_stdout plug-in fails to parse the log.|Click the error message to view details, which may contain the following error types: -   parse docker line error: empty line: The log is empty.
-   parse json docker line error...: The plug-in fails to parse the log in JSON format. In this case, handle the error according to the error message and the first 512 bytes of the log.
-   parse cri docker line error...: The plug-in fails to parse the log in CRI format. In this case, handle the error according to the error message and the first 512 bytes of the log. |
|PLUGIN\_ALARM|An error occurs when the plug-in is initialized or called.|Click the error message to view details, which may contain the following error types. Handle the error accordingly. -   init plugin error...: Initiating the plug-in fails.
-   hold on error...: Stopping the plug-in fails.
-   resume error...: Recovering the plug-in fails.
-   start service error...: Starting service input-type plug-ins fails.
-   stop service error...: Stopping service input-type plug-ins fails. |
|PROCESSOR\_INIT\_ALARM|The regex plug-in fails to compile the Regex regular expression specified by the Config.|Click the error message to view details, and then check whether the regular expression is correct.|
|PROCESS\_TOO\_SLOW\_ALARM|Logtail parses logs too slowly.|1.  Click the error message to view details, and then determine whether the slow parsing is normal according to the number of logs, buffer size, and parsing time.
2.  If the slow parsing is abnormal, check whether inappropriate parsing configurations exist. For example, the processes on the node where Logtail resides occupy excessive CPU resources, or an inefficient regular expression exists. |
|REDIS\_PARSE\_ADDRESS\_ALARM|The redis plug-in fails to parse the ServerUrls specified by the Config.|Click the error message to view details, and then check the URL.|
|REGEX\_FIND\_ALARM|The regex plug-in cannot find the fields specified by SourceKey in the Config from the log.|Click the error message to view details, and then check whether the SourceKey is incorrect or an invalid log exists.|
|REGEX\_UNMATCHED\_ALARM|The regex plug-in fails to match the log.|Click the error message to view details, which may contain the following error types. Handle the error accordingly, for example, determine whether the Config is correct. -   unmatch this log content...: The log cannot match the regular expression in the Config.
-   match result count less...: The number of matched logs is less than that of Keys specified in the Config. |
|SAME\_CONFIG\_ALARM|There are Configs with the same name in a Logstore. In this case, Logtail chooses one to collect the log, and the others will be discarded.|Click the error message to view details, and then handle the error according to the Config path and other information.|
|SPLIT\_FIND\_ALARM|The split\_char and split\_string plug-ins cannot find the fields specified by SourceKey in the Config from the log.|Click the error message to view details, and then check whether SourceKey settings are incorrect or an invalid log exists.|
|SPLIT\_LOG\_ALARM|The number of parsed fields parsed by the processor\_split\_char and processor\_split\_string plug-ins does not match that of fields specified by SplitKeys.|Click the error message to view details, and then check whether SourceKey settings are incorrect or an invalid log exists.|
|STAT\_FILE\_ALARM|An error occurs when the plug-in collects files through the LogFileReader object.|Click the error message to view details, and handle the error according to the file path and other information.|
|SERVICE\_SYSLOG\_INIT\_ALARM|The service\_syslog plug-in initialization fails.|Click the error message to view details, and check whether Address in the Config is correct.|
|SERVICE\_SYSLOG\_STREAM\_ALARM|An error occurs when the service\_syslog plug-in collects data through TCP.|Click the error message to view details, which may contain the following error types. Handle the error accordingly. -   accept error...: An error occurs during Accept execution. In this case, the plug-in waits for a while and restarts.
-   setKeepAlive error...: Setting Keep Alive fails. In this case, the plug-in ignores the error and runs properly.
-   connection i/o timeout...: Reading data through TCP expires. In this case, the plug-in resets the expiration duration and reads data properly.
-   scan error...: An error occurs when the plug-in reads data through TCP. In this case, the plug-in waits for a while and tries again. |
|SERVICE\_SYSLOG\_PACKET\_ALARM|An error occurs when the service\_syslog plug-in collects data through UDP.|Click the error message to view details, which may contain the following error types. Handle the error accordingly. -   connection i/o timeout...: Reading data through UDP expires. In this case, the plug-in resets the expiration duration and reads data properly.
-   read from error...: An error occurs when the plug-in reads data through UDP. In this case, the plug-in waits for a while and tries again. |

