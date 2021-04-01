# Usage notes

This topic describes the limits and billing of the Log Audit Service application.

## Limits

-   Limits on storage methods and regions
    -   Centralized storage

        Logs collected from multiple Alibaba Cloud accounts across regions are stored in a central project of an Alibaba Cloud account. The central project can reside in the following regions.

        **Note:** When you switch the region where the central Alibaba Cloud account resides, Log Service creates a central project in the region. The original project is not deleted.

        -   China \(Beijing\), China \(Hohhot\), China \(Hangzhou\), China \(Shanghai\), and China \(Shenzhen\)
        -   Singapore \(Singapore\) and Japan \(Tokyo\)
    -   Regional storage

        Access logs of Server Load Balancer \(SLB\), Object Storage Service \(OSS\), and PolarDB-X are collected from multiple Alibaba Cloud accounts across different regions. These access logs are stored in the Logstores of a regional project in the region where instances of the three Alibaba Cloud services reside. For example, the access logs collected from an OSS bucket that resides in the China \(Hangzhou\) region are stored in the Logstore of the regional project that resides in the China \(Hangzhou\) region.

    -   Synchronization to a central project

        You can synchronize log data in the Logstores of the regional project for SLB, OSS, and PolarDB-X to a central project. This improves the efficiency when you query, analyze, and visualize the collected logs. You can also configure alerts for the logs and perform secondary development.

        The synchronization process is based on the data transformation feature of Log Service. The supported regions for log data synchronization exclude China \(Qingdao\) and China \(Heyuan\).

-   Limits on resources
    -   Each Alibaba Cloud account has only one central project in a region. The name of the central project is in the format of slsaudit-center-Alibaba Cloud account ID-Region, for example, slsaudit-center-1234567890-cn-beijing. Central projects cannot be deleted in the Log Service console. You can delete a central project by using the command-line interface \(CLI\) or by calling the related API operation.
    -   SLB, OSS, and PolarDB-X have multiple regional projects. The names of the projects that store access logs of SLB instances, OSS buckets, and PolarDB-X instances are in the format of slsaudit-region-Alibaba Cloud account ID-Region, for example, slsaudit-region-1234567890-cn-beijing. Regional projects cannot be deleted in the Log Service console. You can delete a regional project by using the CLI or by calling the related API operation.
    -   If you turn on the related switches on the Audit-Related Logs tab for Alibaba Cloud services, the Log Audit Service application creates one or more dedicated Logstores for data storage. You can manage the dedicated Logstores in the same way that you manage other Logstores. However, the dedicated Logstores have the following limits:
        -   To prevent data tampering, you can write only the logs of the specified Alibaba Cloud service to the specified Logstore. In addition, you cannot modify or delete the indexes of the specified Logstore.
        -   You can modify the retention period of logs and delete the dedicated Logstores only on the Global Configuration page of the Log Audit Service application or by calling the related API operation.
        -   If you turn on the **Synchronization to Central Project** switch for SLB, OSS, or PolarDB-X on the Global Configuration page, data transformation tasks are generated in the regional projects. The tasks synchronize the log data stored in the Logstores of the regional projects for SLB, OSS, or PolarDB-X to the central project that resides in the specified region.
            -   The data transformation task that is generated for SLB is named Internal Job: SLS Audit Service Data Sync for OSS Access. The data transformation task that is generated for OSS is named Internal Job: SLS Audit Service Data Sync for SLB. The data transformation task that is generated for PolarDB-X is named Internal Job: SLS Audit Service Data Sync for DRDS.
            -   You can stop the task only on the Global Configuration page of the Log Audit Service application or by calling the related API operation.
            -   For example, assume that you turn on the **Synchronization to Central Project** switch for SLB, OSS, or PolarDB-X. The log data in the Logstores of the regional projects is synchronized to the dedicated Logstore of the central project that corresponds to SLB, OSS, or PolarDB-X. You cannot manage the regional Logstores. However, you can perform operations such as log query on the central Logstore.

## Billing

-   Log Service

    You must activate Log Service and enable the Log Audit Service application for the central Alibaba Cloud account that is used to collect logs from other Alibaba Cloud accounts. You do not need to activate Log Service for other Alibaba Cloud accounts. If the Log Audit Service application is based on specific modules of the cloud services for these accounts, you need to activate Log Service. No fees are incurred in these Alibaba Cloud accounts. When you use the Log Audit Service application, you are charged for the data storage, the read and write traffic, and the data transformation feature based on the pay-as-you-go method. For more information, see [Billing overview](/intl.en-US/Pricing/Billing overview.md).

    **Note:** If you turn on the Synchronization to Central Project switch for SLB, OSS, or PolarDB-X, log data is synchronized by using the data transformation feature. If you turn on the switch on the Audit-Related Logs tab for Container Service for Kubernetes \(ACK\), log data of ACK is also synchronized by using the data transformation feature. You are charged for the data transformation feature and the cross-network traffic based on the pay-as-you-go method. For more information, see [Billing overview](/intl.en-US/Pricing/Billing overview.md).

    You can use resource plans and free resource quotas to offset the incurred fees.

-   Alibaba Cloud services

    After you enable the Log Audit Service application in the Log Service console and turn on the related switches on the Audit-Related Logs tab for Alibaba Cloud services, additional fees may be incurred in the console of the Alibaba Cloud services. The following table describes the Alibaba Cloud services that may incur additional fees.

    |Alibaba Cloud service|Additional fee|
    |---------------------|--------------|
    |Web Application Firewall \(WAF\)|You can log on to the WAF console and click **Log Service** to enable the log analysis feature. For information about additional fees, see [Billing methods](/intl.en-US/Log Management/Log service/Billing methods.md).|
    |Security Center|You can log on to the Security Center console and click **Log Analysis** to enable the log analysis feature. For information about additional fees, see [Billing](/intl.en-US/Pricing/Billing.md).|
    |Cloud Firewall|You can log on to the Cloud Firewall console and click **Log Analysis** to enable the log analysis feature. For information about additional fees, see [Billing](/intl.en-US/Logs/Log analysis/Billing.md).|
    |ApsaraDB RDS|After you enable the log collection feature for ApsaraDB RDS, SQL Explorer is automatically enabled on the RDS instances that meet the specific requirements. For information about additional fees, see [Pricing, billable items, and billing methods](/intl.en-US/Purchase Guide/Pricing, billable items, and billing methods.md).|
    |PolarDB for MySQL|After you enable the log collection feature for PolarDB for MySQL, SQL Explorer is automatically enabled on the PolarDB for MySQL clusters that meet the specific requirements. For information about additional fees, see [t3012.md\#section\_7n5\_1om\_2a6](/intl.en-US/Product Billing/Billable items.md).|


