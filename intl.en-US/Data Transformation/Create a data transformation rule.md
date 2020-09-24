# Create a data transformation rule

This topic describes how to configure a data transformation rule in the Log Service console.

-   A project and a Logstore are created. For more information, see [Create a project and a Logstore](/intl.en-US/Quick Start/Quick start.md).
-   Log data is collected. For more information, see [Data collection](/intl.en-US/Data Collection/Log collection methods.md).

The data transformation feature of Log Service allows you to read log data from the source Logstore and process and filter the data. You can create a transformation rule to transform raw log data that is continuously updated or generated within a specified time period. Then, you can write the transformed log data to multiple Logstores. You can also query the transformed log data.

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the project from which you want to transform data.

3.  Enable the data transformation mode.

    You can enable the data transformation mode by using one of the following methods:

    -   Choose **Log Management** \> **Logstores**. Click the source Logstore. On the search and analytics page that appears, turn on the **Data Transformation** switch. The data transformation page appears.
    -   Choose **Log Management** \> **Logstores**. On the Logstores tab, find the source Logstore, and click the **\>** icon of the Logstore. Choose **Data Transformation** \> **Data Transformation**. Click the plus sign \(+\) next to **Data Transformation**. The data transformation page appears.
4.  On the data transformation page, set a query time range of raw log data.

    Make sure that data is available on the Raw Logs tab.

5.  Enter a transformation rule in the text box.

    For more information, see [Data transformation syntax](/intl.en-US/Data Transformation/Data processing syntax/Language introduction.md).

6.  Preview data.

    You can preview data in Quick mode or Advanced mode. For more information, see [Configure preview modes](/intl.en-US/Data Transformation/Configure preview modes.md).

7.  View the transformation results.

    -   If data fails to be transformed because the syntax of the transformation rule or permissions are invalid, troubleshoot the failure as prompted.
    -   If the transformed data is returned as expected, perform the operations in [Step 8](#step_snp_zml_13r).
8.  Save the data transformation rule.

    1.  Click **Save as Transformation Rule**.

    2.  On the Create Data Transformation Rule page, set the parameters, and then click **OK**. The following table describes the parameters.

        You can create multiple storage targets for transformation results and store the transformation results in multiple destination Logstores.

        **Note:**

        -   You can use the e\_output or e\_couput function in the data transformation rule. You can use the name parameter to write specific data to specific destination Logstores.
        -   If you do not use the e\_output function in the transformation rule, the transformed data is written to the Logstore that stores the first storage target. If you need to store transformed data in only one destination Logstore, you do not need to use the e\_output function in the transformation rule.
        |Parameter|Description|
        |---------|-----------|
        |Rule Name|The name of the transformation rule.|
        |Mode|You can authorize Log Service to read data from a source Logstore by use one of the following methods:        -   Default role: Authorize Log Service to assume the system role AliyunLogETLRole to read data from the source Logstore.

Click **Authorize the system role AliyunLogETLRole** and authorize Log Service as prompted.

**Note:**

            -   If you use a RAM user to log on to Log Service, you must grant read/write permissions on Log Service to the RAM user by using an Alibaba Cloud account.
            -   If you have authorized Log Service to assume the system role, skip this step.
        -   Custom role: Authorize Log Service to assume a custom role to read data from the source Logstore.

In the **Role ARN** field, enter the ARN of a custom role. For more information, see [Authorize Log Service to assume a custom role](/intl.en-US/Data Transformation/Authorize Log Service/Authorize Log Service to assume a custom role.md).

        -   AccessKey pair: Log Service uses the AccessKey pair of an Alibaba Cloud account or RAM user to read data from the source Logstore. The Alibaba Cloud account or RAM user must have read permissions on the source Logstore.

Enter an AccessKey ID in the **AccessKey** ID field and the corresponding AccessKey secret in the **AccessKey Secret** field. For more information, see [Configure an AccessKey pair for a RAM user to access a source Logstore and a destination Logstore](/intl.en-US/Data Transformation/Authorize Log Service/Configure an AccessKey pair for a RAM user to access a source Logstore and a destination
         Logstore.md). |
        |Storage Target|
        |Target Name|The logical name of the storage target.|
        |Target Project|The name of the destination project to which the storage target is written. You can select a project in the same region as the source project.|
        |Target Logstore|The name of the destination Logstore to which the storage target is written.|
        |Authorization Method|You can authorize Log Service to write data to the destination Logstore by use one of the following methods:        -   Default role: Authorize Log Service to assume the system role AliyunLogETLRole so that Log Service can write data to the destination Logstore.

Click **Authorize the system role AliyunLogETLRole** and authorize Log Service as prompted.

**Note:**

            -   If you use a RAM user to log on to Log Service, you must grant read/write permissions on Log Service to the RAM user by using an Alibaba Cloud account.
            -   If you have authorized Log Service to assume the system role, skip this step.
        -   Custom role: Authorize Log Service to assume a custom role so that Log Service can write data to the destination Logstore.

In the **Role ARN** field, enter the ARN of a custom role. For more information, see [Authorize Log Service to write data to a destination Logstore](/intl.en-US/Data Transformation/Authorize Log Service/Authorize Log Service to assume a custom role.mdsection_v6z_5m4_cyt).

        -   AccessKey pair: Log Service uses the AccessKey pair of an Alibaba Cloud account or RAM user to write transformed data to the destination Logstore. The Alibaba Cloud account or RAM user must have read/write permissions on the destination Logstore.

Enter an AccessKey ID in the **AccessKey ID** field and the corresponding AccessKey secret in the **AccessKey Secret** field. For more information, see [Create an AccessKey pair for a RAM user to access a destination Logstore](/intl.en-US/Data Transformation/Authorize Log Service/Configure an AccessKey pair for a RAM user to access a source Logstore and a destination
         Logstore.mdsection_6rs_1c4_9h8). |
        |Processing Range|
        |Time Range|Valid values:         -   **All**: transforms data in the source Logstore from the first log entry until the transformation task is manually stopped.
        -   **From Specific Time**: transforms data in the source Logstore from the position that corresponds to the specified start time until the transformation task is manually stopped.
        -   **Within Specific Period**: transforms data in the source Logstore from the position that corresponds to the specified start time to the position that corresponds to the specified end time.
**Note:** The time range that is specified by the **Time Range** parameter is subject to the log receiving time. |
        |Advanced|
        |Advanced Parameter Settings|Set the required passwords in the transformation rule in the key-value pair format. For example, you can set the password that is used to connect to a database in the key-value pair format. Passwords are referenced in the transformation rule syntax by using the `${key}` variable.You can click the plus sign \(**+**\) multiple times to create multiple key-value pairs. For example, config.vpc.vpc\_id.test1:vpc-uf6mskb0b\*\*\*\*n9yj indicates the ID of the VPC where an RDS instance resides.

![Advanced Parameter Settings](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7888280061/p130800.png) |

        After you save the transformation rule, the rule is automatically executed.


After a data transformation task is created, you can manage the task on the Data Transformation Overview page. For more information, see [Manage a data transformation task](/intl.en-US/Data Transformation/Manage a data transformation task.md).

