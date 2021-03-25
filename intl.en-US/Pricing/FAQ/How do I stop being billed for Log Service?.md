# How do I stop being billed for Log Service?

If you no longer need to use Log Service, you can delete all data to deactivate Log Service. Then, you are no longer billed for Log Service.

## Procedure

To delete your data from Log Service, you can delete your Logstores and projects. You are still charged for data storage on the day you delete the Logstores and projects. However, you are not charged the next day.

**Warning:** After you delete your Logstores and projects, all related log data and resource configurations are deleted and cannot be restored. Proceed with caution.

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Delete a Logstore.

    **Note:**

    -   Before you delete a Logstore, you must delete all Logtail configuration files that are associated with the Logstore.
    -   If the log shipping feature is enabled for a Logstore, we recommend that you stop writing data to the Logstore before you delete the Logstore. In addition, make sure that all data in the Logstore is shipped.
    -   If you are not authorized to delete a Logstore by using your Alibaba Cloud account, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm).
    1.  In the Projects section, click the name of the project that you want to delete.

    2.  Choose **Log Storage** \> **Logstores**. Click the management icon of the destination Logstore, and then choose **![Modify the configurations of a Logstore](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5023359951/p52318.png)** \> **Delete**.

    3.  In the dialog box that appears, click **OK**.

3.  Delete a project.

    1.  Go back to the Projects section, find the project, and then click **Delete** in the Actions column.

    2.  In the panel that appears, select a reason in the Reason for Deletion section, and then click **OK**.


