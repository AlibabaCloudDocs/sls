# PolarDB for MySQL

This topic describes the fields of PolarDB for MySQL logs. PolarDB for MySQL logs include audit logs, slow query logs, and performance logs.

## Audit logs

|Log field|Description|
|---------|-----------|
|\_\_topic\_\_|The topic of a log entry. Valid value: polardb\_audit\_log.|
|owner\_id|The ID of an Alibaba Cloud account.|
|region|The region where a PolarDB for MySQL cluster resides.|
|cluster\_id|The ID of a PolarDB for MySQL cluster.|
|node\_id|The ID of a node in a PolarDB for MySQL cluster.|
|check\_rows|The number of scanned rows.|
|db|The name of a database.|
|fail|Indicates the result that is returned after an SQL statement is executed. Valid values:-   0: successful
-   1: failed |
|client\_ip|The IP address of a client that accesses a PolarDB for MySQL cluster.|
|latency|The latency of a result that is returned after an SQL statement is executed. Unit: microseconds.|
|origin\_time|The time when an SQL statement is executed.|
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
|region|The region where a PolarDB for MySQL cluster resides.|
|cluster\_id|The ID of a PolarDB for MySQL cluster.|
|node\_id|The ID of a node in a PolarDB for MySQL cluster.|
|db\_type|The type of a PolarDB database.|
|db\_name|The name of a PolarDB database.|
|version|The version number of a PolarDB database.|
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
|mysql\_perf\_active\_session|The number of active connections per second.|
|mysql\_perf\_binlog\_size|The local binary log usage. Unit: MB.|
|mysql\_perf\_com\_delete|The average number of times that the DELETE statements are executed per second.|
|mysql\_perf\_com\_delete\_multi|The average number of times that the Multi DELETE statements are executed per second.|
|mysql\_perf\_com\_insert|The average number of times that the INSERT statements are executed per second.|
|mysql\_perf\_com\_insert\_select|The average number of times that the INSERT SELECT statements are executed per second.|
|mysql\_perf\_com\_replace|The average number of times that the REPLACE statements are executed per second.|
|mysql\_perf\_com\_replace\_select|The average number of times that the REPLACE SELECT statements are executed per second.|
|mysql\_perf\_com\_select|The average number of times that the SELECT statements are executed per second.|
|mysql\_perf\_com\_update|The average number of times that the UPDATE statements are executed per second.|
|mysql\_perf\_com\_update\_multi|The average number of times that the Multi UPDATE statements are executed per second.|
|mysql\_perf\_cpu\_ratio|The CPU utilization. Unit: percent.|
|mysql\_perf\_created\_tmp\_disk\_tables|The number of temporary tables that are automatically created per second.|
|mysql\_perf\_data\_size|The data usage. Unit: MB.|
|mysql\_perf\_innodb\_buffer\_dirty\_ratio|The dirty ratio of a buffer pool. Unit: percent.|
|mysql\_perf\_innodb\_buffer\_read\_hit|The read hit ratio of a buffer pool. Unit: percent.|
|mysql\_perf\_innodb\_buffer\_use\_ratio|The utilization of a buffer pool. Unit: percent.|
|mysql\_perf\_innodb\_data\_read|The amount of data that is read from a storage engine per second. Unit: bytes.|
|mysql\_perf\_innodb\_data\_reads|The average number of read operations that are performed on a buffer pool per second.|
|mysql\_perf\_innodb\_data\_writes|The average number of write operations that are performed on the buffer pool per second.|
|mysql\_perf\_innodb\_data\_written|The amount of data that is written to a storage engine per second. Unit: bytes.|
|mysql\_perf\_innodb\_log\_write\_requests|The average number of log write requests per second.|
|mysql\_perf\_innodb\_os\_log\_fsyncs|The average number of write operations to a log file by calling the fsync\(\) function.|
|mysql\_perf\_innodb\_rows\_deleted|The number of deleted rows per second.|
|mysql\_perf\_innodb\_rows\_inserted|The number of inserted rows per second.|
|mysql\_perf\_innodb\_rows\_read|The number of read rows per second.|
|mysql\_perf\_iops|The IOPS.|
|mysql\_perf\_iops\_r|The read IOPS per second.|
|mysql\_perf\_iops\_throughput|The I/O throughput per second. Unit: MB.|
|mysql\_perf\_iops\_throughput\_r|The read I/O throughput per second. Unit: MB.|
|mysql\_perf\_iops\_throughput\_w|The write I/O throughput per second. Unit: MB.|
|mysql\_perf\_iops\_w|The write IOPS per second.|
|mysql\_perf\_kbytes\_received|The average inbound traffic per second. Unit: KB.|
|mysql\_perf\_kbytes\_sent|The average outbound traffic per second. Unit: KB.|
|mysql\_perf\_mem\_ratio|The memory utilization. Unit: percent.|
|mysql\_perf\_mps|The number of data operations per second.|
|mysql\_perf\_other\_log\_size|The usage of other logs. Unit: MB.|
|mysql\_perf\_qps|The number of requests per second.|
|mysql\_perf\_redolog\_size|The usage of local redo logs. Unit: MB.|
|mysql\_perf\_slow\_queries|The number of slow queries per second.|
|mysql\_perf\_sys\_dir\_size|The usage of the system space. Unit: MB.|
|mysql\_perf\_tmp\_dir\_size|The usage of the temporary space. Unit: MB.|
|mysql\_perf\_total\_session|The average number of total connections.|
|mysql\_perf\_tps|The average number of transactions per second.|

