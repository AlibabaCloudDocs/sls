# Enable the full log feature

This topic describes how to enable the full log feature in the anti-DDoS Pro \(old\) console. This feature allows you to collect logs of Anti-DDoS Pro and send the logs to Log Service.

-   An Anti-DDoS Pro instance is created. A domain is added to the instance.

    Anti-DDoS Pro has been upgraded to Anti-DDoS Pro \(BGP\). The Anti-DDoS Pro \(old\) console is available only for users who have purchased Anti-DDoS Pro instances. You can no longer create Anti-DDoS Pro instances in the Anti-DDoS Pro \(old\) console.

-   Log Service is activated.

1.  Log on to the [Anti-DDoS Pro \(old\) console](https://yundun.console.aliyun.com/?spm=5176.11750983.103.1.752d2aafZZS4k4&p=ddospronext#/statistic/layer7?region=cn-hangzhou).

2.  In the left-side navigation pane, choose **Log** \> **Full Log**.

3.  On the Full Log page, authorize Anti-DDoS Pro to use the AliyunDDoSCOOLogArchiveRole role to access Log Service.

    **Note:**

    -   This operation is required only when you enable the full log feature for the first time.
    -   If you use a RAM user to log on to the Anti-DDoS Pro console, you must grant the required permissions to the RAM user.
    -   To ensure that logs can be pushed to Log Service, do not revoke permissions from the RAM role or delete the RAM role.
4.  Select the website domain that you want to enable the full log feature, turn on the **Status** switch.

    ![Enable the full log feature](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8493723061/p6786.jpg)


After the logs of Anti-DDoS Pro are pushed to Log Service, you can query, analyze, download, ship, and transform the collected logs. You can also configure alerts for the logs. For more information, see [Common operations on logs of Alibaba Cloud services](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).

