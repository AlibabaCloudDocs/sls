# Log fields

Redis logs include audit logs, slow logs, and operational logs. This topic describes the fields of these logs.

## Audit logs

The audit logs are stored in the Logstore named redis\_audit\_log. The following table describes the log fields.

|Field|Description|
|-----|-----------|
|\_\_topic\_\_|The topic of the log entry.-   redis\_audit\_log: audit logs of the database
-   redis\_proxy\_audit\_log: audit logs of the proxy |
|account|The name of the database account.|
|command|The Redis command that is run on the database.|
|db|The name of the database.|
|extend\_information|The additional information.|
|instanceid|The ID of the Redis instance.|
|ip|The IP address of the database server.|
|is\_cautious|Indicates whether the operation is dangerous. Valid values:-   0: No
-   1: Yes |
|latency|The request latency.|
|time|The timestamp, for example, 1597048424.|
|type|The type of the log entry.|

## Slow logs

The slow logs are stored in the Logstore named redis\_slow\_run\_log. The following table describes the log fields.

|Field|Description|
|-----|-----------|
|\_\_topic\_\_|The topic of the log entry.-   redis\_audit\_log: audit logs of the database
-   redis\_proxy\_audit\_log: audit logs of the proxy |
|account|The name of the database account.|
|command|The Redis command that is run on the database.|
|db|The name of the database.|
|extend\_information|The additional information.|
|instanceid|The ID of the Redis instance.|
|ip|The IP address of the database server.|
|is\_cautious|Indicates whether the operation is dangerous. Valid values:-   0: No
-   1: Yes |
|latency|The request latency.|
|time|The timestamp, for example, 1597048424.|
|type|The type of the log entry.|

## Operational logs

The operational logs are stored in the Logstore named redis\_slow\_run\_log. The following table describes the log fields.

|Field|Description|
|-----|-----------|
|\_\_topic\_\_|The topic of the log entry. Valid value: redis\_run\_log.|
|extend\_information|The additional information.|
|instanceid|The ID of the Redis instance.|
|node\_type|The type of the log entry.-   proxy: operational logs of the proxy
-   db: operational logs of the database |
|runlog|The content of the operational log entry.|
|time|The timestamp, for example, 1597048424.|

