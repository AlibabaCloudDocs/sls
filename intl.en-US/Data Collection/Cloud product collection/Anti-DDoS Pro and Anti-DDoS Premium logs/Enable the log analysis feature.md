# Enable the log analysis feature

This topic describes how to enable the log analysis feature in the Anti-DDoS Pro or Anti-DDoS Premium console. The log analysis feature allows you to collect the logs of Anti-DDoS Pro or Anti-DDoS Premium to Log Service.

-   An Anti-DDoS Pro or Anti-DDoS Premium instance is created. For more information, see [Purchase mitigation plans for Anti-DDoS Pro and Anti-DDoS Premium](/intl.en-US/Anti-DDoS Pro & Premium User Guide/Purchase mitigation plans for Anti-DDoS Pro and Anti-DDoS Premium.md).
-   One or more websites are added to the Anti-DDoS Pro or Anti-DDoS Premium instance. For more information, see [Add a website](/intl.en-US/Anti-DDoS Pro & Premium User Guide/Provisioning/Use domains/Add a website.md).

## Procedure

1.  Log on to the [Anti-DDoS Pro console](https://yundun.console.aliyun.com/?p=ddoscoo).

2.  In the upper-left corner of the page, select **Mainland China**.

    After you select Mainland China, the Anti-DDoS Pro console appears. If you want to use Anti-DDoS Premium, select **Outside Mainland China**.

3.  In the left-side navigation pane, choose **Investigation** \> **Log Analysis**.

4.  Select a log storage capacity and duration on the buy page.

    If you have purchased a log storage capacity and duration, skip this step.

    1.  Click **Purchase Now**.

    2.  On the buy page, set the required parameters. The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |Applicable Product|Select **Anti-DDoS Pro**. In this example, select Anti-DDoS Pro. If you want to use Anti-DDoS Premium, select **Anti-DDoS Premium**. |
        |Logservice Storage|Select a log storage capacity. Unit: TB. If the log storage space is full, no more logs can be stored. In most cases, each request log occupies about 2 KB of storage space. If the average request volume of your workload is 500 queries per second \(QPS\), the storage space required for one day is: 500 × 60 × 60 × 24 × 2 = 86,400,000 KB \(about 82 GB\). By default, logs are stored for 180 days. If you need to store logs that are generated in the last 180 days, you must select a log storage capacity that is larger than 14,832 GB \(about 14.5 TB\). |
        |Duration|Select a validity period for the log analysis feature. After the validity period expires, no more logs can be stored. **Warning:** If the log analysis feature expires and is not renewed within seven days, the Anti-DDoS Pro server clears all logs that are stored in the Logstore of your instance. |

    3.  Click **Buy Now** to complete the payment.

5.  On the Log Analysis page, authorize Anti-DDoS Pro to use the AliyunDDoSCOOLogArchiveRole role to access Log Service.

    **Note:**

    -   This operation is required only when you enable the log analysis feature for the first time. You must complete the authorization by using your Alibaba Cloud account.
    -   If you use a RAM user to log on to Anti-DDoS Pro or Anti-DDoS Premium, you must grant required permissions to the RAM user. For more information, see [RAM user authorization](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).
    -   To ensure that logs can be collected to Log Service, do not revoke permissions from the RAM role or delete the RAM role.
6.  On the Log Analysis page, select the website domain, and turn on the Status switch to enable the log analysis feature.

    **Note:** We recommend that you check the remaining log storage space and validity period at regular intervals when you use the log analysis feature. If the storage space usage exceeds 70%, we recommend that you upgrade the log storage space. Otherwise, you cannot store additional logs and you may lose data.


After the logs of Anti-DDoS Pro or Anti-DDoS Premium are collected to Log Service, you can query, analyze, download, ship, and transform the collected logs. You can also configure alerts for the logs. For more information, see [Common operations on logs of Alibaba Cloud services](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).

