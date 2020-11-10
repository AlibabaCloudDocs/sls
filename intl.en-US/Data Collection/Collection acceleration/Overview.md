# Overview

In addition to the Alibaba Cloud internal network and the Internet, you can also use the global acceleration feature to collect data to Log Service. The global acceleration feature is based on the Internet. Compared with the Internet, the global acceleration feature provides lower latency and higher stability during data transmission. The feature is suitable for data collection and consumption scenarios that require reliable and fast transmission. The global acceleration feature of Log Service depends on the acceleration environment that is provided by [Dynamic Route for CDN](https://www.alibabacloud.com/product/dcdn?spm=a2796.7990255.5942891490.4.36aa2351Z2GWEb). This feature solves issues such as slow response, packet loss, and unstable service performance that are caused by cross-ISP services, network instability, traffic bursts, and network congestion. It improves overall data transmission performance and user experience.

The global acceleration feature of Log Service is based on Alibaba Cloud Content Delivery Network \(CDN\) hardware resources. This feature optimizes the stability of data transmission from various data sources, such as mobile phones, Internet of Things \(IoT\) devices, smart devices, on-premises servers, and other cloud servers.

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5596549951/p8060.png)

## Implementation basics

The global acceleration feature of Log Service is based on Alibaba Cloud CDN hardware resources. Your terminals such as mobile phones, IoT devices, smart devices, on-premises servers, and other cloud servers can access the nearest node of Alibaba Cloud CDN. Then data is routed to Log Service over the inner high-speed channels of CDN. Compared with data transmission over the Internet, data transmission over the global acceleration feature reduces network latency and jitters.

![Collection acceleration basics - 001](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5596549951/p94571.png)

The following figure shows how the global acceleration feature interacts with the client and Log Service. The overall process is described as follows:

1.  The client sends a request to a public DNS server for the resolution of the domain name for CDN `your-project.log-global.aliyuncs.com`.
2.  The public DNS server maps the domain name `your-project.log-global.aliyuncs.com` to the CNAME record `your-project.log-global.aliyuncs.com.w.kunlungr.com`. The domain name resolution request is then forwarded to Alibaba Cloud CDN.
3.  Alibaba Cloud CDN returns the IP address of the optimal node to the public DNS server based on the intelligent scheduling system.
4.  The public DNS server returns the resolved IP address to the client.
5.  The client sends a request to Log Service based on the obtained IP address.
6.  After the CDN node receives the request, it uses dynamic routing and a private transport protocol to route the request to the node closest to the Log Service server. The node then forwards the request to Log Service.
7.  After the Log Service server receives the request from the CDN node, the Log Service server returns the result of the request to the node.
8.  CDN transmits the request result to the client in the pass-through mode.

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5596549951/p8062.png)

## Billing

When you use the global acceleration feature, you are charged for the following fees:

-   Fees for accessing Log Service

    When you use the global acceleration feature to access Log Service, you are charged based on the pay-as-you-go method. The fees are the same as when you access Log Service over the Internet. Free quotas are provided. For more information about billing, see [Pay-as-you-go](/intl.en-US/Pricing/Pay-as-you-go.md).

-   Fees for using the DCDN service

    For more information, see [Billing of Dynamic Route for CDN](https://www.alibabacloud.com/product/dcdn/pricing?spm=a3c0i.20793967.4673095820.1.28c039c90p5qEo).


## Scenarios

-   Advertising

    Advertisements can run on mobile devices and PCs across the world. Log data about ad views and clicks is crucial to the billing of advertisements. In some remote areas, data transmission over the Internet is less stable. Logs may be lost during transmission. The global acceleration feature provides a more stable and reliable channel to upload logs.

-   Gaming

    The gaming industry requires timely and stable data collection from various sources, such as official websites, logon services, sales services, and gaming services. If data is collected from mobile games or globalized games, timely and stable data collection is hard to guarantee. In this case, we recommend that you use the global acceleration feature.

-   Finance

    Financial applications require highly available and secure networks. Audit logs of each transaction and user operation must be collected to servers in a reliable and secure way. Mobile transactions such as online banking, credit card malls, and mobile securities are popular. The HTTPS acceleration feature provides a secure, fast, and stable channel to collect transaction logs.

-   IoT

    IoT devices and smart devices such as smart speakers and smart watches can send collected data such as sensor data, operations logs, and critical system logs to servers for data analysis. These devices are distributed all over the world. However, their surrounding networks are not always reliable. For stable and reliable collection of logs, we recommend that you use the global acceleration feature.


## Acceleration effects

|Regions|Latency in ms \(Internet\)|Latency in ms \(global acceleration\)|Percentage of timed-out requests \(Internet\)|Percentage of timed-out requests \(global acceleration\)|
|:------|:-------------------------|:------------------------------------|:--------------------------------------------|:-------------------------------------------------------|
|China \(Hangzhou\)|152.881|128.501|0.0|0.0|
|Regions of Europe|1750.738|614.227|0.5908|0.0|
|Regions of US|736.614|458.340|0.0010|0.0|
|Singapore \(Singapore\)|567.287|277.897|0.0024|0.0|
|UAE \(Dubai\)|2849.070|444.523|1.0168|0.0|
|Australia \(Sydney\)|1491.864|538.403|0.014|0.0|

The test environment is described as follows:

-   Region of Log Service: China \(Hohhot\)
-   Average size of uploaded packets: 10 KB
-   Test time range: one day \(average\)
-   Request protocol: HTTPS
-   Request server: Alibaba Cloud Elastic Compute Service \(ECS\) \(1 vCPU and 1 GB memory\)

**Note:** The acceleration effects are for reference only.

