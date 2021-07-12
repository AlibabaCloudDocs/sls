# Enable the global acceleration feature

This topic describes how to enable the global acceleration feature of Log Service in Log Service console.

## Procedure

1.  Submit a [ticket](https://workorder-intl.console.aliyun.com/console.htm) to obtain a canonical name \(CNAME\).

2.  Enable the global acceleration feature.

    1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

    2.  In the project list, click the project for which you want to enable the global acceleration feature.

    3.  On the Overview page, click **Modify** next to **Global Acceleration**.

    4.  In the Global Acceleration panel, enter the **CNAME**, and then click **Enable Acceleration**.

        ![Enable the global acceleration feature](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6693525061/p8065.png)


## What to do next

After you enable the global acceleration feature, you can configure collection acceleration.

-   Use Logtail to collect logs.
    -   If you enable the global acceleration feature before you install Logtail, select **Global Acceleration** when you install Logtail. For more information, see [Install Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md).
    -   If you enable the global acceleration feature after you install Logtail, switch the collection mode to **Global Acceleration**. For more information, see [Configure log collection acceleration for Logtail](/intl.en-US/Data Collection/Collection acceleration/Configure log collection acceleration for Logtail.md).
-   Use an SDK to collect logs.

    If you use an SDK to collect logs, you can replace the specified endpoint of the Log Service project with `log-global.aliyuncs.com`.


