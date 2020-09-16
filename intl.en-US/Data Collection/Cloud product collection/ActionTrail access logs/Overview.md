# Overview

ActionTrail provides the operations log shipping feature. You can use this feature to ship operations logs from ActionTrail to Log Service in real time. Log Service allows you to query, ship, transform, and visualize the data in real time. You can also create alerts for the data. This way, you can monitor the operations on your cloud resources. This topic describes the resources and billings that are related to the operations log shipping feature of ActionTrail.

## Resources

-   Project

    If you enable the log shipping feature in the ActionTrail console and ship operations logs to Log Service, you must specify a destination project.

-   Dedicated Logstore

    After you specify a destination project for operations logs, a dedicated Logstore named in the actiontrail\_Trail Name format is automatically created in the project.

    -   The indexing feature is automatically enabled for the Logstore. Indexes are configured for some fields.
    -   By default, log data is permanently retained in the Logstore. You can modify the retention period of log data based on your business requirements. For more information, see [Manage a Logstore](/intl.en-US/Data Collection/Preparation/Manage a Logstore.md).
-   Dedicated dashboard

    After you enable the feature, a dedicated dashboard is automatically created for the operations logs.

    **Note:** We recommend that you do not make changes to the dedicated dashboard because this may affect the usability of the dashboard. You can customize a dashboard to display query results. For more information, see [Create a dashboard](/intl.en-US/Query and visualization/Dashboard/Create a dashboard.md).

    |Dashboard|Description|
    |---------|-----------|
    |actiontrail\_ Trail Name\_audit\_center\_en|Displays the visualized log data of operations on cloud resources in real time. The log data includes page views \(PVs\), unique views \(UVs\), the number of source servers, the distribution of event sources, and the trend of the PV/UV ratio.|


## Billing

-   You are not charged for using the operations log shipping feature of ActionTrail.
-   After operations logs are shipped to Log Service, you are charged for the storage space that the log data occupies and the number of read/write operations on the data. You are also charged for reading, transforming, and shipping the data. For more information, see [Billing methods](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).

## Limits

-   You can create a trail in the ActionTrail console to ship operations logs to an OSS bucket or a Log Service Logstore. You can create a maximum of five trails in a region.
-   You can write only operations logs of cloud services to dedicated Logstores.

## Scenarios

-   You can use the feature to monitor the operations on the cloud resources of your Alibaba Cloud account and troubleshoot exceptions. You can also use operations logs to track accidental deletions and risky operations.
-   You can use log analysis results to track the distribution and sources of operations on essential cloud resources and optimize strategies on the cloud resources.
-   You can query operations logs of cloud services such as ActionTrail and view the distribution and time of operations. This way, you can monitor the operational status of cloud resources in real time.
-   You can customize query statements and saved searches based on your operational and data requirements. You can also customize dashboards to analyze data in real time based on your resource usage and user logons.

