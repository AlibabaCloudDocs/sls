# Usage notes

The log audit feature of ApsaraDB for Redis allows you to collect and send audit logs, slow query logs, and operational logs to Log Service. You can query, ship, and transform the collected logs. You can also visualize log analysis results and configure alerts for the logs. This topic describes the assets, billing, and limits of the log audit feature in ApsaraDB for Redis.

## Assets

-   Dedicated project and Logstores

    After you enable the log audit feature of ApsaraDB for Redis, Log Service creates a project named nosql-Alibaba Cloud account ID-region ID and Logstores named redis\_audit\_log and redis\_slow\_run\_log.

    -   The redis\_audit\_log Logstore is used to store the audit logs of ApsaraDB for Redis.
    -   The redis\_slow\_log Logstore is used to store the slow query logs and operational logs of ApsaraDB for Redis.
-   Dedicated dashboards

    **Note:** We recommend that you do not make changes to the dedicated dashboards because this may affect the usability of the dashboards. You can customize a dashboard to display query results. For more information, see [Create a dashboard](/intl.en-US/Visualization/Dashboard/Create a dashboard.md).

    -   After you enable the log audit feature, Log Service generates a dashboard for the redis\_audit\_log Logstore by default.

        |Dashboard|Description|
        |---------|-----------|
        |Redis Audit Center|Shows the statistics of the audit logs of ApsaraDB for Redis. The information displayed on the dashboard includes the number of users, clients, and audit log entries, average response time \(RT\), and average queries per second \(QPS\).|

    -   After you enable the log audit feature, Log Service generates a dashboard for the redis\_slow\_run\_log Logstore by default.

        |Dashboard|Description|
        |---------|-----------|
        |Redis Slow Log Center|Shows the statistics of the slow query logs of ApsaraDB for Redis. The information displayed on the dashboard includes the number of users, clients, and audit log entries, average RT, and average QPS.|


## Billing

-   You are charged for ApsaraDB for Redis instances based on the storage space and data retention period. The billing varies by region. For more information, see [Billing items and pricing](/intl.en-US/Pricing/Billing items and pricing.md).

    You are not charged when you write, store, and query slow query logs and operational logs.

-   After logs are pushed from ApsaraDB for Redis to Log Service, you are charged for data transformation and log shipping. You are also charged for the read traffic over the Internet. For more information, see [Log Service pricing](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).

## Limits

-   You can write only the logs of ApsaraDB for Redis to a dedicated Logstore. You cannot modify the attributes of a dedicated Logstore. In addition, you cannot modify or delete the indexes of a dedicated Logstore.
-   You cannot modify the data retention period of a dedicated Logstore by using the Log Service console, API, or SDK. You can modify the data retention period of a dedicated Logstore in the ApsaraDB for Redis console.
-   If you have overdue payments for your Log Service resources, the log analysis feature is automatically stopped. To ensure service continuity, you must pay your overdue payment within the prescribed time limit.
-   The log audit feature is available for only ApsaraDB for Redis instances whose version is Redis 4.0 or later and minor version is the latest. The editions of ApsaraDB for Redis include Community Edition and Enhanced Edition.

