# Select a network type

This topic describes how to select a network type when you use Logtail to collect logs.

## Network type

-   Alibaba Cloud internal network: The Alibaba Cloud internal network provides gigabits of shared bandwidth. Transmission of log data over the internal network is stable and fast. The Alibaba Cloud internal network includes virtual private clouds \(VPCs\) and the classic network.
-   Internet: Transmission of log data over the Internet is limited by the network bandwidth. In addition, network issues such as jitters, latency, and packet loss may affect the speed and stability of data transmission.
-   Global acceleration: This network type accelerates log collection by using the nodes of Alibaba Cloud Content Delivery Network \(CDN\). Compared with the Internet, global acceleration offers a more stable network with minimal transmission latency.

## Select a network type

-   Alibaba Cloud internal network

    You can use the Alibaba Cloud internal network to transmit log data in the following two scenarios:

    -   The ECS instances and the Log Service project belong to the same Alibaba Cloud account and reside in the same region.
    -   The ECS instances and the Log Service project belong to different Alibaba Cloud accounts but reside in the same region.
    We recommend that you create a Log Service project in the region where the ECS instances reside. Then you can collect logs of the ECS instances to Log Service over the Alibaba Cloud internal network.

-   Internet

    You can use the Internet to transmit log data in the following two scenarios:

    -   The ECS instances and the Log Service project reside in different regions.
    -   The servers are provided by a third-party cloud service provider or deployed in your on-premises data center.
-   Global acceleration

    If your servers are deployed in your on-premises data center outside China or provided by a third-party cloud service provider outside China, data transmission over the Internet may be unstable or suffer high latency. In this case, you can enable [Global acceleration](/intl.en-US/Data Collection/Collection acceleration/Overview.md).


|Server type|Reside in the same region as the project|Required to configure an Alibaba Cloud account ID|Network type|
|:----------|:---------------------------------------|:------------------------------------------------|:-----------|
|ECS instance of the current Alibaba Cloud account|Yes|No|Alibaba Cloud internal network|
|No|No|Internet or global acceleration|
|ECS instance of another Alibaba Cloud account|Yes|Yes|Alibaba Cloud internal network|
|No|Yes|Internet or global acceleration|
|Server that is provided by a third-party cloud service provider or located in your on-premises data center|N/A|Yes|Internet or global acceleration|

**Note:** Log Service cannot obtain the owner information about the ECS instances of other Alibaba Cloud accounts, servers that are provided by third-party cloud service providers, or on-premises servers. If you install Logtail on such ECS instances or servers, you must configure an Alibaba Cloud account ID for them. Otherwise, Log Service cannot receive their heartbeats or collect their logs. For more information, see [Configure an account ID for a server](/intl.en-US/Data Collection/Logtail collection/Machine Group/Configure an account ID for a server.md).

## Examples

The following table shows how to select a network type based on your business requirements.

**Note:** If your Log Service project resides in the China \(Hong Kong\) region and your servers reside in a global on-premises data center, we recommend that you select global acceleration in the China \(Hong Kong\) region to collect logs. Compared with the Internet, global acceleration offers a more stable network with minimal transmission latency.

|Scenario|Region of the Log Service project|Server type|Region of ECS instances|Region selected when Logtail is installed|Network type|Required to configure an Alibaba Cloud account ID|
|:-------|:--------------------------------|:----------|:----------------------|:----------------------------------------|:-----------|:------------------------------------------------|
|ECS instances and the Log Service project in the same region|China \(Hangzhou\)|ECS instances of the current Alibaba Cloud account|China \(Hangzhou\)|China \(Hangzhou\)|Alibaba Cloud internal network|No|
|ECS instances and the Log Service project in different regions|China \(Shanghai\)|ECS instances of the current Alibaba Cloud account|China \(Beijing\)|China \(Shanghai\)|Internet|No|
|ECS instances and the Log Service project managed by different Alibaba Cloud accounts|China \(Shanghai\)|ECS instances of other Alibaba Cloud accounts|China \(Beijing\)|China \(Shanghai\)|Internet|Yes|
|Servers located in your on-premises data center|China \(Shenzhen\)|On-premises servers|N/A|China \(Shenzhen\)|Internet|Yes|
|Global acceleration|China \(Hong Kong\)|On-premises servers|N/A|China \(Hong Kong\)|Global acceleration|Yes|

![Network selection example](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2696549951/p12057.png)

