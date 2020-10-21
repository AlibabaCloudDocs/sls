# Log fields

This topic describes the log fields of an RDS SQL audit log.

|Field|Description|
|-----|-----------|
|\_\_topic\_\_|The topic of the log. Valid value: rds\_audit\_log.|
|instance\_id|The ID of an ApsaraDB for RDS instance.|
|check\_rows|The number of scanned rows.|
|db|The name of the database.|
|fail|Indicates the result after an SQL statement is executed. Valid values: -   0: successful
-   1: failed |
|client\_ip|The IP address of the client that accesses the ApsaraDB for RDS instance.|
|latency|The network latency. Unit: ms.|
|origin\_time|The time when the SQL statement is executed. The time is in the UNIX timestamp format. Unit: ms.|
|return\_rows|The number of rows that the SQL statement returns.|
|sql|The SQL statement.|
|thread\_id|The ID of the thread.|
|user|The name of the user who executes the SQL statement.|
|update\_rows|The number of updated rows.|

