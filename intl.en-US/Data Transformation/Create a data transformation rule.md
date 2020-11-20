# Create a data transformation rule

This topic describes how to create a data transformation rule in the Log Service console.

-   A project and a Logstore are created. For more information, see [Create a project and a Logstore](/intl.en-US/Quick Start/Quick start.md).
-   Log data is collected. For more information, see [Log collection methods](/intl.en-US/Data Collection/Log collection methods.md).

The data transformation feature of Log Service allows you to read, process, and filter the log data from a source Logstore. You can create a transformation rule to transform raw logs that are continuously updated or generated within a specified period of time. Then, you can write the transformed log data to multiple Logstores. You can also query and analyze the transformed log data to extract more value.

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project.

3.  Enable the data transformation mode.

    You can enable the data transformation mode by using one of the following two methods:

    -   Choose **Log Management** \> **Logstores**, and then click the source Logstore. On the search and analysis page that appears, turn on the **Data Transformation** switch.
    -   Choose **Log Management** \> **Logstores**. On the Logstores tab, click the **\>** icon of the source Logstore, choose **Data Transformation** \> **Data Transformation**, and then click the plus sign \(+\) next to **Data Transformation**. The data transformation mode is enabled.
4.  On the data transformation page, select a time range for the raw logs.

    Make sure that log data exists on the Raw Logs tab.

5.  Enter a transformation rule in the text box.

    For more information, see [Data processing syntax](/intl.en-US/Data Transformation/Data processing syntax/Language introduction.md).

6.  Preview the log data.

    You can preview the log data in Quick mode or Advanced mode. For more information, see [Configure preview modes](/intl.en-US/Data Transformation/Configure preview modes.md).

7.  View the transformation results.

    -   Due to invalid syntax of a transformation rule or configuration errors of permissions, data cannot be transformed. Troubleshoot the errors as prompted.
    -   If the transformed data is returned as expected, perform the operations in [Step 8](#step_snp_zml_13r).
8.  Save the data transformation rule.

    1.  Click **Save as Transformation Rule**.

    2.  In the Create Data Transformation Rule dialog box, set the required parameters, and then click **OK**. The following table describes the required parameters.

        You can add multiple storage targets and store the transformation results in multiple destination Logstores.

        **Note:**

        -   If you add multiple storage targets, you can set the Target Name parameter to the value of the name parameter in the e\_output or e\_couput function. This way, you can write specified data to specified destination Logstores.
        -   If you add multiple storage targets and do not use the e\_output function in a transformation rule, the transformed data is written to the destination Logstore in the first storage target. If you store the transformation results in only one destination Logstore, you do not need to use the e\_output function in a transformation rule.
        |Parameter|Description|
        |---------|-----------|
        |Rule Name|The name of the transformation rule.|
        |Authorization Method|You can authorize Log Service to read data from a source Logstore by using one of the following methods:        -   Default role: Authorize Log Service to assume the system role AliyunLogETLRole to read data from the source Logstore.

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
        |Target Region|The region where the destination project resides.To ensure the privacy of log data, HTTPS is used in data transformation across regions.

The Internet is used to transmit data in data transformation across regions. If the Internet connection is unstable, a data transformation delay may occur. You can select **DCDN Acceleration** to accelerate the cross-region transmission. You must enable the global acceleration feature for the corresponding project when you select DCDN Acceleration. For more information, see [Enable the global acceleration feature](/intl.en-US/Data Collection/Collection acceleration/Enable the global acceleration feature.md).

**Note:** You are charged for data transformation across regions based on compressed Internet traffic. For more information, see [Billing overview](/intl.en-US/Pricing/Billing overview.md). |
        |Target Project|The name of the destination project.|
        |Target Logstore|The name of the destination Logstore.|
        |Authorization Method|You can authorize Log Service to read data from and write data to a destination Logstore by using one of the following methods:        -   Default role: Authorize Log Service to assume the system role AliyunLogETLRole to write transformed data to the destination Logstore.

Click **Authorize the system role AliyunLogETLRole** and authorize Log Service as prompted.

**Note:**

            -   If you use a RAM user to log on to Log Service, you must grant read/write permissions on Log Service to the RAM user by using an Alibaba Cloud account.
            -   If you have authorized Log Service to assume the system role, skip this step.
        -   Custom role: Authorize Log Service to assume a custom role to write transformed data to the destination Logstore.

In the **Role ARN** field, enter the ARN of a custom role. For more information, see [Authorize Log Service to write data to a destination Logstore](/intl.en-US/Data Transformation/Authorize Log Service/Authorize Log Service to assume a custom role.mdsection_v6z_5m4_cyt).

        -   AccessKey pair: Log Service uses the AccessKey pair of an Alibaba Cloud account or RAM user to write transformed data to the destination Logstore. The Alibaba Cloud account or RAM user must have the read and write permissions on the destination Logstore.

Enter an AccessKey ID in the **AccessKey ID** field and the corresponding AccessKey secret in the **AccessKey Secret** field. For more information, see [Create an AccessKey pair for a RAM user to access a destination Logstore](/intl.en-US/Data Transformation/Authorize Log Service/Configure an AccessKey pair for a RAM user to access a source Logstore and a destination
         Logstore.md). |
        |Processing Range|
        |Time Range|Valid values:         -   **All**: transforms the data in the source Logstore from the first log entry until the transformation task is manually stopped.
        -   **From Specific Time**: transforms the data in the source Logstore from the log entry that is received at the specified start time until the transformation task is manually stopped.
        -   **Within Specific Period**: transforms the data in the source Logstore from the log entry that is received at the specified start time to the log entry that is received at the specified end time.
**Note:** The time range that is specified in the **Time Range** parameter refers to the time when logs are received. |
        |Advanced|
        |Advanced Parameter Settings|You can set required passwords in the key-value pair format in the transformation rule, such as a password that is used to connect to a database. Passwords are referenced in the transformation rule syntax by using the `${key}` variable.You can click the plus sign \(**+**\) to create multiple key-value pairs. For example, config.vpc.vpc\_id.test1:vpc-uf6mskb0b\*\*\*\*n9yj indicates the ID of the VPC where an RDS instance resides.

![Advanced parameter settings](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7888280061/p130800.png) |

        After you save the transformation rule, it is automatically executed.


After a data transformation task is created, you can manage the task on the Data Transformation Overview page. For more information, see [Manage a data transformation task](/intl.en-US/Data Transformation/Manage a data transformation task.md).

