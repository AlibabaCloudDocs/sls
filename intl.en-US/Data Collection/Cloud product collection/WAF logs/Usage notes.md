# Usage notes

Web Application Firewall \(WAF\) allows you to query, analyze, transform, and consume logs by using Log Service. After you enable the log analysis feature, the access logs and anti-attack logs of your website domain are collected in real time. This feature helps you better protect and manage your website. This topic describes the assets, billing, and limits of using the log analysis feature.

## Assets

-   Dedicated projects and Logstores
    -   If you set Region to Mainland China when you purchased the WAF instance, after you enable the log analysis feature, Log Service creates a project named waf-project-Alibaba Cloud account ID-cn-hangzhou and a Logstore named waf-logstore.
    -   If you set Region to International when you purchased the WAF instance, after you enable the log analysis feature, Log Service creates a project named waf-project-Alibaba Cloud account ID-ap-southeast-1 and a Logstore named waf-logstore.
-   Dedicated dashboards

    After you enable the log analysis feature, Log Service creates three dashboards by default.

    **Note:** To ensure that the dashboards are up to date, we recommend that you do not modify the dashboards. You can create a custom dashboard to visualize the results of log analysis. For more information, see [Create a dashboard](/intl.en-US/Query and visualization/Dashboard/Create a dashboard.md).

    |Dashboard|Description|
    |:--------|:----------|
    |Operation Center|The Operation Center dashboard shows the details of website operation, traffic, and attacks. The metrics of website operation include Valid Request Ratio, Valid Request Traffic Ratio, Peak Attack Size, Attack Traffic, and Attack Count. The traffic metrics include Peak Network In, Peak Network Out, Received Requests, Traffic Received, and Traffic Out.|
    |Access Center|The Access Center dashboard shows the basic access details, the access trend, the distribution of visitors, and other information. The metrics of basic access details include the number of page views \(PVs\) and the number of unique visitors \(UVs\).|
    |Security Center|The Security Center dashboard shows the attack metrics, attack types, attack trend, attacker distribution, and other information.|


## Billing

You are charged based on the log storage capacity and duration that you select on the buy page of WAF. You are charged by Log Service when you transform logs, ship logs, and read data on the Internet. For more information, see [Log Service Pricing](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).

## Limits

-   To use the log analysis feature, you must ensure that the billing method is subscription and the edition of WAF is Pro, Business, or Enterprise.
-   If you have overdue payments in Log Service, you can no longer use the log analysis feature.
-   Only the data that is generated in WAF can be written to the dedicated Logstore. This limit does not apply to other log operations such as query, statistics, alerts, and consumption.
-   The dedicated Logstore cannot be deleted. The data retention period of the dedicated Logstore cannot be changed.
-   You must ensure that the available storage space of WAF logs is sufficient. After the log storage capacity is exhausted, logs can no longer be stored.

    **Note:** You can view the usage of log storage space in the WAF console. However, the usage is not updated in real time. The displayed usage does not include the usage in the last two hours.


## Benefits

-   Classified protection compliance: The website access logs are stored for more than six months. Requirements of classified protection compliance are met.
-   Ease of use: To enable the log analysis feature, you only need to perform a few simple operations. The feature ensures that the access logs and anti-attack logs of your website domain are collected in real time. You can customize the log storage capacity and duration. You can select a website for log collection.
-   Real-time analysis: The WAF console provides the real-time log analysis service and out-of-the-box dashboards. The dashboards provide insights into the visits to and attacks on your website.
-   Real-time monitoring: You can monitor your website by using specific metrics. You can customize alert settings to receive alerts almost in real time. This allows you to handle the exceptions of critical business in a timely manner.
-   Integration: You can use Log Service together with other data solutions such as real-time compute, cloud storage, and visualization to maximize the value of your business data.

## Scenarios

-   Track anti-attack logs and trace the source of security threats.
-   Monitor web requests in real time and view traffic trends.
-   Obtain information about the efficiency of security operations and respond to issues in a timely manner.
-   Export security logs to data centers.

