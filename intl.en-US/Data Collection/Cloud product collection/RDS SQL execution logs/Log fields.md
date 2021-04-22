# Log fields

This topic describes the fields of audit log entries for an ApsaraDB RDS for MySQL instance.

|Log field|Description|
|---------|-----------|
|\_\_topic\_\_|The topic of a log entry. Valid value: rds\_audit\_log.|
|instance\_id|The ID of an RDS instance.|
|check\_rows|The number of scanned rows.|
|db|The name of an RDS database.|
|fail|Indicates the result after an SQL statement is executed. Valid values: -   0: successful
-   1: failed |
|client\_ip|The IP address of a client that accesses an RDS instance.|
|latency|The latency of a result that is returned after an SQL statement is executed. Unit: microseconds.|
|origin\_time|The time when an SQL statement is executed.|
|return\_rows|The number of returned rows.|
|sql|The SQL statement.|
|thread\_id|The ID of a thread.|
|user|The username of a user who executes an SQL statement.|
|update\_rows|The number of updated rows.|

