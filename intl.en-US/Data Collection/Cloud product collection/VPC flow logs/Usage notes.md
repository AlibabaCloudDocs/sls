# Usage notes

Virtual Private Cloud \(VPC\) provides the flow log feature that is supported by Log Service. You can use this feature to record the traffic that is transferred over the elastic network interfaces \(ENIs\) and vSwitches in a VPC. You can also use this feature to record the inbound and outbound traffic of a VPC. The feature allows you to check access control rules, monitor network traffic, and fix network errors. This topic describes the resources, billing methods, and limits that are related to the flow log feature of VPC.

The traffic information that is captured by the flow log feature is written as log data to Log Service. Each log entry includes a five-tuple traffic flow captured within a specified time window. The maximum time window is about 10 minutes. During this time window, the flow log feature captures and aggregates traffic flows and then sends the traffic flows as log entries to Log Service. If you create a flow log instance for a VPC or a vSwitch, the traffic that is transferred over the ENIs in the VPC or vSwitch is captured. These ENIs include the ENIs that are attached to the vSwitch after the flow log instance is created.

## Assets

-   Projects and Logstores

    **Note:**

    -   We recommend that you do not delete the projects or Logstores that are related to VPC flow logs. Otherwise, the flow logs cannot be sent to Log Service.
    -   After the VPC flow log feature is enabled, the data retention period of the Logstores that store VPC flow logs must be changed to seven days.
-   Dedicated dashboards

    Log Service generates three dashboards for VPC flow logs by default.

    **Note:** We recommend that you do not make changes to the dedicated dashboards because this may affect the usability of the dashboards. You can create a custom dashboard to visualize log analysis results. For more information, see [Create a dashboard](/intl.en-US/Query and visualization/Dashboard/Create a dashboard.md).

    |Dashboard|Description|
    |---------|-----------|
    |Logstore Name-vpc\_flow\_log\_traffic\_cn|Displays the overall traffic flows of a VPC. The information includes the heatmap that shows the number of bytes that are sent from the source. The information also includes the top 10 destination ports by number of accepted bytes, and the number of bytes that are sent per minute by various protocols.|
    |Logstore Name-vpc\_flow\_log\_rejection\_cn|Displays the traffic flows that are rejected by security groups and network ACLs. The information includes the total number of bytes in rejected packets, percentage of rejected bytes, and percentage of rejected packets.|
    |Logstore Name-vpc\_flow\_log\_overview\_cn|Displays the overall information of a VPC. The information includes the total number of actions about traffic flows, accepted bytes, rejected bytes, and accepted packets.|


## Billing

The flow log feature allows you to ship only the captured flow logs to Log Service. The fees related to this feature include the log capture fee and the usage fee of Log Service resources.

-   Log capture fee

    The log capture fee is calculated based on the number of the captured logs.

    **Note:**

    -   You are not charged for capturing flow logs during the public preview period.
    -   Bills are generated in the VPC console.
-   The usage fee of Log Service resources

    After VPC flow logs are collected by Log Service, you are charged based on the storage space, read traffic, the number of requests, data transformation, and data shipping. For more information, see [Log Service Pricing](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).


## Limits

-   Supported regions
    -   The project that stores VPC flow logs must reside in the same region as the flow log instance.
    -   The flow log feature is available for public preview. To use this feature, you can [submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).

        The following table lists the regions where the flow log feature is supported.

        |Parameter|Supported regions|
        |---------|-----------------|
        |Asia Pacific|China \(Qingdao\), China \(Beijing\), China \(Zhangjiakou\), China \(Hohhot\), China \(Ulanqab\), China \(Hangzhou\), China \(Shanghai\), China \(Shenzhen\), China \(Heyuan\), China \(Guangzhou\), China \(Chengdu\), China \(Hong Kong\), Japan \(Tokyo\), Singapore \(Singapore\), Australia \(Sydney\), Malaysia \(Kuala Lumpur\), and Indonesia \(Jakarta\)|
        |Europe & America|US \(Silicon Valley\), US \(Virginia\), Germany \(Frankfurt\), and UK \(London\)|
        |Middle East & India|India \(Mumbai\) and UAE \(Dubai\)|

-   Resource limits

    |Item|Limit|Adjustable|
    |----|-----|----------|
    |Maximum number of flow log instances that can be created in a region|10|Submit [a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).|
    |VPCs that do not support flow logs|VPCs that contain instances of the following instance families: ecs.c1, ecs.c2, ecs.c4, ecs.ce4, ecs.cm4, ecs.d1, ecs.e3, ecs.e4, ecs.ga1, ecs.gn4, ecs.gn5, ecs.i1, ecs.m1, ecs.m2, ecs.mn4, ecs.n1, ecs.n2, ecs.n4, ecs.s1, ecs.s2, ecs.s3, ecs.se1, ecs.sn1, ecs.sn2, ecs.t1, and ecs.xn4.

|Upgrade or release an Elastic Compute Service \(ECS\) instance that does not support advanced network features.

    -   For more information, see [Upgrade the instance types of subscription instances](/intl.en-US/Instance/Change configurations/Change instance types/Upgrade the instance types of subscription instances.md) and [Change the instance type of a pay-as-you-go instance](/intl.en-US/Instance/Change configurations/Change instance types/Change the instance type of a pay-as-you-go instance.md).
    -   For more information, see [Release an instance](/intl.en-US/Instance/Manage instances/Release an instance.md).
**Note:** If the VPC to which a specified vSwitch or ENI belongs contains ECS instances, you must upgrade the ECS instances in two conditions. One condition is that the ECS instances are of the specified instance families. The other condition is that you have created flow log instances. If you do not upgrade the ECS instances in these two conditions, the flow log feature may not function as normal. |
    |vSwitches that do not support the flow log feature|VPCs to which the vSwitches belong contain ECS instances of the following instance families: ecs.c1, ecs.c2, ecs.c4, ecs.ce4, ecs.cm4, ecs.d1, ecs.e3, ecs.e4, ecs.ga1, ecs.gn4, ecs.gn5, ecs.i1, ecs.m1, ecs.m2, ecs.mn4, ecs.n1, ecs.n2, ecs.n4, ecs.s1, ecs.s2, ecs.s3, ecs.se1, ecs.sn1, ecs.sn2, ecs.t1, and ecs.xn4. |
    |ENIs that do not support flow logs|VPCs to which the ENIs belong contain ECS instances of the following instance families: ecs.c1, ecs.c2, ecs.c4, ecs.ce4, ecs.cm4, ecs.d1, ecs.e3, ecs.e4, ecs.ga1, ecs.gn4, ecs.gn5, ecs.i1, ecs.m1, ecs.m2, ecs.mn4, ecs.n1, ecs.n2, ecs.n4, ecs.s1, ecs.s2, ecs.s3, ecs.se1, ecs.sn1, ecs.sn2, ecs.t1, and ecs.xn4. |


