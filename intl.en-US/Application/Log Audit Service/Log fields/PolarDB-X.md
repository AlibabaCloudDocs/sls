# PolarDB-X

This topic describes the fields of SQL audit logs in PolarDB-X.

|Log field|Description|
|---------|-----------|
|\_\_topic\_\_|The topic of a log entry. Valid value: drds\_audit\_log.|
|instance\_id|The ID of a PolarDB-X instance.|
|instance\_name|The name of a PolarDB-X instance.|
|owner\_id|The ID of an Alibaba Cloud account.|
|region|The region where a PolarDB-X instance resides.|
|db\_name|The name of a PolarDB-X database.|
|user|The name of a user who executes an SQL statement.|
|client\_ip|The IP address of a client that accesses a PolarDB-X instance.|
|client\_port|The port number of a client that accesses a PolarDB-X instance.|
|sql|The SQL statement.|
|trace\_id|The trace ID of an SQL statement when the statement is executed. If a transaction is executed, it is tracked by using an ID. The ID consists of the trace ID, a hyphen \(-\), and a number, for example, drdsabcdxyz-1.|
|sql\_code|The hash value of a template SQL statement.|
|hint|The hint that is used to execute an SQL statement.|
|table\_name|The names of the tables that are involved in a query. Multiple tables are separated by commas \(,\).|
|sql\_type|The type of an SQL statement. Valid values: Select, Insert, Update, Delete, Set, Alter, Create, Drop, Truncate, Replace, and Other.|
|sql\_type\_detail|The name of an SQL parser.|
|response\_time|The response time. Unit: microseconds.|
|affect\_rows|The number of affected or returned rows when an SQL statement is executed.|
|fail|Indicates the result that is returned after an SQL statement is executed. Valid values:-   0: successful
-   1: failed |
|sql\_time|The time when an SQL statement is executed.|

