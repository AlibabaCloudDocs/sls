# Enable the Layer 4 monitoring feature

This topic describes how to enable the Layer 4 monitoring feature for a Server Load Balancer \(SLB\) instance in the SLB console. After you enable the feature, Layer 4 monitoring metrics are sent to Log Service. The monitoring metrics are accurate to the second.

-   An SLB instance is created. For more information, see [Create a CLB instance](/intl.en-US/Classic Load Balancer/Quick Start (New Console)/Create a CLB instance.md).
-   A TCP or UDP listener is configured for the SLB instance. For more information, see [Add a TCP listener](/intl.en-US/Classic Load Balancer/User Guide/Listeners/Add a TCP listener.md) and [Add a UDP listener](/intl.en-US/Classic Load Balancer/User Guide/Listeners/Add a UDP listener.md).
-   A Log Service project and a Metricstore are created in the region where the SLB instance resides. For more information, see [Create a project](/intl.en-US/Preparation/Manage a project.md) and [Create a Metricstore](/intl.en-US/Preparation/Manage Metricstores.md).

## Procedure

1.  Log on to the [SLB console](https://slb.console.aliyun.com/slb).

2.  In the left-side navigation pane, choose **CLB \(Formerly Known as SLB\)** \> **Instances**.

3.  On the Instances page, click the ID of the SLB instance.

4.  Click **Fine-grained Monitoring**.

5.  Enable the fine-grained monitoring feature in the current region.

    If the fine-grained monitoring feature is enabled in the current region, skip this step.

    1.  Click **Enable Fine-grained Monitoring**.

    2.  In the Enable Fine-grained Monitoring for SLB dialog box, select the project and Metricstore that you created, select the **I understand the above information** check box, and then click **OK**.

        **Note:**

        -   When you perform this operation, the system automatically creates a Resource Access Management \(RAM\) role named AliyunServiceRoleForSlbLogDelivery. You can asign this RAM role to SLB to send monitoring metrics that are accurate to the second to Log Service.
        -   To ensure that monitoring metrics for SLB instances can be sent to Log Service, do not revoke permissions from the RAM role or delete the RAM role.
6.  Enable the Layer 4 monitoring feature for the TCP or UDP listener.

    1.  On the Fine-grained Monitoring tab, click **Settings**.

    2.  On the Settings tab, turn on the **Fine-grained Monitoring** switch for the TCP or UDP listener.


After Log Service collects Layer 4 monitoring metrics for the specified SLB instance, you can query, analyze, download, ship, and transform the metrics in the Log Service console. You can also configure alerts for the metrics. For more information, see [Common operations on logs of Alibaba Cloud services](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).

