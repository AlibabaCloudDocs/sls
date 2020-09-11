# Configure log collection

This topic describes how to configure log collection for Alibaba Cloud services in the Log Audit Service application.

-   An Alibaba Cloud account is created.

    We recommend that you use a RAM user of an Alibaba Cloud account to configure log collection. The RAM user must be granted read permissions on RAM. For this purpose, you can attach the AliyunRAMReadOnlyAccess policy to the RAM user. In addition, the RAM user must be granted the read and write permissions on Log Service. For this purpose, you can attach the AliyunLogFullAccess policy to the RAM user.

-   Log Service is activated.

    When you log on to the [Log Service console](https://sls.console.aliyun.com) for the first time, activate Log Service as prompted.

-   Relevant features are enabled for the Alibaba Cloud services from which you want to collect logs. For more information, see [Supported cloud services](/intl.en-US/Application/Log Audit Service/Overview.md).

## Initial configurations

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Log Application section, click **Start** in the **Log Audit Service** section.

3.  Choose **Access to Cloud Products** \> **Global Configurations**. Then, perform the following steps:

    1.  In the **Region of the Central Project** section, select a region for centralized storage of log data.

        The available regions include China \(Beijing\), China \(Hohhot\), China \(Hangzhou\), China \(Shanghai\), China \(Shenzhen\), Singapore \(Singapore\), and Japan \(Tokyo\).

    2.  In the Cloud Products column, find the service for which you want to enable log audit, and set the Collection Period parameter to specify the retention period for log data.

        You can also set **Synchronization to Central Project** for layer-7 access logs of SLB, OSS access logs, and PolarDB-X audit logs. After you turn on the **Synchronization to Central Project** switch, the retention period is automatically shortened to the recommended period set by the console. This is because the project for regional storage serves as a temporary storage space.

    3.  Configure authorization for log data collection.

        You can select Manual Authorization or AccessKey Pair-Based Authorization.

        -   Manual Authorization: For more information, see [Authorize Log Service to collect cloud service logs of the current Alibaba Cloud account](/intl.en-US/Application/Log Audit Service/Authorize Log Service to collect and synchronize logs.md).
        -   AccessKey Pair-Based Authorization: Enter the AccessKey ID and AccessKey secret of the logon account. The AccessKey pair is for temporary use and is not saved.

            If you enter the AccessKey pair of a RAM user, the RAM user must have the read and write permissions on RAM. For this purpose, you can attach the AliyunRAMFullAccess policy to the RAM user.

    4.  Click **Save**.

4.  In the left-side navigation pane, choose **Access to Cloud Products** \> **Status Dashboard** to view the access status of logs.

    After you complete the configurations, the initial synchronization is completed in about 2 minutes. If an exception occurs, modify the configurations as prompted. For more information, see [FAQ](#section_7az_6wh_x2p).


## Configure multiple accounts for log collection

The Log Audit Service application allows you to collect logs from the Alibaba Cloud services of another Alibaba Cloud account and send the logs to a Logstore of the current Alibaba Cloud account. Before you implement cross-account log collection, authorize the relevant accounts.

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Log Application section, click **Start** in the **Log Audit Service** section.

3.  Choose **Multi-Account Configurations** \> **Global Configurations**. Then, perform the following steps:

    You can select Manual Authorization or AccessKey Pair-Based Authorization.

    -   Manual Authorization: Enter one or more Alibaba Cloud account IDs. For information about how to grant relevant permissions to the accounts, see [Authorize Log Service to collect logs from the cloud services purchased by other Alibaba Cloud accounts](/intl.en-US/Application/Log Audit Service/Authorize Log Service to collect and synchronize logs.md).
    -   AccessKey Pair-Based Authorization: in **AccessKey Pair for Other Accounts to Authorize Log Service**, enter the AccessKey ID and AccessKey secret of other accounts and the ID of the primary account. The AccessKey pairs are for temporary use and are not saved.

        If you enter the AccessKey pair of a RAM user, the RAM user must have the read and write permissions on RAM. For this purpose, you can attach the AliyunRAMFullAccess policy to the RAM user.

4.  In the left-side navigation pane, choose **Access to Cloud Products** \> **Status Dashboard** to view the access status of logs.

    After you complete the configurations, the initial synchronization is completed in about 2 minutes. If an exception occurs, modify the configurations as prompted. For more information, see [FAQ](#section_7az_6wh_x2p).


## Stop log collection

If you no longer need to collect logs of Alibaba Cloud services but want to retain the collected logs, you can perform the following steps:

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Log Application section, click **Start** in the **Log Audit Service** section.

3.  Choose **Access to Cloud Products** \> **Global Configurations**. In the upper-right corner of the page that appears, click **Modify**.

4.  Turn off the Audit-Related Logs switch for the target Alibaba Cloud service and click **OK**.


## Delete log audit resources

To delete the resources of the Log Audit Service application, such as Logstores, dashboards, and alerts, perform the following steps:

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Log Application section, click **Start** in the **Log Audit Service** section.

3.  Choose **Access to Cloud Products** \> **Global Configurations**. In the upper-right corner of the page that appears, click **Delete Audit Resources**.

4.  Delete the resources as prompted.


## FAQ

-   How can I view the access status of logs?

    Choose **Access to Cloud Products** \> **Status Dashboard**. On the page that appears, you can view the access status of logs.

-   What can I do if an Alibaba Cloud account does not have relevant permissions or the AccessKey pair is invalid?

    Check whether you have granted relevant permissions to the account. For information about log collection from services of a single Alibaba Cloud account, see [Authorize Log Service to collect cloud service logs of the current Alibaba Cloud account](/intl.en-US/Application/Log Audit Service/Authorize Log Service to collect and synchronize logs.md). For information about log collection across multiple Alibaba Cloud accounts, see [Authorize Log Service to collect logs from the cloud services purchased by other Alibaba Cloud accounts](/intl.en-US/Application/Log Audit Service/Authorize Log Service to collect and synchronize logs.md). For example, if such an error occurs, the probable cause is that you have not attached the **ReadOnlyAccess** policy under **System Policy** to the sls-audit-service-monitor role.

-   What can I do if a required feature is not enabled for the account?

    In most cases, this error occurs because you have not enabled a specific feature for an Alibaba Cloud service. For example, you configured log audit for Security Center, but you have not activated the **Log Analysis** feature. For more information, see [Supported cloud services](/intl.en-US/Application/Log Audit Service/Overview.md).


