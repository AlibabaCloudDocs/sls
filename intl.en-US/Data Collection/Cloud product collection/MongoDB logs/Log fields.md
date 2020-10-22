# Log fields

MongoDB logs include audit logs, slow logs, and operational logs. This topic describes the fields of these logs.

## Audit logs

The audit logs are stored in the Logstore named mongo\_audit\_log. The following table describes the log fields.

**Note:** The names of the fields in audit log and slow logs are the same. You can distinguish the two types of logs based on the value of the audit\_type field. The value of the audit\_type field in slow logs is slowop. If the value of this field in a log entry is not slowop, this log entry is an audit log entry.

|Field|Description|
|-----|-----------|
|\_\_topic\_\_|The topic of the log entry. Valid value: mongo\_audit\_log.|
|audit\_type|The type of the log entry, for example, Command.|
|coll|The dataset.|
|db|The name of the database.|
|docs\_examined|The number of scanned rows.|
|instanceid|The ID of the ApsaraDB for MongoDB instance.|
|keys\_examined|The number of rows of scanned indexes.|
|latency|The response latency.|
|optype|The type of the operation. Valid values:-   query: query data
-   find: search for data
-   insert: insert data
-   update: update data
-   delete: delete data
-   remove: remove data
-   getMore: read data
-   command: operation command |
|return\_num|The number of entries returned.|
|thread\_id|The ID of the thread.|
|time|The timestamp.|
|user|The account that is used to log on to the ApsaraDB for MongoDB database.|
|user\_ip|The IP address of the client used to access the ApsaraDB for MongoDB instance.|

## Slow query logs

The slow logs are stored in the Logstore named mongo\_slow\_run\_log. The following table describes the log fields.

|Field|Description|
|-----|-----------|
|\_\_topic\_\_|The topic of the log entry. Valid value: mongo\_run\_log.|
|audit\_type|The type of the log entry. Valid value: slowop.|
|coll|The dataset.|
|db|The name of the database.|
|docs\_examined|The number of scanned rows.|
|instanceid|The ID of the ApsaraDB for MongoDB instance.|
|keys\_examined|The number of scanned rows.|
|latency|The response latency.|
|optype|The type of the operation. Valid values:-   query: query data
-   find: search for data
-   insert: insert data
-   update: update data
-   delete: delete data
-   remove: remove data
-   getMore: read data
-   command: operation command |
|return\_num|The number of entries returned.|
|thread\_id|The ID of the thread.|
|time|The timestamp.|
|user|The account that is used to log on to the ApsaraDB for MongoDB database.|
|user\_ip|The IP address of the client used to access the ApsaraDB for MongoDB instance.|

## Operational logs

The operational logs are stored in the Logstore named mongo\_slow\_run\_log. The following table describes the log fields.

|Field|Description|
|-----|-----------|
|\_\_topic\_\_|The topic of the log entry. Valid value: mongo\_run\_log.|
|category|The type of the log entry, for example, NETWORK logs.|
|connection|The log connection information.|
|content|The log content.|
|instanceid|The ID of the ApsaraDB for MongoDB instance.|
|ip|The IP address of the database server.|
|level|The severity of the log entry.|
|port|The port number.|
|time|The time when the log entry was generated.|

