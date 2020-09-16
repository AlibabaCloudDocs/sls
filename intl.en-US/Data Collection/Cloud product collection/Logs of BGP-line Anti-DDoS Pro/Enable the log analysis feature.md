# Enable the log analysis feature

This topic describes how to enable the log analysis feature in the Anti-DDoS Pro console. This feature allows you to dump the logs of Anti-DDoS Pro to Log Service.

-   An Anti-DDoS Pro or Anti-DDoS Premium instance is created. For more information, see [Purchase mitigation plans for Anti-DDoS Pro and Anti-DDoS Premium](/intl.en-US/Anti-DDoS Pro & Premium User Guide/Purchase mitigation plans for Anti-DDoS Pro and Anti-DDoS Premium.md).
-   One or more websites are added to the Anti-DDoS Pro or Anti-DDoS Premium instance. For more information, see [Add a website](/intl.en-US/Anti-DDoS Pro & Premium User Guide/Provisioning/Use domains/Add a website.md).

## Procedure

1.  Log on to the [Anti-DDoS Pro console](https://yundun.console.aliyun.com/?p=ddoscoo).

2.  In the upper-left corner of the page, select **Mainland China**.

    Anti-DDoS Pro is used in this example. If you want to use Anti-DDoS Premium, select **Outside Mainland China**.

3.  In the left-side navigation pane, choose **Investigation** \> **Log Analysis**.

4.  Select a log storage capacity and duration on the buy page.

    If you have selected a log storage capacity and duration, skip this step.

    1.  Click **Purchase Now**.

    2.  On the buy page, set the parameters, as described in the following table.

        |Parameter|Description|
        |---------|-----------|
        |Applicable Product|Select **Anti-DDoS Pro**. In this example, select Anti-DDoS Pro. If you want to use Anti-DDoS Premium, select **Anti-DDoS Premium**. |
        |Logservice Storage|Select a log storage capacity. Unit: TB. After the log storage capacity is exhausted, logs can no longer be stored. In most cases, each request log occupies about 2 KB of storage space. If the average request volume of your workload is 500 queries per second \(QPS\), the storage space required for one day is: 500 × 60 × 60 × 24 × 2 = 86,400,000 KB \(about 82 GB\). By default, logs are stored for 180 days. If you need to store logs that are generated in the last 180 days, you must select a log storage capacity that is larger than 14,832 GB \(about 14.5 TB\). |
        |Duration|Select a validity period for the log analysis feature. After the validity period expires, no more logs can be stored. **Warning:** If the log analysis feature expires and is not renewed within seven days, the Anti-DDoS Pro server clears all logs that are stored in the Logstore of your instance. |

    3.  Click **Buy Now** to complete the payment.

5.  On the Log Analysis page, authorize Anti-DDoS Pro to use the AliyunDDoSCOOLogArchiveRole role to access Log Service.

    **Note:**

    -   This operation is required only when you enable the log analysis feature for the first time.
    -   If you need to enable the log analysis feature as a Resource Access Management \(RAM\) user, the RAM user must be authorized by its parent Alibaba Cloud account.
    -   Do not cancel the authorization or delete the RAM role. Otherwise, the logs of Anti-DDoS Pro cannot be dumped to Log Service.
6.  On the Log Analysis page, select the website domain, and turn on the Status switch to enable the log analysis feature.

    **Note:** We recommend that you check the remaining log storage space and validity period at regular intervals when you use the log analysis feature.

    -   If the usage of log storage space reaches 70%, upgrade the storage capacity to make sure that new logs can be stored.
    -   If a large amount of storage space remains unused for a long time, downgrade the storage capacity based on your needs.

After the logs of Anti-DDoS Pro are dumped to Log Service, you can query, analyze, download, ship, and transform the dumped logs. You can also create alerts. For more information, see [Common operations on logs of Alibaba Cloud services](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).

