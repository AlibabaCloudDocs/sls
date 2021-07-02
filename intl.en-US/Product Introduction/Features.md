# Features

This topic describes the features of Log Service.

## Data collection

Log Service can collect the following types of data from more than 50 data sources:

-   Logs, time series data, and traces from servers and applications
-   Logs from IoT devices
-   Logs from Alibaba Cloud services
-   Data from mobile clients
-   Data from open source software such as Logstash, Flume, Beats, FluentID, and Telegraph
-   Data transferred over protocols such as HTTP, HTTPS, Syslog, Kafka, and Prometheus

![Data cleansing](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6757125261/p2357.png)

For more information, see [Log collection methods](/intl.en-US/Data Collection/Log collection methods.md).

## Query and analysis

Log Service supports data query and analysis in real time.

-   Log Service supports exact query, fuzzy query, full-text query, and field query.
-   Log Service supports features such as contextual query, LogReduce, LiveTail, and reindexing.
-   Log Service supports the SQL-92 syntax.
-   Log Service provides dedicated SQL instances.

![DevOps and online O&M](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7757125261/p2364.png)

For more information, see [Query and analyze logs](/intl.en-US/Index and query/Log search.md).

## Data transformation

You can use the data transformation feature to standardize, enrich, forward, mask, and filter data.

-   Data standardization: Log Service can extract fields from different formats of logs and convert the logs into structured data for stream processing and computing in data warehouses.
-   Data enrichment: Log Service joins the fields of logs, such as order logs, and dimension tables, such as user information tables, to add more log information for data analysis.
-   Data transfer: Log Service transfers logs from regions outside China to a central region by using the cross-region acceleration feature. This way, global logs can be managed in a centralized manner.
-   Data masking: Log Service can mask sensitive information, such as passwords, mobile phone numbers, and addresses, that are contained in data.
-   Data filtering: Log Service can filter the logs of key services for further analysis.

![Data transformation](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7757125261/p273484.png)

For more information, see [Data transformation](/intl.en-US/Data Transformation/Overview.md).

## Consumption and shipping

The consumption and shipping feature allows you to consume data in real time by using SDKs and the Log Service API. You can also ship logs to other Alibaba Cloud services, such as Object Storage Service \(OSS\) and MaxCompute, in real time.

-   You can ship logs to other Alibaba Cloud services, such as OSS, MaxCompute, AnalyticDB, TSDB, and Tablestore for storage and computing.
-   You can ship logs to third-party software, such as Splunk, QRadar, SOC, Fluentd, and Flume for storage and computing.
-   You can consume data by using Java, Python, GO, and C ++.
-   You can perform stream computing by using Flink, Spark, Storm, and StreamCompute.

![消费与投递](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5228125261/p286720.png)

For more information, see [Log consumption and shipping](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Overview.md).

## Visualization

Log service allows you to visualize query and analysis results on a chart.

-   Built-in charts in dashboards: Log Service provides various statistical charts, such as tables, line charts, and bar charts. You can select chart types to visualize query and analysis results on a dashboard as required and save the results to the dashboard.
-   Third-party visualization software: Log Service is compatible with third-party visualization tools, such as Grafana and DataV.

![Visualization](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7757125261/p277249.png)

For more information, see [Visualization](/intl.en-US/Visualization/Analysis graph/Overview.md).

## Alerting

You can use the alerting feature of Log Service to monitor alert data, manage alert incidents, and configure silence policies and notification methods.

-   Alert monitoring: The alert monitoring system can regularly check and evaluate query and analysis results based on alert monitoring rules, trigger alerts or recovery notifications, and send the alerts or notifications to the alert management system.
-   Alert management: The alert management system dispatches, suppresses, deduplicates, denoises, and merges alerts based on alert policies. Then, the alerts are sent to the notification management system.
-   Notification management: The notification management system sends notifications to specified recipients by using specified notification methods. Recipients can be users, user groups, or on-duty groups.
-   Alert ingestion: The alert ingestion system receives alerts from external monitoring systems such as Grafana and Prometheus by using webhook URLs. After the system receives the alerts, you can manage the alerts and send alert notifications.

![Alerting architecture](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3419872261/p261972.png)

For more information, see [Alerts](/intl.en-US/Alerting/Alerting (New)/Feature overview/The alerting feature of Log Service.md).

## Log audit

The Log Audit Service application provides all features of Log Service and supports automated collection and third-party cloud services.

-   The Log Audit Service application can automatically collect and audit cloud service logs across Alibaba Cloud accounts in a centralized manner in real time.
-   You can use the Log Audit Service application to audit logs that are collected from the following Alibaba Cloud services: ActionTrail, Container Service for Kubernetes \(ACK\), OSS, Apsara File Storage NAS, Server Load Balancer \(SLB\), API Gateway, ApsaraDB RDS, PolarDB-X, PolarDB for MySQL, Web Application Firewall \(WAF\), Anti-DDoS, Cloud Firewall, and Security Center.
-   You can also audit logs that are collected from third-party cloud services and self-managed security operations centers \(SOCs\).
-   The Log Audit Service application provides hundreds of built-in alert rules. You can enable the alert rules with only a few clicks. The alert rules help you monitor the compliance of hosts, databases, networks, and logs in account security and permission management.

![Log audit](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7757125261/p273493.png)

For more information, see [Log Audit Service](/intl.en-US/Application/Log Audit Service/Background information.md).

