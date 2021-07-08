# Enable the flow log feature

This topic describes how to enable the flow log feature in the VPC console. After you enable the feature, you can use Log Service to collect flow logs.

-   Relevant instances are created. For more information, see [Create an ENI](/intl.en-US/Network/Elastic Network Interfaces/Create an ENI.md), [Create a VPC](/intl.en-US/VPCs and vSwitchs/Work with VPCs.md), or [Create a vSwitch](/intl.en-US/VPCs and vSwitchs/Work with vSwitches.md).
-   A project and a Logstore are created in the region where the resource instances are located. For more information, see [Step 2: Create a project and a Logstore](/intl.en-US/.md).

1.  Log on to the [VPC console](https://vpcnext.console.aliyun.com).

2.  In the upper-left corner of the page, select the region where the instances reside.

3.  In the left-side navigation pane, click **Flow Log**.

4.  Authorized VPC as prompted.

    **Note:**

    -   This operation is required only when you perform the authorization for the first time. You must complete the authorization by using your Alibaba Cloud account.
    -   If you use a RAM user to log on to VPC, the RAM user must be granted relevant permissions. For more information, see [RAM user authorization](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).
    -   You must not delete the RAM role or revoke the permissions from the RAM role. Otherwise, logs cannot be shipped to Log Service.
5.  On the Flow Log page, click **Create FlowLog**.

6.  In the Create FlowLog dialog box, set the parameters and click **OK**.

    |Parameter|Description|
    |---------|-----------|
    |**Name**|The name of a flow log instance. The name must be 2 to 128 characters in length and can contain letters, digits, hyphens \(-\), and underscores \(\_\). The name must start with a letter and cannot start with `http://` or `https://`. |
    |**Resource Type**|Select the type of resources for which you want to capture traffic data, and select a resource. Valid values:     -   **Network Interface**: captures traffic data for a specified ENI.
    -   **vSwitch**: captures traffic data for all the ENIs attached to a specified vSwitch.
    -   **VPC**: captures traffic data for all the ENIs in a specified VPC.
If the VPC to which a specified vSwitch or ENI belongs contains ECS instances of the following instance families, you cannot create a flow log instance for the VPC, vSwitch, or ENI.

ecs.c1, ecs.c2, ecs.c4, ecs.ce4, ecs.cm4, ecs.d1, ecs.e3, ecs.e4, ecs.ga1, ecs.gn4, ecs.gn5, ecs.i1, ecs.m1, ecs.m2, ecs.mn4, ecs.n1, ecs.n2, ecs.n4, ecs.s1, ecs.s2, ecs.s3, ecs.se1, ecs.sn1, ecs.sn2, ecs.t1, and ecs.xn4.

In this case, you must upgrade or release the ECS instances.

    -   For more information about how to upgrade an ECS instance, see [Upgrade the instance types of subscription instances](/intl.en-US/Instance/Change configurations/Change instance types/Upgrade the instance types of subscription instances.md) or [Change the instance type of a pay-as-you-go instance](/intl.en-US/Instance/Change configurations/Change instance types/Change the instance type of a pay-as-you-go instance.md).
    -   For more information about how to release an ECS instance, see [Release an instance](/intl.en-US/Instance/Manage instance status/Release an instance.md).
**Note:** If the VPC to which a specified vSwitch or ENI belongs contains ECS instances of the specified instance families, and flow log instances are created, you must upgrade or release the ECS instances. Otherwise, the flow log feature may not function as expected. |
    |**Traffic Type**|Select the type of data traffic to be captured. Valid values:     -   **All**: captures all data traffic of the specified resource.
    -   **Allow**: captures only the data traffic allowed by the security group rules.
    -   **Drop**: captures only the data traffic that is rejected by the security group rules. |
    |**Project**|Select a project to store flow logs.**Note:** The project that stores VPC flow logs must reside in the same region as the flow log instance. You can ship flow logs of multiple resource instances in the same region to the same Logstore. |
    |**Logstore**|Select a Logstore to store flow logs.|
    |**Turn on FlowLog Analysis Report Function**|If you turn on the switch, Log Service enables the indexing feature for the Logstore and creates a dashboard.|
    |**Description**|The description of the flow log instance. The description must be 2 to 256 characters in length and cannot start with `http://` or `https://`. |


After VPC flow logs are collected by Log Service, you can query, download, ship, and transform the logs. You can also create alerts for the logs. For more information, see [Common operations on logs of Alibaba Cloud services](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).

