# Query and analysis

This topic describes the limits of query and analysis in Log Service.

## Query

|Item|Description|Remarks|
|:---|:----------|:------|
|Number of keywords|You can specify a maximum of 30 keywords for each query. If you use Boolean Logical operators together with keywords, the operators are not counted as keywords.|None|
|Size of a field value|The maximum size of a field value is 10 KB. The excess part is not queried.|If the size of a field value is greater than 10 KB, log data may fail to be retrieved by using keywords. However, the data is complete.|
|Concurrent queries in a project|You can perform a maximum of 100 queries in a single project at the same time.|None|
|Number of returned log entries|By default, a maximum of 100 log entries are returned for each query.|None|
|Log entry display|A maximum of 10,000 characters in a field value can be split by using delimiters due to browser performance limits.|If a field value in a log entry contains more than 10,000 characters, the console returns the following message: The log contains log data of more than 10,000 characters, and some display will be downgraded.|
|Fuzzy search|If you perform a fuzzy search, Log Service finds a maximum of 100 words in log entries that meet the specified conditions. Log entries that contain the words and meet the search conditions are returned.|None|
|Query result sorting|By default, query results are displayed from the log entries that are generated the latest. The time at which a log entry is generated is accurate to the minute.|None|

## Analytics

|Item|Standard instance|
|----|-----------------|
|Number of concurrent analytic statements|Each project supports a maximum of 15 concurrent analytic statements at a time. For example, 15 users can execute analytic statements in all Logstores of a project at the same time. |
|Data volume|Each shard supports only 1 GB of data for a single analytic statement.|
|Method to enable|Standard instances are enabled by default.|
|Resource usage fee|Free of charge.|
|Applicable scope|You can analyze only the data that is written to Log Service after the log analysis feature is enabled. If you want to analyze historical data, you must re-index the historical data. For more information, see [Reindex logs for a Logstore](/intl.en-US/Index and query/Query/Reindex logs for a Logstore.md). |
|Returned result|After you execute an analytic statement, a maximum of 100 rows of data are returned by default. If you require more data, use the LIMIT clause. For more information, see [LIMIT syntax](/intl.en-US/Index and query/Analysis grammar/LIMIT syntax.md). |
|Size of a field value|The maximum size of a field value is 16 KB. If the size of a field value exceeds 16 KB, the excess content is not analyzed.|
|Timeout period|The maximum timeout period for a single analytic statement is 55 seconds.|
|Number of digits that consists of a field value of the double type|Each field value of the double type consists of a maximum of 52 digits. If the number of digits is greater than 52, the accuracy of the field value is compromised. |

