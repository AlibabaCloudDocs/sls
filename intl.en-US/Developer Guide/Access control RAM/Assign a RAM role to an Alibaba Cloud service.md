# Assign a RAM role to an Alibaba Cloud service

A trusted Alibaba Cloud service can assume a Resource Access Management \(RAM\) role to access your Alibaba Cloud resources. This topic describes how to create a RAM role and assign the RAM role to an Alibaba Cloud service.

## Create a RAM role

1.  Log on to the [RAM console](https://ram.console.aliyun.com).

2.  In the left-side navigation pane, click **RAM Roles**.

3.  On the RAM Roles page, click **Create RAM Role**.

4.  In the Create RAM Role panel, select **Alibaba Cloud Service** in the Trusted entity type section, and then click **Next**.

5.  Set the required parameters in the Configure Role step.

    The following table describes the parameters.

    |Parameter|Description|
    |---------|-----------|
    |Role Type|Select **Normal Service Role**.|
    |RAM Role Name|Enter the name of the RAM role, for example, aliyunlogreadrole.|
    |Note|Enter the description of the RAM role.|
    |Select Trusted Service|Select **Log Service** from the drop-down list.|

6.  Click **OK**.


## Grant a RAM role the permissions to access Log Service

1.  In the left-side navigation pane, click **RAM Roles**.

2.  On the RAM Roles page, find the RAM role to which you want to grant permissions, and click **Add Permissions** in the Actions column.

3.  In the Add Permissions panel, click **System Policy**, select the **AliyunLogReadOnlyAccess** policy, and then click **OK**.

4.  Verify the authorization result, and click **Complete**.


