# Enable the log analysis feature

Before you analyze Security Center logs in the Log Service console, you must enable the log analysis feature in the Security Center console. This topic describes how to enable the log analysis feature in the Security Center console.

Log Service and Security Center are activated.

## Procedure

1.  Log on to the [Security Center console](https://yundun.console.aliyun.com/?p=sas).

2.  In the left-side navigation pane, choose **Investigation** \> **Log Analysis**.

3.  In the **Activate Log Service** wizard, click **Activate Now**.

4.  On the Purchase page, select the edition and specify the storage capacity. For more information about other parameters, see [Purchase Security Center](/intl.en-US/Pricing/Purchase Security Center.md).

    After you specify the **log storage capacity**, the log analysis feature is enabled.

    **Note:**

    -   The log analysis feature is unavailable for Security Center of the basic edition.
    -   You can view the network logs, security logs, and host logs of Security Center of the enterprise edition.
5.  Complete the payment as prompted.

    After you enable the log analysis feature, Log Service immediately collects logs from Security Center. At the same time, a dedicated project and a Logstore are automatically created and indexes are created in the Logstore.


After logs are collected, you can query, download, ship, and transform the logs. You can also create alerts for the logs. For more information, see [Common operations on logs of Alibaba Cloud services](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).

