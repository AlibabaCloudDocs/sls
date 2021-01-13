# Usage notes

The SQL audit and analysis feature of Distributed Relational Database Service \(DRDS\) allows you to collect SQL audit logs and send the logs to Log Service. You can query, ship, and transform the collected logs. You can also visualize log analysis results and configure alerts for the logs. This topic describes the assets, billing, and limits of the SQL audit and analysis feature in DRDS.

## Assets

-   Dedicated projects and Logstores

    After you enable the SQL audit and analysis feature, Log Service creates a project named drds-audit-region name-Alibaba Cloud account ID and a Logstore named drds-audit-log.

-   Dedicated dashboards

    After you enable the SQL audit and analysis feature, Log Service generates three dashboards by default.

    **Note:** We recommend that you do not make changes to the dedicated dashboards because this may affect the usability of the dashboards. You can create a custom dashboard to visualize log analysis results. For more information, see [Create a dashboard](/intl.en-US/Query and visualization/Dashboard/Create a dashboard.md).

    |Dashboard|Description|
    |---------|-----------|
    |Operation Center \(Simple\)|Shows the statistics of DRDS instances, including the metrics, distribution, and trends of SQL statement executions.|
    |Performance Center|Shows the performance metrics, average time consumed by each type of SQL statement, and the distribution and sources of slow SQL queries.|
    |Security Center|Shows the security metrics, batch delete events, and malicious SQL executions.|


## Billing

-   You are not charged for the SQL audit and analysis feature in the DRDS console.
-   After SQL audit logs are sent to Log Service, you are charged by Log Service based on the storage space, read traffic, the number of requests, data transformation, and data shipping. For more information, see [Log Service pricing](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).

## Limits

-   You can write only DRDS SQL audit logs to a dedicated Logstore. In addition, you cannot modify the indexes in a dedicated Logstore.
-   The SQL audit and analysis feature of DRDS is available in the following regions: China \(Hangzhou\), China \(Shanghai\), China \(Qingdao\), China \(Beijing\), China \(Zhangjiakou\), China \(Hohhot\), China \(Shenzhen\), China \(Hong Kong\), and Singapore \(Singapore\).

    The Log Audit Service application of Log Service also supports access to DRDS SQL audit logs. For more information, see [Log Audit Service](/intl.en-US/Application/Log Audit Service/Overview.md).


