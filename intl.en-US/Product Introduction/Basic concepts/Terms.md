# Terms

This topic introduces the terms in Log Service.

## Basic resources

|Term|Description|
|----|-----------|
|Project|A project in Log Service is used to separate different resources of multiple users and control access to specific resources. For more information, see [Project](/intl.en-US/Product Introduction/Basic concepts/Projects.md).|
|Logstore|A Logstore in Log Service is used to collect, store, and query log data. For more information, see [Logstore](/intl.en-US/Product Introduction/Basic concepts/Logstore.md).|
|Metricstore|A Metricstore in Log Service is used to collect, store, and query time series data. For more information, see [Metricstore](/intl.en-US/Product Introduction/Basic concepts/Metricstore.md).|
|Log|Logs are records of changes that occur in a system during the running of the system. The records contain information about the operations that are performed on specific objects and the results of the operations. The records are ordered by time. For more information, see [Log](/intl.en-US/Product Introduction/Basic concepts/Logs.md).|
|Log group|A log group is a collection of logs and is the basic unit that is used to write and read logs. The logs in a log group contain the same metadata, such as the IP address and log source. For more information, see [Log group](/intl.en-US/Product Introduction/Basic concepts/Log groups.md).|
|Time series data|Time series data is a series of data points that are ordered by time. For more information, see [Time series data](/intl.en-US/Product Introduction/Basic concepts/Time series.md).|
|Trace data|Trace data indicates the execution process of an event or a procedure in a distributed system. For more information, see [Trace]().|
|Shard|A shard is used to control the read and write capacity of a Logstore. In Log Service, data is stored in a shard. Each shard has an MD5 hash range and each range is a left-closed and right-open interval. Each range does not overlap with the ranges of other shards. The entire MD5 hash range is within the following range: \[00000000000000000000000000000000, ffffffffffffffffffffffffffffffff\). For more information, see [Shards](/intl.en-US/Product Introduction/Basic concepts/Shards.md).|
|Topic|A topic is a basic management unit in Log Service. You can specify log topics when you collect logs. This way, Log Service classifies logs by log topic. For more information, see [Topic](/intl.en-US/Product Introduction/Basic concepts/Log topics.md).|
|Endpoint|An endpoint of Log Service is a URL that is used to access a project. To access projects in different regions, you must use different endpoints. To access the projects in the same region over the Alibaba Cloud internal network or the Internet, you must also use different endpoints. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|
|AccessKey pair|An AccessKey pair is an identity credential that consists of an AccessKey ID and an AccessKey secret. The AccessKey ID and the AccessKey secret are used for symmetric encryption and identity verification. The AccessKey ID is used to identify a user. The AccessKey secret is used to encrypt and verify a signature string. The AccessKey secret is confidential. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).|
|Region|The region is the physical location where a data center of Log Service is located. You can specify a region when you create a project. After the project is created, you cannot change the region.|

## Data collection

|Term|Description|
|----|-----------|
|Logtail|Logtail is used to collect logs in Log Service. For more information, see [Logtail overview](/intl.en-US/Data Collection/Logtail collection/Overview/Logtail overview.md).|
|Logtail configuration|A Logtail configuration is a set of policies that are used by Logtail to collect logs. The configuration specifies the log source and the collection method. For more information, see [Logtail configuration files](/intl.en-US/Developer Guide/API Reference/Common resources/Logtail configuration files.md).|
|Machine groups|A machine group is a virtual group that contains multiple servers. Log Service uses server groups to manage the servers from which you want to collect logs by using Logtail. For more information, see [machine groups](/intl.en-US/Data Collection/Logtail collection/Machine Group/Introduction.md).|

## Query and analysis

|Term|Description|
|----|-----------|
|Query|You can use search statements to configure a filtering rule. After you configure the rule, Log Service returns logs that meet the conditions in the rule. For more information, see [Log search](/intl.en-US/Index and query/Log search.md).|
|Analysis|You can invoke SQL functions based on query results to obtain analysis results. -   Log Service supports the SQL-92 syntax when you analyze logs. For more information, see [Log analysis](/intl.en-US/Index and query/Log analysis.md).
-   Log Service supports the SQL-92 syntax and the PromQL syntax when you analyze time series data. For more information, see [Overview of time series search and analysis](/intl.en-US/Time Series Storage/Search and analysis/Overview of time series search and analysis.md). |
|Query statement|The format of a query statement is `Search statement | Analytic statement`. A search statement can be separately executed. However, an analytic statement must be executed together with a search statement. The log analysis feature is used to analyze search results or all data in the Logstore. For more information, see [Query and analysis](/intl.en-US/Index and query/Log search.md).|
|Index|Indexes are used in a storage structure to sort one or more columns of log data. You can query and analyze data only after you configure indexes for the data. Log Service provides the following two index types:-   full-text index: Log Service splits an entire log entry into multiple words based on specified delimiters. In a query, specified field names \(keys\) or field values \(values\) are plaintext.
-   field index: After you configure field indexes, you can query log entries. To query log entries, you can specify field name and field value as key-value pairs in the format of key:value.

For more information, see [Configure indexes](/intl.en-US/Index and query/Configure indexes.md). |
|Standard SQL instance|Standard SQL instances are free resources that are used to analyze data by executing SQL statements. Compared with dedicated SQL instances, more limits are imposed on standard SQL instances.|

## Data transformation

|Term|Description|
|----|-----------|
|Domain-specific language \(DSL\)|DSL is a Python-compatible scripting language that is used for data transformation in Log Service. For more information, see [Language introduction](/intl.en-US/Data Transformation/Data processing syntax/Language introduction.md).|
|Transformation rule|A transformation rule is a data transformation script that is orchestrated by using the DSL for Log Service. For more information, see [Syntax introduction](/intl.en-US/Data Transformation/Data processing syntax/Syntax introduction.md).|

## Consumption and shipping

|Term|Description|
|----|-----------|
|Consumer group|You can use consumer groups to consume data in Log Service. A consumer group consists of multiple consumers. Each consumer consumes different log entries that are stored in a Logstore. For more information, see [Use consumer groups to consume log data](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Multi-language applications/Consumption by consumer groups/Use consumer groups to consume log data.md).|

## Alerting

|Term|Description|
|----|-----------|
|Alert|An alert indicates an alert event. If an alert is triggered based on a specified alert monitoring rule, the event is passed to the notification management system. The alerting module of Log Service provides features, entities, submodules, and subsystems. such as the alert monitoring system and alert monitoring rules.

For more information, see [The alerting feature of Log Service](/intl.en-US/Alerting/Alerting (New)/Feature overview/The alerting feature of Log Service.md). |
|Alert monitoring|The alert monitoring system is a subsystem that triggers alerts. The alert monitoring system consists of alert monitoring rules and resource data. An alert monitoring rule is used to periodically evaluate query results. If an alert is triggered or cleared, an alert notification or recovery notification is sent to the alert management system based on the monitoring rule orchestration. |
|Alert management system|The alert management system is a subsystem that manages silence policies and alert statuses. The alert management system consists of alert policies, alert incidents, and alert dashboards. The alert management system dispatches, suppresses, deduplicates, denoises, and merges alerts based on alert policies. The processed alerts are sent to the notification management system. The alert management system also allows you to configure alert statuses and alert handlers. |
|Notification management system|The notification management system is a subsystem that manages notification methods and recipients. The notification management system consists of action policies, alert templates, calendars, users, user groups, on-duty groups, and notification method quotas. The notification management system sends notifications to specified recipients by using specified notification methods. Recipients can be users, user groups, or on-duty groups. The notification management system also allows you to escalate alerts and customize alert templates. |
|Alert ingestion|The alert ingestion system ingests external alerts. The alert ingestion system consists of alert ingestion services and alert ingestion applications. Each alert ingestion application provides an API operation to ingest external alerts, such as recovery notifications from Zabbix and Prometheus. After an external alert is ingested, the alert is processed and sent to the alert management system for further processing. |

