# Usage notes

The Layer 4 monitoring feature is available for Server Load Balancer \(SLB\) instances and the monitoring metrics can be sent to Log Service. You can view the traffic, queries per second \(QPS\), and error rate of SLB instances by using monitoring metrics that are accurate to the second. This improves fine-grained service monitoring and troubleshooting.

## Assets

-   Custom projects and Metricstores

    **Note:** We recommend that you do not delete the projects or Metricstores that are related to Layer 4 monitoring metrics for SLB instances. Otherwise, the monitoring metrics cannot be sent to Log Service.

-   Dedicated dashboards

    By default, Log Service generates two dedicated dashboards for monitoring metrics that are accurate to the second.

    **Note:** We recommend that you do not make changes to the dedicated dashboards. This may affect the usability of the dashboards. You can create a custom dashboard to visualize data query and analysis results. For more information, see [Create a dashboard](/intl.en-US/Visualization/Create a dashboard.md).

    |Dashboard|Description|
    |---------|-----------|
    |SLB Layer 4 Monitoring|Shows the trends of metrics such as SLB instances, ports, outbound traffic, inbound traffic, and the number of connections.|
    |SLB Layer 4 Data Analysis|Shows the statistics of metrics such as SLB instances, ports, outbound traffic, inbound traffic, and the number of connections.|


## Billing

-   You are not charged for the Layer 4 monitoring feature in the SLB console.
-   After the monitoring metrics for SLB instances are sent to Log Service, you are charged based on storage space, read traffic, and the number of requests. You are also charged when you transform and ship data. For more information, see [Log Service pricing](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).

## Limits

-   Only an SLB instance that is configured with a TCP or UDP listener supports the Layer 4 monitoring feature.
-   The project that is used to store monitoring metrics must reside in the same region as the specified SLB instance.

