# Usage notes

ActionTrail provides the inner-ActionTrail feature. You can use this feature to ship platform operations logs from ActionTrail to Log Service in real time. Log Service allows you to query, transform, and consume the logs in real time. This topic describes the resources, billings, and limits that are related to the inner-ActionTrail feature.

**Note:** You can use this feature to collect platform operations logs of OSS buckets, ECS instances, and RDS instances.

## Resources

-   Projects and Logstores

    -   By default, log data is permanently retained in the Logstore. You can modify the retention period of log data based on your business requirements. For more information, see [Manage a Logstore](/intl.en-US/Data Collection/Preparation/Manage a Logstore.md).
    **Note:** You must not delete the projects or Logstores that are related to platform operations logs. Otherwise, the platform operations logs cannot be sent to Log Service.

-   Dedicated dashboard

    After you enable the feature, a dedicated dashboard is automatically created for the platform operations logs.

    **Note:** We recommend that you do not make changes to the dedicated dashboard because this may affect the usability of the dashboard. You can customize a dashboard to display query results. For more information, see [Create a dashboard](/intl.en-US/Query and visualization/Dashboard/Create a dashboard.md).

    |Dashboard|Description|
    |---------|-----------|
    |actiontrail\_ Trail Name\_audit\_center\_en|Displays the visualized log data of operations on cloud resources in real time. The log data includes page views \(PVs\), unique views \(UVs\), the number of source servers, distribution of event sources, and trend of the PV/UV ratio.|


## Billing

-   You are not charged for using the inner-AcionTrail feature of ActionTrail.
-   After platform operations logs are shipped to Log Service, you are charged for the storage space that the log data occupies and the number of read/write operations. You are also charged for reading, transforming, and shipping the data. For more information, see [Billing methods](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).

## Limits

-   To use the inner-ActionTrail feature, you must be granted relevant permissions. To do so, submit a ticket or contact technique support.
-   You must clear overdue payments to make sure that Log Service is available.
-   All platform operations logs are shipped to the same Logstore.
-   You can write only platform operations logs to a dedicated Logstore.
-   You cannot modify the retention period for the platform operations logs stored in a dedicated Logstore.

## Benefits

-   Classified protection compliance: stores the platform operations logs of Alibaba Cloud services for six months or more to meet the classified protection compliance requirements.
-   Ease of use: allows you to collect platform operations logs in real time with simple configurations.
-   Real-time analysis: provides real-time analysis capabilities and out-of-the-box dashboards. This allows you to monitor the distribution and other details of platform operations logs.
-   Real-time alerts: supports real-time monitoring and alerting based on customized metrics. This ensures that you can respond to critical business exceptions at the earliest opportunity.
-   Collaboration: allows you to integrate the feature with stream computing, cloud storage, visualization, and other data ecosystems to perform finer-grained analysis.

## Scenarios

-   Track platform operations logs of Alibaba Cloud services and locate the causes of resource changes.
-   View, audit, and evaluate platform operations logs in near real time.
-   Export platform operations logs to data centers.

