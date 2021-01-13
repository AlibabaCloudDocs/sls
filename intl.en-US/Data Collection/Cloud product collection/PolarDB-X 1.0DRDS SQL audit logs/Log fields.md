# Log fields

This topic describes the fields of Distributed Relational Database Service \(DRDS\) SQL audit log entries.

|Log field|Description|
|:--------|:----------|
|\_\_topic\_\_|The topic of a log entry. The value must be in the format of drds\_audit\_log\_instance ID\_database name, for example, drds\_audit\_log\_drdsxyzabcd\_demo\_drds\_db.|
|affect\_rows|The number of rows that are returned after an SQL statement is executed.-   The number of rows that are affected when an SQL statement is executed to add, delete, or modify data.
-   The number of rows that are returned after a query statement is executed.

This field is available for instance version 5.3.4-15378085 and later. |
|client\_ip|The IP address of a client that accesses a DRDS instance.|
|client\_port|The port number of a client that accesses a DRDS instance.|
|db\_name|The name of a DRDS database.|
|fail|The result of an SQL statement. Valid values:-   0: The SQL statement is executed.
-   1: The SQL statement fails to be executed.

This field is available for instance version 5.3.4-15378085 and later. |
|hint|The hint that is used to execute an SQL statement.|
|instance\_id|The ID of a DRDS instance.|
|response\_time|The response time. Unit: milliseconds.This field is available for instance version 5.3.4-15378085 and later. |
|sql|The SQL statement.|
|sql\_code|The hash value of a template SQL statement.|
|sql\_time|The time when an SQL statement is executed. The value is in the format of yyyy-MM-dd HH:mm:ss.SSS.|
|sql\_type|The type of an SQL statement. Valid values: Select, Insert, Update, Delete, Set, Alter, Create, Drop, Truncate, Replace, and Other.|
|sql\_type\_detail|The name of an SQL parser.|
|table\_name|The name of a database table. Multiple tables are separated by commas \(,\).|
|trace\_id|The trace ID of an SQL statement when the statement is executed. If a transaction is executed, it is tracked by using an ID. The ID consists of a trace ID, a hyphen \(-\), and a number, for example, drdsabcdxyz-1 and drdsabcdxyz-2.|
|user|The name of the user who executes an SQL statement.|

