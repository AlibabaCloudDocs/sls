# Usage notes

The full log feature of Alibaba Cloud Anti-DDoS Pro \(old\) allows you to collect, query, analyze, transform, and consume website access logs and HTTP flood attack logs in real time. You can use the full log feature to resolve website access errors, trace HTTP flood attacks, and analyze website operations. This topic describes the assets, billing, and limits of the full log feature.

**Note:** Anti-DDoS Pro \(old\) has been upgraded to Anti-DDoS Pro \(BGP\). The Anti-DDoS Pro \(old\) console is available only for users who have purchased Anti-DDoS Pro instances. You can no longer create Anti-DDoS Pro instances in the Anti-DDoS Pro \(old\) console.

## Assets

-   A dedicated project and dedicated Logstore
    -   After you enable the full log feature for Anti-DDoS Pro instances that are deployed in mainland China, Log Service creates a project named ddos-pro-project-Alibaba Cloud account ID-cn-hangzhou and a Logstore named ddos-pro-logstore.
    -   After you enable the full log feature for Anti-DDoS Pro instances that are deployed outside mainland China, Log Service creates a project named ddos-pro-Alibaba Cloud account ID-ap-southeast-1 and a Logstore named ddos-pro-logstore.
-   Dedicated dashboards

    After you enable the full log feature, Log Service generates two dedicated dashboards by default.

    **Note:** We recommend that you do not make changes to the dedicated dashboards because this may affect the usability of the dashboards. You can create a dashboard to visualize log analysis results. For more information, see [Create a dashboard](/intl.en-US/Query and visualization/Dashboard/Create a dashboard.md).

    |Dashboard|Description|
    |---------|-----------|
    |DDoS Operation Center|Displays the operational statistics of protected websites. The operational statistics include the rate of valid requests, amount of valid traffic, number of requests and interceptions, and overview of attacks.|
    |DDoS Access Center|Displays the access statistics of the protected websites. The access statistics include page views \(PVs\), unique visitors \(UVs\), inbound traffic, peak inbound bandwidth, peak outbound bandwidth, PV/UV trends, and source distribution.|


## Billing

-   You are not charged for enabling the full log feature in the Anti-DDoS \(old\) console.
-   After Anti-DDoS Pro logs are shipped to Log Service, you are charged for the storage space that the log data occupies and the number of read/write operations. You are also charged for reading, transforming, and shipping the data. For more information, see [Billing methods](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).

## Limits

-   You can write only Anti-DDos Pro log data to the dedicated Logstore.
-   If you have overdue payments for your Log Service resources, the full log feature is automatically disabled. To ensure service continuity, you must clear your overdue payment within the prescribed time limit.

## Benefits

-   Ease of use: To enable the full log feature, you only need to perform a few simple operations. The feature ensures that Anti-DDoS Pro logs are collected in real time. After you add a website to an Anti-DDoS Pro instance, logs of the website are automatically collected.
-   Real-time analysis: Based on Log Service, the full log feature provides real-time analysis and out-of-the-box dashboards. The dashboards provide insights into the HTTP flood attacks and access details.
-   Real-time monitoring: You can use Log Service to monitor specified metrics and send alerts in real time. This allows you to resolve business exceptions at the earliest opportunity.
-   High compatibility: Log Service is compatible with other big data solutions, such as stream processing, cloud storage, and visualization. This allows you to extract more value from your business data.

