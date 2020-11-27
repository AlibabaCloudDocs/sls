# Manage a project

This topic describes how to create, modify, and delete a project in the Log Service console.

Log Service is activated.

When you log on to the [Log Service console](https://sls.console.aliyun.com) for the first time, activate Log Service as prompted.

## Create a project

**Note:** You can create a maximum of 50 projects for each Alibaba Cloud account.

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Click **Create Project** in the **Projects** section.

3.  In the Create Project dialog box, set the required parameters. The following table describes the parameters.

    |Parameter|Description|
    |---------|-----------|
    |Project Name|The name of the project. The name must be unique in a region and cannot be modified after the project is created.|
    |Description|The description of the project.|
    |Region|The region to which the project belongs. We recommend that you select a region based on the log source. For example, to collect logs of ECS instances, you can select the region where the ECS instances reside. Then, you can use the internal network of Alibaba Cloud to accelerate log collection.

After you create a project, you cannot migrate the project to another region or modify the region to which the project belongs. For more information about the endpoints of different regions, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). |

4.  Click **OK**.


## Modify a project

After a project is created, you can modify only the description of the project.

1.  Find the destination project in the **Projects** section. Click **Edit** in the Actions column.

2.  In the Modify Project dialog box, modify the description of the project.

3.  Click **OK**.


## Delete a project

**Warning:**

-   You cannot directly delete the projects that are created by using the Log Audit Service application or other cloud services. However, you can [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm?spm=a2796.7919406.0.dcontactus3.676a2d23RjosdV#/ticket/add/?productId=1210) to delete these projects.
-   After you delete a project, all logs and configurations of the project are permanently deleted. Proceed with caution.

1.  Find the destination project in the **Projects** section. Click **Delete** in the Actions column.

2.  In the Delete Project dialog box, select the reason why you want to delete the project, and click **OK**.


