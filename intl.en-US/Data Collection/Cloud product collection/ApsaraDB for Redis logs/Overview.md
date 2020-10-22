# Overview

ApsaraDB for Redis provides the log audit feature supported by Log Service. You can use this feature to query audit logs, slow logs, and operational logs of ApsaraDB for Redis and visualize the query results. You can also create alerts for the logs, ship, and transform the logs. This topic describes the resources, billings, and limits that are related to the log audit feature.

## Resources

-   Dedicated project and Logstores

    If you enable the log audit feature of ApsaraDB for Redis, a project named in the format of nosql-Alibaba Cloud account ID-region ID and Logstores named redis\_audit\_log and redis\_slow\_run\_log are created.

    -   The redis\_audit\_log Logstore is used to store Redis audit logs.
    -   The redis\_slow\_log Logstore is used to store slow logs and operational logs of Redis.
-   Dedicated dashboards

    **Note:** We recommend that you do not make changes to the dedicated dashboards because this may affect the usability of the dashboards. You can create a custom dashboard to visualize the results of log queries. For more information, see [Create a dashboard](/intl.en-US/Query and visualization/Dashboard/Create a dashboard.md).

    -   A dashboard is automatically generated for the redis\_audit\_log Logstore.

        |Dashboard|Description|
        |---------|-----------|
        |Redis Audit Center|Displays audit log of ApsaraDB for Redis, including the number of users, clients, and audit log entries, average response time \(RT\), and average request rate.|

    -   A dashboard is automatically generated for the redis\_slow\_log\_logstore Logstore.

        |Dashboard|Description|
        |---------|-----------|
        |Redis Slow Log Center|Displays slow logs of ApsaraDB for Redis, including the number of users, clients, and audit log entries, average response time \(RT\), and average request rate.|


## Billing

-   The log audit feature of ApsaraDB for Redis includes the free trial version and official version. The free trial version is available and is free of charge.
-   After logs are shipped to Log Service, you can transform and ship the logs. You can also read the logs in streaming mode over the external network. Bills are generated in Log Service for data transformation, shipping, and reading. For more information, see [Log Service pricing](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).

## Limits

-   You can write only Redis logs to a dedicated Logstore. In addition, you cannot modify the indexes in a dedicated Logstore.
-   You cannot delete dedicated projects or Logstores.
-   The retention period of data in a dedicated Logstore is one day and cannot be modified.
-   If you have overdue payments for your Log Service resources, the log analysis feature is automatically stopped. To ensure service continuity, you must pay your overdue payment within the prescribed time limit.
-   The log audit feature is available for ApsaraDB for Redis Community Edition and Enhanced Edition 4.0 or later.

