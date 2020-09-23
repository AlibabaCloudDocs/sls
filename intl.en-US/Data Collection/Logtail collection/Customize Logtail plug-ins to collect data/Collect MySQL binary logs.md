# Collect MySQL binary logs

This topic describes how to configure Logtail in the Log Service console to collect MySQL binary logs.

Logtail is installed on the server that you use to collect MySQL binary logs. For more information, see [Install Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md) or [Install Logtail in Windows](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md).

**Note:** Servers that run Linux support Logtail 0.16.0 or later. Servers that run Windows support Logtail 1.0.0.8 or later.

## Description

Logtail communicates with the primary MySQL server in the way that secondary MySQL servers communicate with the primary MySQL server. The following process is used during the communication:

1.  Logtail acts as a secondary server and sends dump requests to the primary server.
2.  After the primary server receives the dump requests, it sends binary logs to Logtail in real time.
3.  Logtail parses events in the binary logs, filters and parses data, and then uploads the parsed data to Log Service.

![Procedure](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9223359951/p2929.png)

## Features

-   Allows you to collect incremental data of MySQL databases, such as ApsaraDB for RDS databases, by using binary logs. This data collection mechanism provides excellent performance.
-   Supports multiple methods that filter data in databases.
-   Allows you to set binary log positions.
-   Allows you to record synchronization statuses by using the checkpoint mechanism.

## Limits

-   Binary logs of MySQL 8.0 or later cannot be collected.
-   Row-based binary logs must be enabled for MySQL databases. By default, row-based binary logs are enabled for RDS databases.

    ```
    # Check whether binary logging is enabled.
    mysql> show variables like "log_bin";
    +---------------+-------+
    | Variable_name | Value |
    +---------------+-------+
    | log_bin       | ON    |
    +---------------+-------+
    1 row in set (0.02 sec)
    # View the binary log format.
    mysql> show variables like "binlog_format";
    +---------------+-------+
    | Variable_name | Value |
    +---------------+-------+
    | binlog_format | ROW   |
    +---------------+-------+
    1 row in set (0.03 sec)
    ```

-   The ID of the secondary MySQL server whose role Logtail assumes must be unique.
-   Limits on RDS instances
    -   Logtail cannot be installed on a server where an RDS instance resides. You must install Logtail on a server that can communicate with the RDS instance.
    -   You can collect binary logs only from a primary RDS database. Collection of binary logs from secondary RDS databases is not supported.

## Scenarios

The MySQL binary logging feature is suitable for scenarios in which you need to synchronize large amounts of data. The following section lists three example scenarios:

-   Query the incremental data of databases in real time.
-   Audit operations that are performed on databases.
-   Use Log Service to query database updates, visualize search and analysis results, transform data for stream computing, export log data to MaxCompute for offline computing, and export log data to OSS for long-term storage.

## Precautions

We recommend that you increase resource limits on Logtail to accommodate traffic surges and avoid risks to your data. If the limits are exceeded, Logtail may be forced to restart.

You can set parameters in the /usr/local/ilogtail/ilogtail\_config.json file. For more information, see [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md).

The following example shows how to set the number of CPU cores to 2 and the memory size to 2,048 MB.

```
{
    ...
    "cpu_usage_limit":2,
    "mem_usage_limit":2048,

    ...
}
```

## Data reliability

We recommend that you enable the global transaction identifier \(GTID\) feature of the MySQL server and upgrade Logtail to version 0.16.15 or later. This avoids repeated data collection during a primary/secondary server switchover and ensures data reliability.

-   Incomplete collection of data: If the network between Logtail and the MySQL server is disconnected for a long period of time, data loss may occur.

    If the network between Logtail and the primary server is disconnected, the primary server still generates binary logs and deletes expired binary logs. When the network connection between Logtail and the primary MySQL server is recovered, Logtail uses a checkpoint to request more binary logs from the primary MySQL server. However, if the network has been disconnected for a long period of time, the data that is generated after the checkpoint may be deleted. In this case, the recovery mechanism is triggered. The mechanism locates the last binary log file position from which Logtail resumes binary log collection. The data between the checkpoint and the collection resumption position is not collected. This leads to incomplete data collection.

-   Repeated data collection: If the binary log file positions on the primary and secondary servers are different and a primary/secondary server switchover occurs, binary logs may be repeatedly collected.

    If the MySQL primary-secondary server synchronization is configured, the primary MySQL server synchronizes binary logs to the secondary MySQL server. The secondary server receives the log data, and stores the log data to its local binary log files. If the binary log file positions on the primary and secondary are different and a primary-secondary server switchover occurs, data may be repeatedly collected. This is because the checkpoint mechanism uses a binary log file name and an offset for data collection.

    For example, a piece of data ranges from `(binlog.100, 4)` to `(binlog.105, 4)` on the primary server, and ranges from `(binlog.1000, 4)` to `(binlog.1005, 4)` on the secondary server. Logtail has obtained the data from the primary server and updated the checkpoint to `(binlog.105, 4)`. If a primary-secondary server switchover occurs, Logtail continues to obtain binary logs from the new primary server based on the local checkpoint `(binlog.105, 4)`. However, the binary log position on the new primary MySQL server ranges from `(binlog.1000, 4)` to `(binlog.1005, 4)`. This position range is greater than the range that Logtail requests, resulting in repeated collection.


## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, select **MySQL BinLog - Plug-in**.

3.  In the Specify Logstore step, select the target project and Logstore, and click **Next**.

    You can also click **Create Now** to create a project and a Logstore. For more information, see [Step 1: Create a project and a Logstore](/intl.en-US/Quick Start/Quick start.md).

4.  In the Create Machine Group step, create a machine group.

    -   If a machine group is available, click **Using Existing Machine Groups**.
    -   This section uses ECS instances as an example to describe how to create a machine group. To create a machine group, perform the following steps:
        1.  Install Logtail on ECS instances. For more information, see [Install Logtail on ECS instances](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail on ECS instances.md).

            If Logtail is installed on the ECS instances, click **Complete Installation**.

            **Note:** If you need to collect logs from user-created clusters or servers of third-party cloud service providers, you must install Logtail on these servers. For more information, see [Install Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md#) or [Install Logtail in Windows](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md).

        2.  After the installation is complete, click **Complete Installation**.
        3.  On the page that appears, specify the parameters for the machine group. For more information, see [Create an IP address-based machine group](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create an IP address-based machine group.md) or [Create a custom ID-based machine group](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create a custom ID-based machine group.md).
5.  In the Machine Group Settings step, apply the configurations to the machine group.

    Select the created machine group and move the group from **Source Server Groups** to **Applied Server Groups**.

6.  In the Specify Data Source step, set the **Config Name** and **Plug-in Config** parameters.

    A template is provided for the **Plug-in Config** parameter. You can configure the inputs and processors fields in the template.

    -   inputs: Required. The Logtail configurations for log collection.

        **Note:** You can configure only one type of data source in the inputs field.

    -   processors: Optional. The Logtail configurations for data processing. You can configure one or more processing methods in the processors field. For more information, see [Process data](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Process data.md).
    ```
    {
     "inputs": [
         {
             "type": "service_canal",
             "detail": {
                 "Host": "************.mysql.rds.aliyuncs.com",
                 "Port": 3306,
                 "User" : "root",
                 "ServerID" : 56321,
                 "Password": "*******",
                 "IncludeTables": [
                     "user_info\\.. *"
                 ],
                 "ExcludeTables": [
                     ". *\\. \\S+_inner"
                 ],
                 "TextToString" : true,
                 "EnableDDL" : true
             }
         }
     ]
    }
    ```

    |Parameter|Type|Required|Description|
    |:--------|:---|:-------|:----------|
    |type|string|Yes|The type of the data source. Set the value to service\_canal.|
    |Host|string|No|The IP address of the primary MySQL server. Default value: 127.0.0.1.|
    |Port|int|No|The port that is used to access the database of the primary MySQL server. Default value: 3306.|
    |User|string|No|The username that is used to log on to the database. Default value: root. Make sure that the user is granted the read permissions, REPLICATION SLAVE, and REPLICATION CLIENT permissions on the database. Example:

    ```
CREATE USER canal IDENTIFIED BY 'canal';  
GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON *. * TO 'canal'@'%';
-- GRANT ALL PRIVILEGES ON *. * TO 'canal'@'%' ;
FLUSH PRIVILEGES;
    ``` |
    |Password|string|No|The password that is used to log on to the database. Default value: null. If you require a high level of security, we recommend that you set the password to xxx. After your configurations are synchronized to the local server, find the Password parameter in the /usr/local/ilogtail/user\_log\_config.json file and set the value to your password. For more information, see [Modify the configurations on the server where Logtail is installed](#section_h4m_4er_yl1).

**Note:** If you modify this parameter in the console, the local configuration will be overwritten after the modification is synchronized to the local server. |
    |ServerID|int|No|The ID of the secondary MySQL server whose role Logtail assumes. Default value: 125. **Note:** The value of the ServerID parameter must be unique. Otherwise, data collection fails. |
    |IncludeTables|String array|Yes|The names of tables from which incremental data is collected. Each name must include the name of the database to which a table belongs and a table name, for example, test\_db.test\_table. You can specify a regular expression for the parameter. Logtail collects incremental data only from tables whose names match the regular expression specified by the IncludeTables parameter. To collect incremental data from all tables of a database, set the IncludeTables parameter to . \*\\\\.. \*. **Note:** To implement exact match, add ^ to the beginning of a regular expression and $ to the end. Example: ^test\_db\\\\.test\_table$. |
    |ExcludeTables|String array|No|The names of tables from which incremental data is not collected. Each name must include the name of the database to which a table belongs and a table name, for example, test\_db.test\_table. You can specify a regular expression for the parameter. If a table meets one of the conditions specified by the ExcludeTables parameter, incremental data from the table is not collected. If you do not specify this parameter, incremental data from all tables is collected. **Note:** To implement exact match, add ^ to the beginning of a regular expression and $ to the end. Example: ^test\_db\\\\.test\_table$. |
    |StartBinName|string|No|The name of the first binary log file from which data is collected. If you do not specify this parameter, incremental data collection starts from the current time. To collect data from a specified position, set StartBinName to the name of the first binary log file from which data is collected and StartBinLogPos to the offset. Example:

    ```
# Set StartBinName to "mysql-bin.000063" and StartBinLogPos to 0.
mysql> show binary logs;
+------------------+-----------+
| Log_name         | File_size |
+------------------+-----------+
| mysql-bin.000063 |       241 |
| mysql-bin.000064 |       241 |
| mysql-bin.000065 |       241 |
| mysql-bin.000066 |     10778 |
+------------------+-----------+
4 rows in set (0.02 sec)
    ```

**Note:**

If you set the StartBinName parameter, the first collection will generate a large amount of traffic. |
    |StartBinlogPos|int|No|The offset of the first binary log file from which incremental data is collected. Default value: 0.|
    |EnableGTID|bool|No|Specifies whether to assign [GTIDs](https://dev.mysql.com/doc/refman/5.7/en/replication-gtids-concepts.html) to uploaded data. Default value: true. If you set the value to false, the data that is uploaded to Log Service is not attached global transaction IDs \(GTIDs\).|
    |EnableInsert|bool|No|Specifies whether to collect INSERT event logs. Default value: true. If you set the value to false, INSERT event logs are not collected.|
    |EnableUpdate|bool|No|Specifies whether to collect UPDATE event logs. Default value: true. If you set the value to false, UPDATE event logs are not collected.|
    |EnableDelete|bool|No|Specifies whether to collect DELETE event logs. Default value: true. If you set the value to false, DELETE event logs are not collected.|
    |EnableDDL|bool|No|Specifies whether to collect data definition language \(DDL\) event logs. Default value: false. This value indicates that DDL event logs are not collected. **Note:** This parameter does not support the IncludeTables and ExcludeTables filtering methods. |
    |Charset|string|No|The encoding format. Default value: utf-8.|
    |TextToString|bool|No|Specifies whether to convert data of the text type to strings. Default value: false. This value indicates that the data type is not converted.|
    |PackValues|bool|No|Specifies whether to convert event data into the JSON format. Default value: false. This value indicates that event data is not converted. If the value is set to true, Logtail converts event data into the data and old\_data fields in the JSON format. The old\_data field is available only for ROW\_UPDATE events. For example, a table has three fields named c1, c2, and c3. If the value is set to false, the ROW\_INSERT event data contains the c1, c2, and c3 fields. If the value is set to true, the c1, c2, and c3 fields are converted into the data field and the value is `{"c1":"...", "c2": "...", "c3": "..."}`.

**Note:** This parameter is available only for Logtail 0.16.19 and later. |
    |EnableEventMeta|bool|No|Specifies whether to collect event metadata. Default value: false. This value indicates that event metadata is not collected. The metadata of binary log events includes event\_time, event\_log\_position, event\_size, and event\_server\_id. **Note:** This parameter is available only for Logtail 0.16.21 and later. |

7.  In the **Configure Query and Analysis** step, configure the indexes.

    Indexes are configured by default. You can re-configure the indexes based on your business requirements. For more information, see [Enable and configure the index feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

    **Note:**

    -   You must configure Full Text Index or Field Search. If you configure both of them, the settings of Field Search are applied.
    -   If the data type of index is long or double, the Case Sensitive and Delimiter settings are unavailable.

After a Logtail configuration file is synchronized to the server, Logtail immediately collects the modified data and writes the data to Log Service if your database has changes.

**Note:** By default, Logtail collects binary logs.

## Modify the configurations on the server where Logtail is installed

If you did not enter real information for the Host, User, Password, and other parameters under the **inputs** field, you can modify the parameters after the configurations are synchronized to the local server.

1.  Log on to the server where Logtail is installed.

2.  Find the service\_canal keyword in the /usr/local/ilogtail/user\_log\_config.json file and set the parameters. These parameters include Host, User, and Password.

3.  Run the following command to restart Logtail:

    ```
    sudo /etc/init.d/ilogtaild stop; sudo /etc/init.d/ilogtaild start
    ```


After Logtail sends MySQL binary logs to Log Service, you can view the logs in the Log Service console. For example, after you perform the `INSERT`, `UPDATE`, `DELETE` operations on the `SpecialAlarm` table of the `user_info` database, binary logs are collected and uploaded to Log Service. This section describes the schema of the table. This section also lists the INSERT, DELETE, and UPDATE operations on the table, and describes sample logs that are introduced by the three operations.

-   Create a schema

    ```
    CREATE TABLE `SpecialAlarm` (
    `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
    `time` datetime NOT NULL,
    `alarmtype` varchar(64) NOT NULL,
    `ip` varchar(16) NOT NULL,
    `count` int(11) unsigned NOT NULL,
    PRIMARY KEY (`id`),
    KEY `time` (`time`) USING BTREE,
    KEY `alarmtype` (`alarmtype`) USING BTREE
    ) ENGINE=MyISAM AUTO_INCREMENT=1;
    ```

-   Database operations

    Perform the INSERT, DELETE, and UPDATE operations on the database.

    ```
    insert into specialalarm (`time`, `alarmType`, `ip`, `count`) values(now(), "NO_ALARM", "10.10. **.***", 55);
    delete from specialalarm where id = 4829235  ;
    update specialalarm set ip = "10.11. ***.**" where id = "4829234";
    ```

    Create an index for `zc.specialalarm`.

    ```
    ALTER TABLE `zc`.`specialalarm` 
    ADD INDEX `time_index` (`time` ASC);
    ```

-   Sample log data

    You can view the log data that is introduced by each operation on the Search & Analysis page of a Logstore. The following examples show log data that is introduced by different operations:

    -   Log data that is introduced by the INSERT operation

        ```
        __source__:  10.30. **.**  
        __tag__:__hostname__:  iZbp145dd9fccu*****  
        __topic__:    
        _db_:  zc  
        _event_:  row_insert  
        _gtid_:  7d2ea78d-b631-11e7-8afb-00163e0eef52:536  
        _host_:  *********.mysql.rds.aliyuncs.com  
        _id_:  113  
        _table_:  specialalarm  
        alarmtype:  NO_ALARM  
        count:  55  
        id:  4829235  
        ip:  10.10. ***.***  
        time:  2017-11-01 12:31:41
        ```

    -   DELETE statement

        ```
        __source__:  10.30. **.**  
        __tag__:__hostname__:  iZbp145dd9fccu****
        __topic__:    
        _db_:  zc  
        _event_:  row_delete  
        _gtid_:  7d2ea78d-b631-11e7-8afb-00163e0eef52:537  
        _host_:  *********.mysql.rds.aliyuncs.com  
        _id_:  114  
        _table_:  specialalarm  
        alarmtype:  NO_ALARM  
        count:  55  
        id:  4829235  
        ip:  10.10. **.***
        time:  2017-11-01 12:31:41
        ```

    -   UPDATE statement

        ```
        __source__:  10.30. **.**  
        __tag__:__hostname__:  iZbp145dd9fccu****  
        __topic__:    
        _db_:  zc  
        _event_:  row_update  
        _gtid_:  7d2ea78d-b631-11e7-8afb-00163e0eef52:538  
        _host_:  *********.mysql.rds.aliyuncs.com  
        _id_:  115  
        _old_alarmtype:  NO_ALARM  
        _old_count:  55  
        _old_id:  4829234  
        _old_ip:  10.10.22.133  
        _old_time:  2017-10-31 12:04:54  
        _table_:  specialalarm  
        alarmtype:  NO_ALARM  
        count:  55  
        id:  4829234  
        ip:  10.11. ***.***
        time:  2017-10-31 12:04:54
        ```

    -   DDL statement

        ```
        __source__:  10.30. **.**  
        __tag__:__hostname__:  iZbp145dd9fccu****  
        __topic__:    
        _db_:  zc  
        _event_:  row_update  
        _gtid_:  7d2ea78d-b631-11e7-8afb-00163e0eef52:539  
        _host_:  *********.mysql.rds.aliyuncs.com  
        ErrorCode:  0
        ExecutionTime:  0
        Query:  ALTER TABLE `zc`.`specialalarm` 
        ADD INDEX `time_index` (`time` ASC)
        StatusVars:
        ```

    |Field|Description|
    |:----|:----------|
    |`_host_`|The name of the host where the database resides.|
    |`_db_`|The name of the RDS database.|
    |`_table_`|The name of the table.|
    |`_event_`|The type of the system event.|
    |`_id_`|The ID of the current collection. The value starts from 0 and increments by 1 each time a binary log event is collected.|
    |`_gtid_`|The global transaction ID.|
    |`_filename_`|The name of the binary log file.|
    |`_offset_`|The offset of the binary log file. The value is updated after each COMMIT operation.|


