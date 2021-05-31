# Manage a project

A project in Log Service is the resource management unit that is used to separate and manage different resources. This topic describes how to create and delete a project in the Log Service console.

Log Service is activated.

When you log on to the [Log Service console](https://sls.console.aliyun.com) for the first time, activate Log Service as prompted.

## Create a project

**Note:** You can create a maximum of 50 projects for each Alibaba Cloud account.

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click **Create Project**.

3.  In the Create Project panel, set the required parameters. The following table describes the parameters.

    |Parameter|Description|
    |---------|-----------|
    |Project Name|The name of the project. The name must be unique in a region and cannot be modified after the project is created.|
    |Description|The description of the project.|
    |Region|Select a region based on log sources. After you create a project, you cannot modify the region where the project resides or migrate the project to another region. For example, to collect logs from an Elastic Compute Service \(ECS\) instance, you can select the region where the ECS instance resides. Then, you can use the internal network of Alibaba Cloud to accelerate log collection. |

4.  Click **OK**.


## Delete a project

**Warning:** After you delete a project, all log data stored in the project and configurations of the project are deleted and cannot be restored. Proceed with caution.

1.  In the **Projects** section, find the project that you want to delete, and then click **Delete** in the Actions column.

2.  In the Delete Project panel, select a reason in the Reason for Deletion section, and then click **OK**.


## Project-related API operations

|Action|Operation|
|------|---------|
|Create a project|[CreateProject](/intl.en-US/Developer Guide/API Reference/Project interfaces/CreateProject.md)|
|Delete a project|[DeleteProject](/intl.en-US/Developer Guide/API Reference/Project interfaces/DeleteProject.md)|
|Query a project|-   Query a specified project: [GetProject](/intl.en-US/Developer Guide/API Reference/Project interfaces/GetProject.md)
-   Query all projects: [ListProject](/intl.en-US/Developer Guide/API Reference/Project interfaces/ListProject.md) |
|Modify a project|[UpdateProject](/intl.en-US/Developer Guide/API Reference/Project interfaces/UpdateProject.md)|

