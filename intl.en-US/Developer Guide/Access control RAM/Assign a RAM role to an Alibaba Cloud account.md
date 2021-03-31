# Assign a RAM role to an Alibaba Cloud account

This topic describes how to assign a Resource Access Management \(RAM\) role to a trusted entity. You can grant a RAM role the permissions to access Log Service and assign the role to a trusted entity. This allows the trusted entity to manage Log Service resources. To do so, you must use your Alibaba Cloud account to create a RAM role. You must grant permissions to the RAM role, and grant the AssumeRole permission to a RAM user of an Alibaba Cloud account. Then, you can assign the role to the RAM user and receive a Security Token Service \(STS\) token for the role.

Roles and users are identities used in RAM. A RAM role is a virtual identity that does not have a credential, such as a password or an AccessKey pair. You can assign the RAM role to a trusted entity, such as an Alibaba Cloud account, RAM user, or Alibaba Cloud service. After the trusted entity receives an STS token for the RAM role, the trusted entity can use the STS token to access the resources that the RAM role is authorized to use.

## Step 1: Create a RAM role and specify an Alibaba Cloud account for the RAM role

1.  Log on to the [RAM console](https://ram.console.aliyun.com).

2.  In the left-side navigation pane, click **RAM Roles**.

3.  On the RAM Roles page, click **Create RAM Role**.

4.  Select **Alibaba Cloud Account** as the trusted entity type, and then click **Next**.

5.  The following table describes the parameters of the role configuration.

    |Parameter|Description|
    |---------|-----------|
    |RAM Role Name|Enter the name of the RAM role.|
    |Note|Enter the description of the RAM role.|
    |Select Trusted Alibaba Cloud Account|Select **Current Alibaba Cloud Account** or **Other Alibaba Cloud Account**.     -   To assign the RAM role to a RAM user of your Alibaba Cloud account, select **Current Alibaba Cloud Account**. For example, you can authorize a mobile app to access Log Service resources as a RAM user.
    -   To assign the RAM role to a RAM user of another Alibaba Cloud account, select **Other Alibaba Cloud Account** and enter the ID of the other Alibaba Cloud account in the field. For example, you can authorize another Alibaba Cloud account to access the resources of your Alibaba Cloud account. |

6.  Click **OK**.


## Step 2: Grant permissions to the RAM role

After you create a RAM role, the RAM role does not have permissions on Alibaba Cloud resources. You must grant the RAM role the permissions to manage Log Service resources. The Alibaba Cloud account that is specified in the previous step can assume the RAM role and manage Log Service resources.

You can attach one or more policies to a RAM role. The policies include system policies and custom policies. To grant full access permissions on Log Service to a RAM role, perform the following steps:

1.  In the left-side navigation pane, click **RAM Roles**.

2.  On the RAM Roles page, find the RAM role, and click **Add Permissions** in the Actions column.

3.  In the Add Permissions panel, click **System Policy**, select the **AliyunLogFullAccess** policy, and then click **OK**.

4.  Confirm the authorization result, and then click **Complete**.


## Step 3: Assign the RAM role to a RAM user of the specified Alibaba Cloud account

To use a RAM role, a RAM user must first assume the RAM role. This allows the RAM user to manage resources that the RAM role is authorized to use.

The specified Alibaba Cloud account must attach the **AssumeRole** policy to a RAM user of the account. Then, the RAM user can call the AssumeRole operation and assume the RAM role that is created in [Step 1: Create a RAM role and specify an Alibaba Cloud account for the RAM role](#section_n0o_15u_v3b).

1.  In the left-side navigation pane, choose **Identities** \> **Users**.

2.  On the Users page, find the RAM user, and then click **Add Permissions** in the Actions column.

3.  In the Add Permissions panel, click **System Policy**, select the **AliyunSTSAssumeRoleAccess** policy, and then click **OK**.

4.  Confirm the authorization result, and then click **Complete**.


## Step 4: Obtain an STS token for the RAM role

After a RAM user is granted the AssumeRole permission, the RAM user calls the [AssumeRole](/intl.en-US/API Reference/API Reference (STS)/Operation interfaces/AssumeRole.md) operation to obtain a temporary STS token for the RAM role that is created in [Step 1: Create a RAM role and specify an Alibaba Cloud account for the RAM role](#section_n0o_15u_v3b).

**Note:**

-   For more information about how to call the AssumeRole operation, see [SDK for Java](/intl.en-US/SDK Reference/STS SDK Reference/SDK for Java.md).
-   After a RAM user obtains the AccessKey ID, AccessKey secret, and STS token, the RAM user can access Log Service by using the SDKs. For more information, see [Overview](/intl.en-US/Developer Guide/SDK Reference/Overview.md).

