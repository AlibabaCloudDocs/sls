# Use STS to enable cross-account access to Log Service resources

You can use an Alibaba Cloud account \(Alibaba Cloud Account A\) to create a RAM role, specify another Alibaba Cloud account \(Alibaba Cloud Account B\) to assume the role, and grant the RAM role the specific permissions on Log Service resources of Alibaba Cloud Account A. Then, you can grant the AssumeRole permission to the RAM users of Alibaba Cloud Account B. Then, you can use Alibaba Cloud Account B or the specified RAM users to call a Security Token Service \(STS\) API operation to obtain temporary security credentials. These credentials include the AccessKey ID, AccessKey secret, and security token. This way, you can use these RAM users to call Log Service API operations and access Log Service resources.

To isolate business data or outsource projects, the user of Alibaba Cloud Account A wants to grant Alibaba Cloud Account B the specific permissions on Log Service resources of Alibaba Cloud Account A. This way, the user of Alibaba Cloud Account B can manage and maintain the specified resources. The following permissions are granted:

-   The user of Alibaba Cloud Account B is authorized to write data to Log Service of Alibaba Cloud Account A. In addition, the user of Alibaba Cloud Account B is authorized to use consumer groups to consume data from Log Service of Alibaba Cloud Account A.
-   The specified RAM users of Alibaba Cloud Account B are authorized to write data to Log Service of Alibaba Cloud Account A. In addition, the specified RAM users are authorized to use consumer groups to consume data from Log Service of Alibaba Cloud Account A.
-   The user of Alibaba Cloud Account B is authorized to call a Security Token Service \(STS\) API operation to obtain temporary security credentials and use the credentials to call Log Service API operations and access Log Service resources. For more information, see [STS](/intl.en-US/API Reference/API Reference (STS)/What is STS?.md).

## Overview of the authorization process

1.  The user of Alibaba Cloud Account B creates a RAM role, specifies Alibaba Cloud Account B to assume this role, and grants Alibaba Cloud Account B the specified permissions on Log Service resources of Alibaba Cloud Account A.
2.  The user of Alibaba Cloud Account B creates RAM User B1 and attaches the `AliyunSTSAssumeRoleAccess` policy to RAM User B1. This allows the user of RAM User B1 to call the STS AssumeRole API operation.
3.  The user of RAM User B1 calls the STS AssumeRole API operation. This allows RAM User B1 to initiate Log Service API requests and manage the Log Service resources of Alibaba Cloud Account A.

## Step 1: The user of Alibaba Cloud Account A creates a RAM role for Alibaba Cloud Account B

The user of Alibaba Cloud Account A creates a RAM role, specifies Alibaba Cloud Account B to assume the role, and grants the RAM role the specific permissions on Log Service resources of Alibaba Cloud Account A.

You can create a RAM role in the RAM console. For more information, see [Create a RAM user and authorize the RAM user to access Log Service](/intl.en-US/Developer Guide/Access control RAM/Create a RAM user and authorize the RAM user to access Log Service.md). You can also call the CreateRole API operation to create a RAM role. For more information, see [CreateRole](https://api.aliyun.com/?product=Ram&api=CreateRole#/?product=Ram&api=CreateRole). The following example shows how to create a RAM user in the console.

1.  Log on to the [RAM console](https://ram.console.aliyun.com/) by using Alibaba Cloud Account A.

2.  Create a RAM role and specify Alibaba Cloud Account B to assume the RAM role.

    1.  In the left-side navigation pane, click **RAM Roles**.

    2.  On the RAM Roles page, click **Create RAM Role**.

    3.  In the Select Role Type step of the Create RAM Role wizard, select **Alibaba Cloud Account** as the trusted entity, and then click **Next**.

    4.  In the Configure Role step, enter a role name in the **RAM Role Name** field and notes about the role in the **Note** field. Select **Other Alibaba Cloud Account** for the **Select Trusted Alibaba Cloud Account** field, and then click **OK**.

        **Note:** To view the ID, move your pointer over the profile picture in the upper-right corner of the console, and then click **Security Settings**.

        The following sample shows the RAM role that was created in the preceding steps:

        ```
        {
          "Statement": [
            {
              "Action": "sts:AssumeRole",
              "Effect": "Allow",
              "Principal": {
                "RAM": [
                  "acs:ram::<The ID of Alibaba Cloud Account B>:root"
                ]
              }
            }
          ],
          "Version": "1"
        }
        ```

3.  Create a permission policy.

    1.  In the left-side navigation pane, click **Policies** under **Permissions**.

    2.  On the Policies page, click **Create Policy**.

    3.  On the Create Custom Policy page, set the parameters. The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |Name|The policy name.|
        |Note|The notes on the policy.|
        |Configuration Mode|Select **Script**.|
        |Policy Document|The permissions that the user of Alibaba Cloud Account A grants to the user of Alibaba Cloud Account B. The following sample policy specifies the write permissions on Log Service:

        ```
{
  "Version": "1",
  "Statement": [
    {
      "Action": "log:PostLogStoreLogs",
      "Resource": "*",
      "Effect": "Allow"
    }
  ]
}
        ```

The following sample policy specifies the permission to use consumer groups to pull data from Log Service:

        ```
{
  "Version": "1",
  "Statement": [
    {
      "Action": [
         "log:GetCursorOrData",
         "log:CreateConsumerGroup",
         "log:ListConsumerGroup",
         "log:ConsumerGroupUpdateCheckPoint",
         "log:ConsumerGroupHeartBeat",
         "log:GetConsumerGroupCheckPoint",
         "log:UpdateConsumerGroup"
      ],
      "Resource": "*",
      "Effect": "Allow"
    }
  ]
}
        ```

The preceding two sample policies specify permissions on all projects and Logstores of Alibaba Cloud Account A. If you want to grant permissions on a specified project and Logstore, include the following code in the Resource field of the sample policies:

        -   To grant permissions on a specified project, use `acs:log::\{projectOwnerAliUid\}:project/`.
        -   To grant permissions on a specified Logstore, use `acs:log::\{projectOwnerAliUid\}:project/\{projectName\}/logstore/\{logstoreName\}/`.
For more information, see [Log Service resources used in RAM](/intl.en-US/Developer Guide/API Reference/RAM/STS/Resource list.md). |

    4.  Click **OK**.

4.  Use Alibaba Cloud Account A to authorize the RAM role.

    1.  In the left-side navigation pane, click **RAM Roles**.

    2.  On the RAM Roles page, find the target RAM role in the **RAM Role Name** column, and then click **Add Permissions** in the Actions column.

    3.  On the Add Permissions page, select **Custom Policy** for the **Select Policy** field, select the policy that you create in [Step 3](#step_skd_d9f_wb3), and then click **OK**.

    4.  Click **Complete**.


## Step 2: The user of Alibaba Cloud Account B creates RAM User B1 and grants permissions to RAM User B1

The user of Alibaba Cloud Account B creates RAM User B1 and attaches the `AliyunSTSAssumeRoleAccess` policy to RAM User B1. This allows RAM User B1 to call the STS AssumeRole API operation.

1.  Use Alibaba Cloud Account B to log on to the [RAM console](https://ram.console.aliyun.com/).

2.  In the left-side navigation pane, choose **Devices** \> **Products**.

3.  On the Users page, click **Create User**.

4.  On the Create User page, set the parameters, and then click **OK**. The following table describes the parameters.

    |Parameter|Description|
    |---------|-----------|
    |**User Account Information**|Specify the **Logon Name** and **Display Name** parameters.|
    |**Access Method**|Select **Console Password Logon** and **Programmatic Access**.|

5.  Return to the Users page. In the **User Logon Name/Display Name** list, find the target RAM user, and then click **Add Permissions**.

6.  On the Add Permissions page, the **Principal** field is automatically filled. Select **System Policy** for the Select Policy field. In the **Authorization Policy Name** column, click the **AliyunSTSAssumeRoleAccess** policy to grant the permission specified in the policy to RAM User B1, and then click **OK**.

7.  On the page that appears, click **Complete**.


## Step 3: The user of RAM User B1 obtains STS temporary security credentials to access Log Service resources

1.  Call the AssumeRole API operation to obtain the temporary AccessKey pair and security token. For more information, see [AssumeRole](/intl.en-US/API Reference/API Reference (STS)/Operation interfaces/AssumeRole.md).

    You can call this operation by using the following method:

    Call the API operation by using the STS SDK. For more information, see [STS SDK](/intl.en-US/SDK Reference/RAM SDK Reference/SDK for Java.md).

2.  Call API operations of Log Service. For more information about the Log Service SDK, see [Overview](/intl.en-US/Developer Guide/SDK Reference/Overview.md).


## Sample code

The sample code is written in the Java programming language. The user of Alibaba Cloud Account B can use STS to write data to a project of Alibaba Cloud Account A. To download the sample code, click [Sample code](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/47277/cn_zh/1479281238498/StsSample.java).

