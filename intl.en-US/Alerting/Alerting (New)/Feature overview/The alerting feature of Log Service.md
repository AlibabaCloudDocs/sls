# The alerting feature of Log Service

You can use the alerting feature of Log Service to monitor alert data, manage alert incidents, and configure silence policies and notification methods.

## Architecture

The alerting feature of Log Service consists of the alert monitoring system, alert management system, and notification management system. The following figure shows the detailed information of the alerting feature.

![Alert architecture](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3419872261/p261972.png)

The alerting feature of Log Service consists of the following components:

-   Logstore: Log Service provides Logstores to store log data. You can use the SQL-92 syntax to query log data. Monitoring tasks are based on the log search and analysis feature.
-   Metricstore: Log Service provides Metricstores to store time series data. You can use the PromQL syntax and SQL-92 syntax to analyze time series data. Monitoring tasks are based on the log search and analysis feature.
-   Alert monitoring system: The alert monitoring system is a subsystem that is used to trigger alerts. The alert monitoring system consists of alert monitoring rules and metadata.
-   Alert management system: The alert management system is a subsystem that is used to manage denoising policies and alert statuses. The alert management system consists of alert policies, alert incidents, and alert dashboards.
-   Notification management system: The notification management system is a subsystem that is used to manage alert notification methods and alert recipients. The notification management system consists of action policies, alert templates, calendars, users, user groups, on-duty groups, and notification method quota.
-   Alert ingestion system: The alert ingestion system is a subsystem that is used to ingest external alerts. Alert ingestion consists of alert ingestion services and alert ingestion applications.

## Benefits

-   Ease of use

    Log Service allows you to import, store, query, analyze, visualize, and configure alerts for log data and time series data. After you import log data or time series data, you can create monitoring tasks and configure notification methods and alert policies within minutes. This way, recipients can receive alert notifications in a timely manner.

    You can also use alert resources in different scenarios and for different teams.

-   High availability and reliability

    The alerting feature of Log Service is based on the high availability and data reliability of Log Service. The availability of the alert-related services is 99.9%. The reliability of alert-related data is higher than 99.99999999%.

-   Low costs and maintenance-free

    You are charged only a small fee when you receive alert notifications by using SMS messages and voice calls. You are not charged when you use the alerting feature to monitor data or manage alert incidents.

    A SaaS-based alerting feature provides a more comprehensive and intelligent alert monitoring and event management capabilities. This improves cost and time efficiency.

-   Quick responses

    A more comprehensive and intelligent alert monitoring and event management capability shortens the time to send alert notifications. This way, alert exceptions can be detected and resolved in a timely manner.


## Billing method

The new alerting feature is in public preview, you are charged only when you receive alert notifications by using SMS messages and voice calls. For more information, see[Log Service pricing](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).

|Notification method|Description|
|-------------------|-----------|
|SMS messages|Fees are incurred when you receive notifications by using SMS messages. You are charged based on the number of times that you receive alert notifications. **Note:** If an SMS message is longer than 70 characters, it may be sent in two messages. In this case, you are charged only once |
|Voice calls|Fees are incurred when you receive notifications by using voice calls. You are charged based on the times that you receive alert notifications. **Note:**

-   If a voice call is not answered, an SMS notification is sent.
-   You are charged only once for the voice call whether the call is answered or not. The notification message does not incur fees. |

