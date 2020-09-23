# Usage notes

Alibaba Cloud Object Storage Service \(OSS\) provides the real-time log query feature that is supported by Log Service. You can use this feature to audit operations performed on OSS, analyze access requests, track anomaly events, and locate errors and exceptions. This topic describes the resources, billings, and limits that are related to the real-time log query feature.

## Resources

-   Dedicated project and Logstore

    If you enable the log query feature of OSS, a project named in the format of oss-Alibaba Cloud account ID-region ID and a Logstore named oss-log-store are created.

-   Dedicated dashboards

    After you enable the feature, four dedicated dashboards are automatically created for OSS access logs.

    **Note:** We recommend that you do not make changes to the dedicated dashboards because this may affect the usability of the dashboards. You can create a custom dashboard to visualize the results of log queries. For more information, see [Create a dashboard](/intl.en-US/Query and visualization/Dashboard/Create a dashboard.md).

    |Dashboard|Description|
    |---------|-----------|
    |oss-log-store\_access\_center|Displays the overall operational statistics of OSS, including the page views \(PVs\), \(unique views\) UVs, traffic, and distribution of requests over the external network.|
    |oss-log-store\_audit\_center|Displays the statistics of operations performed on OSS objects, including the read, write, and delete operations.|
    |oss-log-store\_operation\_center|Displays the information of OSS O&M operations, including the number of requests and distribution of failed operations.|
    |oss-log-store\_performance\_center|Displays the statistics of OSS performance, including the performance of downloads and uploads over the external network, transmission performance over different networks or of different object sizes, and list of differences between object downloads.|


## Billing

-   You can use the real-time log query feature to query OSS logs that are generated in the last 7 days free of charge. You can also write a maximum of 900 GB of OSS logs to a dedicated Logstore free of charge. If the size of a log entry is 1 KB, you can write 900 million log entries to a dedicated Logstore. Excess data is charged based on the billing methods of Log Service. If you set a log retention period longer than 7 days, you are charged for the data storage in the excess days based on the billing methods of Log Service. For more information, see [Billing methods](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).
-   You can use a maximum of 16 shards in a dedicated Logstore to store and query OSS logs free of charge. Excess shards are charged based on the billing methods of Log Service.
-   You are charged for reading data from a dedicated Logstore over the external network. You are also charged for transforming and shipping data. For more information, see [Pay-as-you-go](/intl.en-US/Pricing/Pay-as-you-go.md).

## Limits

-   You can write only OSS access logs to a dedicated Logstore. In addition, you cannot modify the indexes in a dedicated Logstore.
-   A dedicated Logstore cannot be deleted.
-   If you have an overdue payment, the real-time log query feature is unavailable.

