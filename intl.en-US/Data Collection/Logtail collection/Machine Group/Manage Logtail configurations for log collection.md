# Manage Logtail configurations for log collection

This topic describes how to create, view, modify, or delete Logtail configurations for log collection in the Log Service console.

## Create Logtail configurations

For more information about how to create Logtail configurations in the Log Service console, see [Collect text logs](/intl.en-US/Data Collection/Logtail collection/Text logs/Overview.md).

## View Logtail configurations

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the target project.

3.  Choose **Log Management** \> **Logstores**. Click the **\>** icon of the target Logtail configurations. Choose **Data Import** \> **Logtail Configurations**.

4.  Click the target Logtail configurations to view the details of the configurations.


## Modify Logtail configurations

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the target project.

3.  Choose **Log Management** \> **Logstores**. Click the **\>** icon of the target Logtail configurations. Choose **Data Import** \> **Logtail Configurations**.

4.  In the **Logtail Configurations** list, click the target Logtail configurations.

5.  On the Logtail Configurations page, click **Modify**.

6.  Modify the configurations based on your business requirements and then click **Save**.

    For more information about the parameters of the configurations, see [Collect text logs](/intl.en-US/Data Collection/Logtail collection/Text logs/Collect logs in the simple mode.md).


## Delete Logtail configurations

1.  In the **Logtail Configurations** list, find the target Logtail configurations. Click the ![Logtail configuration management](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5123359951/p52766.png) icon on the right of the target Logtail configurations, and then select **Delete**.

2.  In the dialog box that appears, click **OK**.

    After the target Logtail configurations are deleted, the configurations are disassociated from the relevant machine group. Logtail no longer collects logs based on the configurations.

    **Note:** Before you can delete a Logstore, you must delete all Logtail configurations that are associated with the Logstore.


