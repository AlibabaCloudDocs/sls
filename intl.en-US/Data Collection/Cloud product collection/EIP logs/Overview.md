# Overview

Alibaba Cloud allows you to integrate Elastic IP Address \(EIP\) with Log Service to implement fine-grained per-second monitoring. Logs that contain fine-grained monitoring data of network bandwidth are delivered to Log Service. This feature allows you to monitor the fluctuations of Internet data transfer in real time. You can adjust the peak bandwidth of an EIP in a timely manner. This topic describes EIP log-related resources, billing, and limits.

## Resources

-   Projects and Logstores

    **Note:**

    -   You must not delete the projects or Logstores that are related to EIP logs. Otherwise, EIP logs cannot be sent to Log Service.
    -   After the EIP per-second monitoring feature is enabled, the data retention period of Logstores that store EIP logs is forcibly changed to 7 days.
-   Dedicated dashboard

    By default, only one dedicated dashboard is created.

    **Note:** We recommend that you do not make changes to the dedicated dashboard because this may affect the availability of the dashboard. You can create a custom dashboard to visualize log analysis results. For more information, see [Create a dashboard](/intl.en-US/Query and visualization/Dashboard/Create a dashboard.md).

    |Dashboard|Description|
    |---------|-----------|
    |eip\_monitoring|Allows you to monitor the fluctuations of Internet data transfer in real time. These fluctuations include the peak inbound and outbound bandwidth per second, inbound and outbound packet rates per second, inbound and outbound packet loss rates per seconds, and inbound and outbound TCP session establishment rates.|


## Billing

-   You are not charged when you use the log feature of EIP.
-   After EIP logs are shipped to Log Service, you are charged for the storage space that the log data occupies and the number of read/write operations. You are also charged when Log Service reads, transforms, and ships the data. For more information, see [Billing methods](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).

## Limits

-   The Log Service project that stores EIP logs must reside in the same region as the elastic IP address \(EIP\).
-   You can use an Alibaba Cloud account to enable the fine-grained monitoring feature for a maximum of 10 EIPs.

    If you need to enable the fine-grained monitoring feature for more EIPs, you must submit a [ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).


