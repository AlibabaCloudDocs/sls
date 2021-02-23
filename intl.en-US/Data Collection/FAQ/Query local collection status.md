# Query local collection status

Logtail is used to query its own health status and log collection progress, helping you troubleshoot log collection issues and customize status monitoring for log collection.

1.  [User guide](#section_ebd_jjs_zdb)
    1.  [all command](#section_wmp_rjs_zdb)
    2.  [active command](#section_yml_cks_zdb)
    3.  [logstore command](#section_uxb_kks_zdb)
    4.  [logfile command](#section_png_vks_zdb)
    5.  [history command](#section_rzj_cls_zdb)
2.  [Return values](#section_ff4_hls_zdb)
3.  [Use cases](#section_ysz_pls_zdb)
    1.  [Monitor the running status of Logtail](#section_jcw_rls_zdb)
    2.  [Monitor log collection progress](#section_tsq_sls_zdb)
    3.  [Determine whether or not Logtail has finished collecting log files](#section_uxz_tls_zdb)
    4.  [Troubleshoot log collection issues](#section_gg1_vls_zdb)

## User guide

If a Logtail client supporting status query function is installed, you can query local log collection status by entering commands on the client. To install Logtail, see [Install Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md).

Enter the `/etc/init.d/ilogtaild -h` command on the client to check if the client supports querying local log collection status. If the `logtail insight, version` keyword is returned, it indicates that this function is supported on the Logtail client.

```
/etc/init.d/ilogtaild -h
Usage: ./ilogtaild { start | stop (graceful, flush data and save checkpoints) | force-stop | status | -h for help}$
logtail insight, version : 0.1.0
commond list :
       status all [index] 
             get logtail running status 
       status active [--logstore | --logfile] index [project] [logstore] 
             list all active logstore | logfile. if use --logfile, please add project and logstore. default --logstore
       status logstore [--format=line | json] index project logstore 
             get logstore status with line or json style. default --format=line 
       status logfile [--format=line | json] index project logstore fileFullPath 
             get log file status with line or json style. default --format=line 
       status history beginIndex endIndex project logstore [fileFullPath] 
             query logstore | logfile history status.  
index : from 1 to 60. in all, it means last $(index) minutes; in active/logstore/logfile/history, it means last $(index)*10 minutes
```

Currently, Logtail supports the following query commands, command functions, time intervals to query and time windows for result statistics:

|Command|Functions|Time interval to query|Time window for statistics|
|:------|:--------|:---------------------|:-------------------------|
|all|Query the running status of Logtail.|Last 60 min|1 min|
|active|Query Logstores or log files that are currently active \(that is, with data collected\).|Last 600 min|10 minutes.|
|logstore|Query the collection status of a Logstore.|Last 600 min|10 minutes.|
|logfile|Query the collection status of a log file.|Last 600 min|10 minutes.|
|history|Query the collection status of a Logstore or log file over a period of time.|Last 600 min|10 minutes.|

**Note:**

-   The `index` parameter in the command represents the index value of the time window, which is counted from the current time. Its valid range is 1–60. If the time window for statistics is one minute, windows in the last `(index, index-1]` minutes are queried. If the time window for statistics is 10 minutes, windows in the last `(10*index, 10*(index-1)]` minutes are queried.
-   All query commands belong to status subcommands, so the main command is status.

## all command

Command format

```
/etc/init.d/ilogtaild status all [ index ] 
```

**Note:** The all command is used to view the running status of Logtail. The index parameter is optional. If left blank, 1 is taken by default.

Example

```
/etc/init.d/ilogtaild status all 1
ok
/etc/init.d/ilogtaild status all 10
busy
```

Output description

|Item|Description|Priority|Resolution:|
|:---|:----------|:-------|:----------|
|ok|The current status is normal.|None.|No action is needed.|
|busy|The current collection speed is high and the Logtail status is normal.|None.|No action is needed.|
|many\_log\_files|The number of logs being collected is large.|Low|Check if the configuration contains files that do not need to be collected.|
|process\_block|Current log parsing is blocked.|Low|Check if logs are generated too quickly. If you still get this output, [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md#) as per your needs to modify the upper limit of CPU usage or the limit on concurrent sending by using network.|
|send\_block|Current sending is blocked.|Relatively high|blocked. Check if logs are generated too quickly and if the network status is normal. If you still get this output, [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md#) as per your needs to modify the upper limit of CPU usage or the limit on concurrent sending by using network.|
|send\_error|Failed to upload log data.|High|To troubleshoot the issue, see [Diagnose collection errors]().|

## active command

Command format

```
/etc/init.d/ilogtaild status active [--logstore] index
 /etc/init.d/ilogtaild status active --logfile index project-name logstore-name
                
```

**Note:**

-   The `active [--logstore] index` command is used to query Logstores that are currently active. The `--logstore` parameter can be omitted without changing the meaning of the command.
-   The `active --logfile index project-name logstore-name` command is used to query all active log files in a Logstore for a project.
-   The active command is used to query active log files level by level. We recommend that you first locate the currently active Logstore and then query active log files in this Logstore.

Example

```
/etc/init.d/ilogtaild status active 1
sls-zc-test : release-test
sls-zc-test : release-test-ant-rpc-3
sls-zc-test : release-test-same-regex-3

/etc/init.d/ilogtaild status active --logfile 1 sls-zc-test release-test
/disk2/test/normal/access.log
```

Output description

-   To run the `active --logstore index` command, all currently active Logstores are output in the format of `project-name : logstore-name` . To run the `active --logfile index project-name logstore-name` command, the complete paths of active log files are output.
-   A Logstore or log file with no log collection activity in the current query window does not appear in the output.

## logstore command

Command format

```
/etc/init.d/ilogtaild status logstore [--format={line|json}] index project-name logstore-name
                
```

**Note:**

-   The logstore command is used to output the collection statuses of the specified project and Logstore in LINE or JSON format.
-   If the`--format=` parameter is not configured, `--format=line` is selected by default. The echo information is output in LINE format. Note that `--format` parameter must be placed behind `logstore` .
-   If this Logstore does not exist or has no log collection activity in the current query window, you get an empty output in LINE format or a `null` value in JSON format.

Example

```
/etc/init.d/ilogtaild status logstore 1 sls-zc-test release-test-same
time_begin_readable : 17-08-29 10:56:11
time_end_readable : 17-08-29 11:06:11
time_begin : 1503975371
time_end : 1503975971
project : sls-zc-test
logstore : release-test-same
status : ok
config : ##1.0##sls-zc-test$same
read_bytes : 65033430
parse_success_lines : 230615
parse_fail_lines : 0
last_read_time : 1503975970
read_count : 687
avg_delay_bytes : 0
max_unsend_time : 0
min_unsend_time : 0
max_send_success_time : 1503975968
send_queue_size : 0
send_network_error_count : 0
send_network_quota_count : 0
send_network_discard_count : 0
send_success_count : 302
send_block_flag : false
sender_valid_flag : true
/etc/init.d/ilogtaild status logstore --format=json 1 sls-zc-test release-test-same
{
   "avg_delay_bytes" : 0,
   "config" : "##1.0##sls-zc-test$same",
   "last_read_time" : 1503975970,
   "logstore" : "release-test-same",
   "max_send_success_time" : 1503975968,
   "max_unsend_time" : 0,
   "min_unsend_time" : 0,
   "parse_fail_lines" : 0,
   "parse_success_lines" : 230615,
   "project" : "sls-zc-test",
   "read_bytes" : 65033430,
   "read_count" : 687,
   "send_block_flag" : false,
   "send_network_discard_count" : 0,
   "send_network_error_count" : 0,
   "send_network_quota_count" : 0,
   "send_queue_size" : 0,
   "send_success_count" : 302,
   "sender_valid_flag" : true,
   "status" : "ok",
   "time_begin" : 1503975371,
   "time_begin_readable" : "17-08-29 10:56:11",
   "time_end" : 1503975971,
   "Maid": "17-08-29 11:06:11"
}
```

Output description

|Reserved Word|Meaning|Unit|
|:------------|:------|:---|
|Status|The overall status of this Logstore. For specific statuses, descriptions, and change methods, see the following table.|None.|
|time\_begin\_readable|The start time that can be read.|None.|
|time\_end\_readable|The end time that can be read.|None.|
|time\_begin|The start time of statistics.|UNIX timestamp, measured in seconds.|
|time\_end|The end time of statistics.|UNIX timestamp, measured in seconds.|
|project|The project name.|None.|
|logstore|The Logstore name.|None.|
|config|The collection configuration name, which is globally unique and consisted of `##1.0##`, project, `$`, and config.|None.|
|read\_bytes|The number of logs read in the window.|Byte|
|parse\_success\_lines|The number of successfully parsed log lines in the window.|Line|
|parse\_fail\_lines|The number of log lines that failed to be parsed in the window.|Line|
|last\_read\_time|The last read time in the window.|UNIX timestamp, measured in seconds.|
|Read\_count|The number of times that logs are read in the window.|Number|
|avg\_delay\_bytes|The average of the differences between the current offset and the file size each time logs are read in the window.|Byte|
|max\_unsend\_time|The maximum time that unsent data packets are in the send queue when the window ends. The value is 0 when the queue is empty.|UNIX timestamp, measured in seconds.|
|min\_unsend\_time|The minimum time that unsent data packets are in the send queue when the window ends. The value is 0 when the queue is empty.|UNIX timestamp, measured in seconds.|
|max\_send\_success\_time|The maximum time that data is successfully sent in the window.|UNIX timestamp, measured in seconds.|
|send\_queue\_size|The number of unsent data packets in the current send queue when the window ends.|Packet|
|send\_network\_error\_count|The number of data packets that failed to be sent in the window because of network errors.|Packet|
|send\_network\_quota\_count|The number of data packets that failed to be sent in the window because the quota is exceeded.|Packet|
|send\_network\_discard\_count|The number of discarded data packets in the window because of data exceptions or insufficient permissions.|Packet|
|send\_success\_count|The number of successfully sent data packets in the window.|Packet|
|send\_block\_flag|Whether or not the send queue is blocked when the window ends.|None.|
|sender\_valid\_flag|Whether or not the send flag of this Logstore is valid when the window ends. true means the flag is valid, and false means the flag is disabled because of network errors or quota errors.|None.|

Logstore status

|Status|Meaning|Handling method|
|:-----|:------|:--------------|
|ok|The status is normal.|No action is needed.|
|process\_block|Log parsing is blocked.|Check if logs are generated too quickly. If you still get this output, Configure [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md#) as per your needs to modify the upper limit of CPU usage or the limit on concurrent sending by using network.|
|parse\_fail|Log parsing failed.|Check whether or not the log format is consistent with the log collection configuration.|
|send\_block|Current sending is blocked.|blocked. Check if logs are generated too quickly and if the network status is normal. If you still get this output, [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md#) as per your needs to modify the upper limit of CPU usage or the limit on concurrent sending by using network.|
|sender\_invalid|An exception occurred when sending log data.|Check the network status. If the network is normal, see [Diagnose collection errors]() in Query diagnosis errors to troubleshoot the issue.|

## logfile command

Command format

```
/etc/init.d/ilogtaild status logfile [--format={line|json}] index project-name logstore-name fileFullPath
                
```

**Note:**

-   The logfile command is used to output the collection status of a specific log file in LINE or JSON format.
-   If the `--format=` parameter is not configured,`--format=line` is selected by default. The echo information is output in LINE format.
-   If this log file does not exist or has no log collection activity in the current query window, you get an empty output in LINE format or a `null` value in JSON format.
-   The `--format` parameter must be placed behind `logfile` .
-   The `filefullpath`must be a full path name.

Example

```
/etc/init.d/ilogtaild status logfile 1 sls-zc-test release-test-same /disk2/test/normal/access.log
time_begin_readable : 17-08-29 11:16:11
time_end_readable : 17-08-29 11:26:11
time_begin : 1503976571
time_end : 1503977171
project : sls-zc-test
logstore : release-test-same
status : ok
config : ##1.0##sls-zc-test$same
file_path : /disk2/test/normal/access.log
file_dev : 64800
file_inode : 22544456
file_size_bytes : 17154060
file_offset_bytes : 17154060
read_bytes : 65033430
parse_success_lines : 230615
parse_fail_lines : 0
last_read_time : 1503977170
read_count : 667
avg_delay_bytes : 0
/etc/init.d/ilogtaild status logfile --format=json 1 sls-zc-test release-test-same /disk2/test/normal/access.log
{
   "avg_delay_bytes" : 0,
   "config" : "##1.0##sls-zc-test$same",
   "file_dev" : 64800,
   "file_inode" : 22544456,
   "file_path" : "/disk2/test/normal/access.log",
   "file_size_bytes" : 17154060,
   "last_read_time" : 1503977170,
   "logstore" : "release-test-same",
   "parse_fail_lines" : 0,
   "parse_success_lines" : 230615,
   "project" : "sls-zc-test",
   "read_bytes" : 65033430,
   "read_count" : 667,
   "read_offset_bytes" : 17154060,
   "status" : "ok",
   "time_begin" : 1503976571,
   "time_begin_readable" : "17-08-29 11:16:11",
   "time_end" : 1503977171,
   "time_end_readable" : "17-08-29 11:26:11"
}
```

Output description

|Reserved Word|Meaning|Unit|
|:------------|:------|:---|
|Status|The collection status of this log file in the current query window. See the status of logstore command.|None.|
|time\_begin\_readable|The start time that can be read.|None.|
|time\_end\_readable|The end time that can be read.|None.|
|time\_begin|The start time of statistics.|UNIX timestamp, measured in seconds.|
|time\_end|The end time of statistics.|UNIX timestamp, measured in seconds.|
|project|The project name.|None.|
|logstore|The Logstore name.|None.|
|file\_path|The path of the log file.|None.|
|file\_dev|The device ID of the log file.|None.|
|file\_inode|The inode of the log file.|None.|
|file\_size\_bytes|The size of the last scanned file in the window.|Byte|
|read\_offset\_bytes|The parsing offset of this file.|Byte|
|config|The collection configuration name, which is globally unique and consisted of `##1.0##` , project, `$` and config.|None.|
|read\_bytes|The number of logs read in the window.|Byte|
|parse\_success\_lines|The number of successfully parsed log lines in the window.|Line|
|parse\_fail\_lines|The number of log lines that failed to be parsed in the window.|Line|
|last\_read\_time|The last read time in the window.|UNIX timestamp, measured in seconds.|
|read\_count|The number of times that logs are read in the window.|Number of times|
|avg\_delay\_bytes|The average of the differences between the current offset and the file size each time logs are read in the window.|Byte|

## history command

Command format

```
/etc/init.d/ilogtaild status history beginIndex endIndex project-name logstore-name [fileFullPath]
                
```

**Note:**

-   The history command is used to query the collection status of a Logstore or log file over a period of time.
-   `beginIndex` and `endIndex` represent the start and end values for the code query window index respectively. `beginIndex <= endIndex`.
-   If the `fileFullPath` is not entered in the parameter, the code queries the collection information of the Logstore. Otherwise, the collection information of the log file is queried.

Example

```
/etc/init.d/ilogtaild status history 1 3 sls-zc-test release-test-same /disk2/test/normal/access.log
        begin_time status read parse_success parse_fail      last_read_time read_count avg_delay device inode file_size read_offset
 17-08-29 11:26:11 ok 62.12MB 231000 0 17-08-29 11:36:11 671 0B 64800 22544459 18.22MB 18.22MB
 17-08-29 11:16:11 ok 62.02MB 230615 0 17-08-29 11:26:10 667 0B 64800 22544456 16.36MB 16.36MB
 17-08-29 11:06:11 ok 62.12MB 231000 0 17-08-29 11:16:11 687 0B 64800 22544452 14.46MB 14.46MB
$/etc/init.d/ilogtaild status history 2 5 sls-zc-test release-test-same
        begin_time status read parse_success parse_fail      last_read_time read_count avg_delay send_queue network_error quota_error discard_error send_success send_block send_valid max_unsend min_unsend    max_send_success
 17-08-29 11:16:11 ok 62.02MB 230615 0 17-08-29 11:26:10 667 0B 0 0 0 0 300 false true 70-01-01 08:00:00 70-01-01 08:00:00 17-08-29 11:26:08
 17-08-29 11:06:11 ok 62.12MB 231000 0 17-08-29 11:16:11 687 0B 0 0 0 0 303 false true 70-01-01 08:00:00 70-01-01 08:00:00 17-08-29 11:16:10
 17-08-29 10:56:11 ok 62.02MB 230615 0 17-08-29 11:06:10 687 0B 0 0 0 0 302 false true 70-01-01 08:00:00 70-01-01 08:00:00 17-08-29 11:06:08
 17-08-29 10:46:11 ok 62.12MB 231000 0 17-08-29 10:56:11 692 0B 0 0 0 0 302 false true 70-01-01 08:00:00 70-01-01 08:00:00 17-08-29 10:56:10
```

Output description

-   This command outputs historical collection information of a Logstore or log file in the form of list, one line for each window.
-   For the description of each output field, see the `logstore` and `logfile` commands.

## Return values

Normal return value

0 is returned if a command input is valid \(including failure to query a Logstore or log file\), for example:

```
/etc/init.d/ilogtaild status logfile --format=json 1 error-project error-logstore /no/this/file
null
echo $?
0
/etc/init.d/ilogtaild status all
ok
echo $?
0
```

Exceptional return values

A non-zero return value indicates an exception. See the following table.

|Return value|Type|output|Troubleshooting|
|:-----------|:---|:-----|:--------------|
|10|Invalid command or missing parameters|`invalid param, use -h for help.`|Enter `-h` to view help.|
|1|The query goes beyond the 1–60 time window|`invalid query interval`|Enter `-h` to view help.|
|1|Cannot query the specified time window|`query fail, error: $(error)`. For more information, see [errno interpretation](http://man7.org/linux/man-pages/man3/errno.3.html?spm=a2c4g.11186623.2.24.XN6vN6).|This issue might occur when the startup time of Logtail is less than the query time span. For other cases, open a ticket.|
|1|No matching query window time|`no match time interval, please check logtail Status`|Check if Logtail is running. For other cases, open a ticket.|
|1|No data in the query window|`invalid profile, maybe logtail Restart`|Check if Logtail is running. For other cases, open a ticket.|

Example

```
/etc/init.d/ilogtaild status nothiscmd
invalid param, use -h for help.
echo $?
10
/etc/init.d/ilogtaild status/all 99
invalid query interval
echo $?
1
```

## Use cases

You can obtain the overall status of Logtail by querying its health status, and obtain the related metrics during collection by querying the collection progress. With the obtained information, you can monitor log collection in a customized manner.

## Monitor the running status of Logtail

Monitor the running status of Logtail by using the `all` command.

How it works: The current status of Logtail is queried every minute. If Logtail is under `process_block` , `send_block` , or `send_error` status for five successive minutes, an alarm is triggered.

The alarm duration and the status range being monitored can be adjusted according to the importance of log collection in specific scenarios.

## Monitor log collection progress

Monitor the collection progress of a Logstore by using the `logstore` command.

How it works: The `logstore` command is called every ten minutes to obtain the status information of this Logstore. If the `avg_delay_bytes` is over 1 MB \(1024\*1024\) or `status` is not `ok` , an alarm is triggered.

The `avg_delay_bytes` alarm threshold can be adjusted according to the log collection traffic.

## Determine whether or not Logtail has finished collecting log files

Determine whether or not Logtail has finished collecting log files by using the `logfile` command.

How it works: After writing to the log file stops, the `logfile` command is called every ten minutes to obtain the status information of this file. If this file shows the same value for `read_offset_bytes` and `file_size_bytes` , it means that Logtail has finished collecting this log file.

## Troubleshoot log collection issues

If the log collection is delayed on a server, use the `history` command to query related collection information on this server.

1.  If the `send_block_flag` is true, it indicates that the log collection delays because of the network.
    -   If the `send_network_quota_count` is greater than 0, you must split the [Shard](/intl.en-US/Data Collection/Preparation/Manage shards.md) of the Logstore.
    -   If the `send_network_error_count` is greater than 0, you must check the network connectivity.
    -   ◦If no related network error occurs, you must adjust the limit on concurrent sending and [traffic limit](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md) of Logtail.
2.  Sending-related parameters are normal, but the `avg_delay_bytes` is relatively high.
    -   The average log parsing speed can be calculated by using `read_bytes` to determine if traffic generated by logs is normal.
    -   [Resource usage limits](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md) of Logtail can be adjusted as appropriate.
3.  The `parse_fail_lines` is greater than 0.

    Check if the parsing configurations for log collection match with all the logs.


