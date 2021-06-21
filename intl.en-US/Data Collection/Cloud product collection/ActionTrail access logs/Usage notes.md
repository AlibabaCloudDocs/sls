# Usage notes

ActionTrail provides the operation log shipment feature. You can use this feature to collect operation logs from ActionTrail to Log Service in real time. Log Service allows you to query, ship, transform, and visualize data in real time. You can also create alerts for the data. This way, you can monitor the operations on your cloud resources. This topic describes the resources and billing methods that are related to ActionTrail log shipment.

## Resources

-   Project

    When you enable the logging feature for ActionTrail, you must specify a destination project.

-   Dedicated Logstore

    After you specify a destination project for operations logs, a dedicated Logstore named in the actiontrail\_Trail name format is automatically created in the project.

    -   The indexing feature is automatically enabled for the Logstore. Indexes are configured for some fields.
    -   By default, log data is permanently retained in the Logstore. You can modify the retention period of log data based on your business requirements. For more information, see [Manage a Logstore](/intl.en-US/Preparation/Manage a Logstore.md).
-   Dedicated dashboard

    By default, only one dedicated dashboard is created.

    **Note:** We recommend that you do not make changes to the dedicated dashboard because this may affect the availability of the dashboard. You can create a custom dashboard to visualize log analysis results. For more information, see [Create a dashboard](/intl.en-US/Visualization/Create a dashboard.md).

    |Dashboard|Description|
    |---------|-----------|
    |actiontrail\_Trail name\_audit\_center\_cn|Displays the visualized log data of operations on cloud resources in real time. The log data includes page views \(PVs\), unique views \(UVs\), the number of source servers, the distribution of event sources, and the trend of the PV/UV ratio.|


## Billing

-   The cost of ActionTrail log management is included in the billing for ActionTrail. For more information, see [Billing](/intl.en-US/Pricing/Billing.md).
-   After SQL audit logs are sent to Log Service, you are charged based on the storage space, read traffic, the number of requests, data transformation, and data shipping. For more information, see [Log Service pricing](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).

## Limits

-   You can create a trail in the ActionTrail console to ship operation logs to an OSS bucket or a Log Service Logstore. You can create a maximum of five trails in a region.
-   You can write only operation logs of cloud services to dedicated Logstores.

## Scenarios

-   You can use the feature to monitor the operations on the cloud resources of your Alibaba Cloud account and troubleshoot exceptions. You can also use operations logs to track accidental deletions and risky operations.
-   You can use log analysis results to track the distribution and sources of operations on essential cloud resources and optimize strategies on the cloud resources.
-   You can query operations logs of cloud services such as ActionTrail and view the distribution and time of operations. This way, you can monitor the status of cloud resources in real time.
-   You can customize query statements based on your operational and data requirements. You can also customize dashboards to analyze data in real time based on your resource usage and user logons.

