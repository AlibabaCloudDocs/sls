# Enable the log analysis feature

This topic describes how to enable Log Service in the Web Application Firewall \(WAF\) console. After you enable the log analysis feature, you can dump the logs of WAF to Log Service.

-   A subscription-based WAF instance is created. The edition of WAF is Pro, Business, or Enterprise. For more information, see [Activate Alibaba Cloud WAF](/intl.en-US/Pricing/Subscription/Activate Alibaba Cloud WAF.md).

    **Note:**

    -   On the buy page of WAF, set **Access Log Service** to **YES**, and specify the period for which you want to store WAF logs, and specify the maximum size of stored logs.
    -   If you have activated the Basic Edition, you must upgrade WAF to the Pro Edition, Business Edition, or Enterprise Edition before you enable the log analysis feature. For more information, see [t15543.md\#section\_h76\_hk9\_g1k](/intl.en-US/Pricing/Renewal and upgrade.md).
-   Your website is added to WAF. For more information, see [Add your website to WAF](/intl.en-US/Website Access/Tutorial.md).

1.  Log on to the [Web Application Firewall console](https://yundun.console.aliyun.com/?p=waf).

2.  In the left-side navigation pane, choose **Log Management** \> **Log Service**.

3.  On the Log Service page, authorize WAF to use the AliyunWAFAccessingLogRole role to access Log Service.

    **Note:**

    -   This operation is required only when you enable Log Service for the first time.
    -   If you need to enable Log Service as a Resource Access Management \(RAM\) user, the RAM user must be authorized by its parent Alibaba Cloud account.
    -   Do not cancel the authorization or delete the RAM role. Otherwise, the logs of WAF cannot be dumped to Log Service.
4.  On the Log Service page, click **Open now**.

5.  On the Log Service page, select the domain name of your website that is protected by WAF, and turn on the **Status** switch to enable the log analysis feature.


After the logs of WAF are dumped to Log Service, you can query, analyze, download, ship, and transform the dumped logs. You can also create alerts. For more information, see [Common operations on logs of Alibaba Cloud services](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).

