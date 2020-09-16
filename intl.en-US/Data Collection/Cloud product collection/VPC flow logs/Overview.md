# Overview

Alibaba Cloud Virtual Private Cloud \(VPC\) provides the flow log feature that is supported by Log Service. You can use this feature to record the traffic transferred over the elastic network interfaces \(ENIs\) and VSwitches in a VPC and the inbound and outbound traffic of a VPC. This topic describes the resources, billings, and limits that are related to the flow log feature of VPC.

The traffic information captured by the flow log feature is written as log data to Log Service. Each log entry includes a five-tuple traffic flow captured within a specified time window. The maximum time window is approximately 10 minutes. During the time window, the flow log feature captures and aggregates traffic flows and then sends the traffic flows as log entries to Log Service. If you create a flow log instance for a VPC or a VSwitch, the traffic that transferred over the ENIs in the VPC or VSwitch is captured. The ENIs include the ENIs that are attached to the VSwitch after the flow log instance is created.

## Resources

-   Projects and Logstores

    **Note:** You must not delete the projects or Logstores that are related to VPC flow logs. Otherwise, the flow logs cannot be sent to Log Service.

-   Dedicated dashboards

    After you enable the VPC flow log feature, three dedicated dashboards are automatically created for VPC flow logs.

    **Note:** We recommend that you do not make changes to the dedicated dashboards because this may affect the usability of the dashboards. You can customize a dashboard to display query results. For more information, see [Create a dashboard](/intl.en-US/Query and visualization/Dashboard/Create a dashboard.md).

    |Dashboard|Description|
    |---------|-----------|
    |Logstore Name-vpc\_flow\_log\_traffic\_en|Displays the overall traffic flows of a VPC. The information includes the heatmap of the number of bytes sent from the source, the top 10 destination ports by number of accepted bytes, and number of bytes that are sent per minute by various protocols.|
    |Logstore Name-vpc\_flow\_log\_rejection\_en|Displays the traffic flows that are rejected by security groups and network ACLs. The information includes the total number of bytes in rejected packets, percentage of rejected bytes, and percentage of rejected packets.|
    |Logstore Name-vpc\_flow\_log\_overview\_en|Displays the overall information of a VPC. The information includes the total number of actions about traffic flows, accepted bytes, rejected bytes, and accepted packets.|


## Billing

The flow log feature allows you to ship only flow logs to Log Service. The fees relevant to this feature include the log capture fee and Log Service resource usage fee.

-   Log capture fee

    The log capture fee is charged based on the amount of the captured logs.

    **Note:**

    -   You are not charged for capturing flow logs during the public preview period.
    -   Bills are generated in the VPC console.
-   Log Service resource usage fee

    After VPC flow logs are collected by Log Service, you are charged for the storage space that the logs occupy, data reads, read/write requests, data transformation, and data shipping. For more information, see [Billing methods](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).


## Limits

-   Supported regions
    -   The flow log feature is available only in the China \(Hohhot\), Malaysia \(Kuala Lumpur\), Indonesia \(Jakarta\), UK \(London\), and India \(Mumbai\) regions. To use this feature in other regions, submit a [ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).
    -   The project that stores VPC flow logs must reside in the same region as the flow log instance.
-   Resource limits

    |Resource|Limit|Adjustable|
    |--------|-----|----------|
    |Maximum number of flow log instance that can be created in a region|10|Submit a [ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).|
    |VPCs that do not support flow logs|VPCs that contain ECS instances of the following instance families: ecs.c1, ecs.c2, ecs.c4, ecs.ce4, ecs.cm4, ecs.d1, ecs.e3, ecs.e4, ecs.ga1, ecs.gn4, ecs.gn5, ecs.i1, ecs.m1, ecs.m2, ecs.mn4, ecs.n1, ecs.n2, ecs.n4, ecs.s1, ecs.s2, ecs.s3, ecs.se1, ecs.sn1, ecs.sn2, ecs.t1, and ecs.xn4.

|Upgrade or release an Elastic Compute Service \(ECS\) instance that does not support advanced network features.

    -   For more information, see [Upgrade configurations of subscription instances](/intl.en-US/Instance/Change configurations/Upgrade configurations/Upgrade configurations of subscription instances.md) and [Change the instance type of a pay-as-you-go instance](/intl.en-US/Instance/Change configurations/Change configurations of Pay-As-You-Go instances/Change the instance type of a pay-as-you-go instance.md).
    -   For more information, see [Release an instance](/intl.en-US/Instance/Manage instances/Release an instance.md).
**Note:** If the VPC to which a specified VSwitch or ENI belongs contains ECS instances of the specified instance families, and flow log instances are created, you must upgrade the ECS instances. Otherwise, the flow log feature may not function as expected. |
    |VSwitches that do not support the flow log feature|VPCs to which VSwitches belong contain ECS instances of the following instance families: ecs.c1, ecs.c2, ecs.c4, ecs.ce4, ecs.cm4, ecs.d1, ecs.e3, ecs.e4, ecs.ga1, ecs.gn4, ecs.gn5, ecs.i1, ecs.m1, ecs.m2, ecs.mn4, ecs.n1, ecs.n2, ecs.n4, ecs.s1, ecs.s2, ecs.s3, ecs.se1, ecs.sn1, ecs.sn2, ecs.t1, and ecs.xn4. |
    |ENIs that do not support flow logs|The VPC to which ENIs belong contains ECS instances of the following instance families: ecs.c1, ecs.c2, ecs.c4, ecs.ce4, ecs.cm4, ecs.d1, ecs.e3, ecs.e4, ecs.ga1, ecs.gn4, ecs.gn5, ecs.i1, ecs.m1, ecs.m2, ecs.mn4, ecs.n1, ecs.n2, ecs.n4, ecs.s1, ecs.s2, ecs.s3, ecs.se1, ecs.sn1, ecs.sn2, ecs.t1, and ecs.xn4. |


