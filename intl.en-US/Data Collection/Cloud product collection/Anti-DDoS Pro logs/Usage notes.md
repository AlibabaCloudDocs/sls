# Usage notes

The full log feature of Anti-DDoS Pro and Anti-DDoS Premium of the previous version can be used to collect, query, analyze, transform, and consume website access logs and HTTP flood attack logs in real time. You can use this feature to fix website access errors, trace HTTP flood attacks, and analyze website operations. This topic describes the assets, billing, and limits of the full log feature.

**Note:** Anti-DDoS Pro and Anti-DDoS Premium of the previous version has been upgraded to Anti-DDoS Pro. The console of Anti-DDoS Pro and Anti-DDoS Premium of the previous version is available only if you have purchased Anti-DDoS Pro and Anti-DDoS Premium instances. You can no longer create Anti-DDoS Pro and Anti-DDoS Premium instances in the console of Anti-DDoS Pro and Anti-DDoS Premium of the previous version.

## Benefits

-   Ease of use: To enable the full log feature, you only need to perform a few simple operations. The feature ensures that Anti-DDoS Pro and Anti-DDoS Premium logs are collected in real time. After you add a website to an Anti-DDoS Pro and Anti-DDoS Premium instance, the logs of the website are automatically collected.
-   Real-time analysis: The full log feature provides real-time log analysis and out-of-the-box dashboards by using Log Service. The dashboards provide insights into the HTTP flood attacks and access details.
-   Real-time monitoring: You can use Log Service to monitor specified metrics and send alerts in real time. This allows you to handle business exceptions at the earliest opportunity.
-   High compatibility: Log Service is compatible with other big data solutions, such as stream processing, cloud storage, and visualization. This allows you to extract more value from your business data.

## Assets

-   A dedicated project and dedicated Logstore
    -   After you enable the full log feature for an Anti-DDoS Pro and Anti-DDoS Premium of the previous version instance deployed in mainland China, Log Service creates a project named DDoS-pro-project-Alibaba Cloud account ID-cn-hangzhou. In addition. Log Service creates a Logstore named DDoS-pro-logstore.
    -   After you enable the full log feature for an Anti-DDoS Pro and Anti-DDoS Premium of the previous version instance deployed outside mainland China, Log Service creates a project named DDoS-pro-Alibaba Cloud account ID-ap-southeast-1. In addition, Log Service creates a Logstore named DDoS-pro-logstore.

        **Note:** You can write only the logs of the Anti-DDoS Pro and Anti-DDoS Premium of the previous version instance to the dedicated Logstore. However, this limit does not apply to the logs that are related to statistics, alerts, and consumption.

-   Dedicated dashboards

    By default, Log Service generates two dedicated dashboards for the logs of the Anti-DDoS Pro or Premium of the previous version.

    **Note:** We recommend that you do not make changes to the dedicated dashboard. This may affect the usability of the dashboards. You can create a custom dashboard to view log analysis results. For more information, see [Create a dashboard](/intl.en-US/Visualization/Create a dashboard.md).

    |Dashboard|Description|
    |---------|-----------|
    |DDoS Operation Center|Displays the operational statistics of protected websites. The statistics include the rate of valid requests, amount of valid traffic, number of requests and interceptions, and overview of attacks.|
    |DDoS Access Center|Displays the access statistics of the protected websites. The statistics include page views \(PVs\), unique visitors \(UVs\), inbound traffic, peak inbound bandwidth, peak outbound bandwidth, PV/UV trends, and source distribution.|


## Billing

-   Anti-DDoS Pro and Anti-DDoS Premium of the previous version provides the following free quotas. If the number of resources that you have used exceeds the quota, you are charged for the excess based on the standard pricing. For more information, see [Anti-DDoS Pro or Premium Pricing](https://help.aliyun.com/document_detail/40498.html?spm=a2c4g.11186623.6.985.4a4a612dALoDEq).
    -   100 GB of write traffic per day
    -   Free rental of four Shard per day
    -   Three days of free log retention
-   AfterAnti-DDoS Pro and Anti-DDoS Premium of the previous version pushes logs to Log Service, you can query, analyze, monitor, and view log analysis results free of charge. However, you are charged based on the standard pricing of Log Service when you read, transform and ship data, or send alerts by using SMS or voice messages. For more information, see [Log Service pricing](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).

