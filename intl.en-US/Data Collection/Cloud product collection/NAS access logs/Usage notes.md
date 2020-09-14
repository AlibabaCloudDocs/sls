# Usage notes

The log analysis feature of Apsara File Storage NAS allows you to query, analyze, transform, and consume logs that are generated when you access NAS data. This topic describes the assets, billing, and limits of the log analysis feature in NAS.

## Assets

-   Dedicated projects and dedicated Logstores

    After you enable the log analysis feature of NAS, Log Service creates a project named nas-Alibaba Cloud account ID-region ID and a Logstore named nas-nfs in each region.

    **Note:** To ensure that logs can be collected, do not delete dedicated projects and dedicated Logstores.

-   Dedicated dashboards

    After you enable the log analysis feature of NAS, Log Service generates three dashboards by default.

    **Note:** We recommend that you do not make changes to the dedicated dashboards because this may affect the usability of the dashboards. You can create a custom dashboard to visualize the results of log analysis. For more information, see [Create a dashboard](/intl.en-US/Query and visualization/Dashboard/Create a dashboard.md).

    |Dashboard|Description|
    |---------|-----------|
    |nas-nfs-nas\_summary\_dashboard\_cn|Shows the operational statistics of NAS. The information displayed on the dashboard includes the number of the latest accessed volumes, the number of the latest unique visitors \(UVs\), total write throughput, and total read throughput.|
    |nas-nfs-nas\_audit\_dashboard\_cn|Shows the operational statistics of NAS file systems. The information displayed on the dashboard includes the number of create operations, deleted files, and read files.|
    |nas-nfs-nas\_detail\_dashboard\_cn|Shows the details of NAS file systems. The information displayed on the dashboard includes the number of the latest page views \(PVs\) and operation trend.|


## Billing

-   You are not billed for the log analysis feature in NAS.
-   After NAS access logs are dumped to Log Service, you are billed based on the storage space, read traffic, the number of requests, data transformation, and data shipping in the Log Service console. For more information, see [Log Service pricing](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).

## Limits

-   You can write only NAS access log data to a dedicated Logstore.
-   The log analysis feature supports only a file system that uses the Network File System \(NFS\) protocol.

