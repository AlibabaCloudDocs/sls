# ApsaraDB RDS

This topic describes the fields of ApsaraDB RDS logs. ApsaraDB RDS logs include SQL audit logs, slow query logs, and performance logs.

## SQL audit logs

|Log field|Description|
|---------|-----------|
|\_\_topic\_\_|The topic of a log entry. Valid value: rds\_audit\_log.|
|owner\_id|The ID of an Alibaba Cloud account.|
|region|The region where an RDS instance resides.|
|instance\_name|The name of an RDS instance.|
|instance\_id|The ID of an RDS instance.|
|db\_type|The type of an RDS instance.|
|db\_version|The version of an RDS instance.|
|check\_rows|The number of scanned rows.|
|db|The name of a database.|
|fail|Indicates the result that is returned after an SQL statement is executed. Valid values:-   0: successful
-   1: failed |
|client\_ip|The IP address of a client that accesses an RDS instance.|
|latency|The latency of a result that is returned after an SQL statement is executed. Unit: microseconds.|
|origin\_time|The point in time when an SQL statement is executed.|
|return\_rows|The number of returned rows.|
|sql|The SQL statement.|
|thread\_id|The ID of a thread.|
|user|The name of a user who executes an SQL statement.|
|update\_rows|The number of updated rows.|

## Slow query logs

|Log field|Description|
|---------|-----------|
|\_\_topic\_\_|The topic of a log entry. Valid value: rds\_slow\_log.|
|owner\_id|The ID of an Alibaba Cloud account.|
|region|The region where an RDS instance resides.|
|instance\_name|The name of an RDS instance.|
|instance\_id|The ID of an RDS instance.|
|db\_type|The type of an RDS instance.|
|db\_version|The version of an RDS instance.|
|db\_name|The name of a database.|
|rows\_examined|The number of scanned rows.|
|rows\_sent|The number of returned rows.|
|start\_time|The time when an SQL statement is executed.|
|query\_time|The time that is consumed to execute an SQL statement. Unit: seconds.|
|lock\_time|The time that is consumed by a lock wait. Unit: seconds.|
|user\_host|The information of a client.|
|query\_sql|The SQL statement of a slow query.|

## Performance logs

|Metric|Description|
|------|-----------|
|mysql\_perf\_active\_session|The number of active connections.|
|mysql\_perf\_com\_delete|The average number of times that the DELETE statements are executed per second.|
|mysql\_perf\_com\_insert|The average number of times that the INSERT statements are executed per second.|
|mysql\_perf\_com\_insert\_select|The average number of times that the INSERT SELECT statements are executed per second.|
|mysql\_perf\_com\_replace|The average number of times that the REPLACE statements are executed per second.|
|mysql\_perf\_com\_replace\_select|The average number of times of the REPLACE SELECT statements are executed per second.|
|mysql\_perf\_com\_select|The average number of times of the SELECT statements are executed per second.|
|mysql\_perf\_com\_update|The average number of times of the UPDATE statements are executed per second.|
|mysql\_perf\_conn\_usage|The connection utilization of an RDS instance. Unit: percent.|
|mysql\_perf\_cpu\_usage|The CPU utilization of an RDS instance. Unit: percent.|
|mysql\_perf\_data\_size|The data usage of an RDS instance. Unit: MB.|
|mysql\_perf\_disk\_usage|The disk utilization of an RDS instance. Unit: percent.|
|mysql\_perf\_ibuf\_dirty\_ratio|The utilization of a buffer pool. Unit: percent.|
|mysql\_perf\_ibuf\_read\_hit|The read hit ratio of a buffer pool. Unit: percent.|
|mysql\_perf\_ibuf\_request\_r|The average number of read operations that are performed on an InnoDB buffer pool per second.|
|mysql\_perf\_ibuf\_request\_w|The average number of write operations that are performed on an InnoDB buffer pool per second.|
|mysql\_perf\_ibuf\_use\_ratio|The dirty ratio of a buffer pool. Unit: percent.|
|mysql\_perf\_inno\_data\_read|The amount of data that InnoDB reads per second. Unit: KB.|
|mysql\_perf\_inno\_data\_written|The amount of data that InnoDB writes per second. Unit: KB.|
|mysql\_perf\_inno\_row\_delete|The average number of rows that are deleted from an InnoDB table per second.|
|mysql\_perf\_inno\_row\_insert|The average number of rows that are inserted from an InnoDB table per second.|
|mysql\_perf\_inno\_row\_readed|The average number of rows that are read from an InnoDB table per second.|
|mysql\_perf\_inno\_row\_update|The average number of rows that are updated from an InnoDB table per second.|
|mysql\_perf\_innodb\_log\_write\_requests|The average number of log write requests per second.|
|mysql\_perf\_innodb\_log\_writes|The average number of physical writes to a log file per second.|
|mysql\_perf\_innodb\_os\_log\_fsyncs|The average number of write operations to a log file by calling the fsync\(\) function.|
|mysql\_perf\_ins\_size|The disk usage of an RDS instance. Unit: MB.|
|mysql\_perf\_iops|The IOPS. Unit: operations per second.|
|mysql\_perf\_iops\_usage|The IOPS utilization of an RDS instance. Unit: percent.|
|mysql\_perf\_kbytes\_received|The average inbound traffic per second. Unit: KB.|
|mysql\_perf\_kbytes\_sent|The average outbound traffic per second. Unit: KB.|
|mysql\_perf\_log\_size|The binary log usage of an RDS instance. Unit: MB.|
|mysql\_perf\_mem\_usage|The memory utilization of an RDS instance. Unit: percent.|
|mysql\_perf\_open\_tables|The number of opened tables.|
|mysql\_perf\_other\_size|The other space usage of an RDS instance. Unit: MB.|
|mysql\_perf\_qps|The average number of times that SQL statements are executed per second.|
|mysql\_perf\_slow\_queries|The average number of slow queries per second.|
|mysql\_perf\_tb\_tmp\_disk|The number of temporary tables that are automatically created per second in the hard disk when MySQL statements are executed.|
|mysql\_perf\_threads\_connected|The connection number of MySQL threads.|
|mysql\_perf\_threads\_running|The MySQL active threads.|
|mysql\_perf\_tmp\_size|The temporary space usage of an RDS instance. Unit: MB.|
|mysql\_perf\_total\_session|The total number of active connections.|
|mysql\_perf\_tps|The average number of transactions per second.|

