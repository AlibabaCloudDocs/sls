# Access control

Log Service provides features that are related to Resource Access Management \(RAM\) policies and Security Token Service \(STS\) temporary credentials. You can use these features to control and manage resource access in Log Service.

## RAM policies based on users

RAM is a resource access control service provided by Alibaba Cloud. You can configure RAM policies based on users. You can manage users by configuring RAM policies. You can create RAM users for employees, systems, and applications and authorize the users to access the resources of your Alibaba Cloud account. You can also control the permissions that are granted to specific users on specific resources. For example, you can create a RAM policy to grant users only the read permissions on specific resources in a project or Logstore.

A RAM policy is in the JSON format. You can write a RAM policy by specifying the Action, Effect, Resource, and Condition elements in the Statement field. You can add multiple statements to a policy to implement flexible authorization. For more information, see [RAM overview](/intl.en-US/Developer Guide/Access control RAM/Overview.md).

## Temporary access authorization based on STS

RAM policies provide long-term access control. If you need to provide temporary credentials to allow users to access resources, you can use STS. You can obtain temporary AccessKey pairs and tokens by calling STS API operations. Then, you can send the AccessKey pairs and tokens to the temporary users to access Log Service. The permissions that are obtained by using STS are strictly restricted and have time limits. Therefore, the leaks of temporary credentials have a small effect on system security.

You can use STS to grant temporary access to Log Service. You can also use STS to assign an access credential that has a custom validity period and custom permissions to a third-party application or a RAM user.

