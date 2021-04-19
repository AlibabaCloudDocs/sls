# Enable the mitigation analysis feature of Anti-DDoS Origin

This topic describes how to enable the mitigation analysis feature in the Anti-DDoS Origin console. After you enable this feature, you can view Anti-DDoS Origin logs for analysis. This requires the use of the query and analysis feature of Log Service. This can be used to protect against DDoS attacks.

An Anti-DDoS Origin instance is created. For more information, see [Purchase an Anti-DDoS Origin Enterprise instance](/intl.en-US/Anti-DDoS Origin User Guide/Purchase an Anti-DDoS Origin Enterprise instance.md).

## Procedure

1.  Log on to the [Anti-DDoS console](https://yundun.console.aliyun.com/?spm=a2c4g.11186623.2.6.233662bcGd6bPG&p=ddos#/).

2.  In the upper-left corner of the top navigation bar, select a region.

3.  In the left-side navigation pane, choose **Anti-DDoS Origin** \> **Mitigation Analysis \(Beta\)**.

4.  If you enable this feature for the first time, you must complete RAM authorization as prompted.

5.  Upgrade an Anti-DDoS Origin instance.

    1.  On the **Mitigation Analysis \(Beta\)** page, select the Anti-DDoS Origin instance that you want to upgrade from the **Instance** drop-down list.

    2.  Click **Upgrade Now**.

    3.  On the **Upgrade/Downgrade** page, select On for **Mitigation Analysis \(Beta\)**.

    4.  Read and select **Anti-DDOS origin Terms of Service** and click **Buy Now**.

    5.  Complete the payment.

        **Note:** During public preview, the mitigation analysis feature of Anti-DDoS Origin is provided free of charge.

6.  On the **Mitigation Analysis \(Beta\)** page, click **Enable Now**.

    After the feature is enabled, Anti-DDoS Origin collects mitigation logs stored in Log Service from the instance that is being used. This way, you can query and analyze mitigation logs and view mitigation reports. You can turn on or off the Status switch to enable or disable the feature.

    **Note:** When you use the log analysis feature of Anti-DDoS Origin, we recommend that you check the remaining log storage space and the validity period on a regular basis. If more than 70% of storage the space is used, we recommend that you increase the log storage space. Otherwise, you cannot store additional logs and data loss may occur.


After the Log Service collects Anti-DDoS Origin logs, you can query, analyze, download, ship and transform log, or configure alerts for the logs. For more information, see [Common operations on logs of Alibaba Cloud services](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md)

