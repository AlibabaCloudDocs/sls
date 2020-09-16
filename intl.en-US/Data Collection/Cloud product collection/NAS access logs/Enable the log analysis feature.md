# Enable the log analysis feature

This topic describes how to enable the log analysis feature in the Apsara File Storage NAS console and dump logs to Log Service.

-   A file system that uses the Network File System \(NFS\) protocol is created and mounted on a server. For more information, see [Mount an NFS file system]().
-   NAS is authorized to use the role AliyunNASLogArchiveRole to access Log Service.

    Click [Cloud Resource Access Authorization](https://ram.console.aliyun.com/#/role/authorize?request=%7B%22Requests%22:%7B%22request1%22:%7B%22RoleName%22:%22AliyunNASLogArchiveRole%22,%22TemplateId%22:%22LogArchiveRole%22%7D%7D,%22ReturnUrl%22:%22https:%2F%2Fnasnext.console.aliyun.com%2F%22,%22Service%22:%22NAS%22%7D), and authorize NAS as prompted.

    **Note:**

    -   This operation is required only when you enable the log analysis feature for the first time.
    -   If you want to use a Resource Access Management \(RAM\) user to manage cloud service logs, you must use the Alibaba Cloud account to authorize the RAM user.
    -   To ensure that NAS access logs can be dumped to Log Service, do not delete the RAM role.

## Procedure

1.  Log on to the [NAS console](https://nasnext.console.aliyun.com/).

2.  In the left-side navigation pane, choose **Monitoring Audit** \> **Log Analysis**.

3.  On the Log Analysis page, click **New Log Dump**.

4.  In the New Log Dump dialog box, select a system type from the **File System Type** drop-down list, select a file system from the **File System ID/Name** drop-down list, and then click **OK**.


After NAS access logs are dumped to Log Service, you can query, analyze, download, ship, and transform the dumped logs. You can also configure alerts. For more information, see [Common operations on logs of Alibaba Cloud services](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).

