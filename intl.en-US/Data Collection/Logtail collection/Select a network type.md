# Select a network type

This topic describes how to select a network type when you use Logtail to collect logs.

## Network Types

-   Alibaba Cloud internal network: The Alibaba Cloud internal network provides gigabits of shared bandwidth. Transmission of log data over the internal network is stable and fast. The Alibaba Cloud internal network includes virtual private clouds \(VPCs\) and the classic network.
-   Internet: Transmission of log data over the Internet is limited by the network bandwidth. In addition, network issues such as jitters, latency, and packet loss may affect the speed and stability of data transmission.
-   Global acceleration: This network type can be used to accelerate log collection by using the nodes of Alibaba Cloud Content Delivery Network \(CDN\). Compared with the Internet, global acceleration provides a more stable network with low transmission latency.

## Select a network type

-   Alibaba Cloud internal network

    You can use the Alibaba Cloud internal network to transmit log data from an Elastic Compute Service \(ECS\) instance to a Log Service project. Examples are provided in the following two scenarios:

    -   The ECS instance and the Log Service project belong to the same Alibaba Cloud account and reside in the same region.
    -   The ECS instance and the Log Service project belong to different Alibaba Cloud accounts but reside in the same region.
    We recommend that you create a Log Service project in the region where the ECS instance resides. Then, you can collect logs of the ECS instance and send the logs to Log Service over the Alibaba Cloud internal network.

-   Internet

    You can use the Internet to transmit log data from an ECS instance or server to a Log Service project. Examples are provided in the following two scenarios:

    -   The ECS instance and the Log Service project reside in different regions.
    -   The server is provided by a third-party cloud service provider or deployed in your self-managed data center.
-   Global acceleration

    If your servers are deployed in your self-managed data center outside China or provided by a third-party cloud service provider outside China, you can enable global acceleration. Global acceleration can resolve stability and latency issues during data transmission over the Internet. For more information, see [Global acceleration](/intl.en-US/Data Collection/Collection acceleration/Overview.md).


|Server type|Reside in the same region as the project|Require an Alibaba Cloud account ID|Network Type|
|:----------|:---------------------------------------|:----------------------------------|:-----------|
|ECS instance of the current Alibaba Cloud account|Yes|No|Alibaba Cloud internal network|
|No|No|Internet or global acceleration|
|ECS instance of another Alibaba Cloud account|Yes|Yes|Alibaba Cloud internal network|
|No|Yes|Internet or global acceleration|
|A server that is provided by a third-party cloud service provider or deployed in your self-managed data center|N/A|Yes|Internet or global acceleration|

**Note:** Log Service cannot obtain the owner information of the ECS instances within other Alibaba Cloud accounts, servers that are provided by third-party cloud service providers, or self-managed servers. If you install Logtail on such ECS instances or servers, you must configure an Alibaba Cloud account ID for them. Otherwise, Log Service cannot receive their heartbeats or collect their logs. For more information, see [Configure an account ID for a server](/intl.en-US/Data Collection/Logtail collection/Machine Group/Configure an account ID for a server.md).

## Example

The following table describes how to select a network type based on your business requirements.

**Note:** For example, assume that your Log Service project resides in the China \(Hong Kong\) region and your server resides in a global self-managed data center inside or outside China. We recommend that you select global acceleration in the China \(Hong Kong\) region to collect logs when you install Logtail on the server. Compared with the Internet, global acceleration provides a more stable network with low transmission latency.

|Scenario|Region of the Log Service project|Server type|Region of the ECS instance|Region selected when Logtail is installed|Network Type|Require an Alibaba Cloud account ID|
|:-------|:--------------------------------|:----------|:-------------------------|:----------------------------------------|:-----------|:----------------------------------|
|The ECS instance and the Log Service project are in the same region.|China \(Hangzhou\)|ECS instance of the current Alibaba Cloud account|China \(Hangzhou\)|China \(Hangzhou\)|Alibaba Cloud internal network|No|
|The ECS instance and the Log Service project are in different regions.|China \(Shanghai\)|ECS instance of the current Alibaba Cloud account|China \(Beijing\)|China \(Shanghai\)|Internet|No|
|The ECS instance and the Log Service project belong to different Alibaba Cloud accounts|China \(Shanghai\)|ECS instance of other Alibaba Cloud accounts|China \(Beijing\)|China \(Shanghai\)|Internet|Yes|
|The server is deployed in your self-managed data center.|China \(Shenzhen\)|Self-managed server|N/A|China \(Shenzhen\)|Internet|Yes|
|Global acceleration is used.|China \(Hong Kong\)|Self-managed server|N/A|China \(Hong Kong\)|Global acceleration|Yes|

![Select a network type](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2696549951/p12057.png)

