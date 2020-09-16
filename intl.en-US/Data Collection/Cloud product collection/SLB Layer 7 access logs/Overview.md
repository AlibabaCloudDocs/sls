# Overview

Alibaba Cloud Server Load Balancer \(SLB\) provides the access log management feature that is supported by Log Service. You can use SLB access logs to analyze user behavior and the distribution of users and troubleshoot issues. This topic describes the resources, billings, and limits that are related to the access log management feature.

## Resources

-   Project and Logstore

    -   The indexing feature is automatically enabled for the Logstore. Indexes are configured for some fields.
    -   By default, log data is permanently retained in the Logstore. You can modify the retention period of log data based on your business requirements. For more information, see [Manage a Logstore](/intl.en-US/Data Collection/Preparation/Manage a Logstore.md).
    **Note:** You must not delete the project or Logstore that is related to the log management feature. Otherwise, the access logs cannot be sent to Log Service.

-   Dedicated dashboards

    After you enable the feature, two dedicated dashboards are automatically created for SLB access logs.

    **Note:** We recommend that you do not make changes to the dedicated dashboards because this may affect the usability of the dashboards. You can create a custom dashboard to visualize the results of log queries. For more information, see [Create a dashboard](/intl.en-US/Query and visualization/Dashboard/Create a dashboard.md).

    |Dashboard|Description|
    |---------|-----------|
    |slb-user-log-slb\_layer7\_operation\_center\_en|Displays the overall operational statistics of SLB, including page views \(PVs\), unique views \(UVs\), the request success rate, size of request messages, and size of response messages.|
    |slb-user-log-slb\_layer7\_access\_center\_en|Displays the details of requests sent to SLB, including the distribution of clients, trend of request methods, trend of status codes, top clients, and topology of request messages.|


## Limits

-   The access log management feature is available only in SLB instances for which a layer-7 listener is configured.
-   The project that stores SLB access logs must reside in the same region as the SLB instance.

## Billing

-   You are not charged for using the access log management feature of SLB.
-   After SLB access logs are shipped to Log Service, you are charged for the storage space that the logs occupy and the number of read/write operations on the logs. You are also charged for reading, transforming, and shipping the logs. For more information, see [Billing methods](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).

## Benefits

-   Easy to use: The access log management feature allows developers and operations and maintenance \(O&M\) personnel to unburden from tedious and time-consuming log processing. They can focus on business development and technical research.
-   Capable of processing large amounts of data: The size of access logs is proportional to the request PVs of SLB instances. Processing large amounts of data requires high performance and may introduce high costs. Log Service allows you to analyze 100 million log entries in 1 second and is cost-competitive compared with open source solutions.
-   Capable of processing data in real time: Timeliness is essential in multiple scenarios such as DevOps, data monitoring, and alerting. The access log management feature of SLB integrates the data processing capabilities of Log Service. Large amounts of log data can be processed within seconds.
-   Flexible: You can enable or disable the access log management feature based on the specification of your SLB instances. You can also set a custom retention period for log data. In addition, the storage capacity of a Logstore automatically scales based on your the data volume.

