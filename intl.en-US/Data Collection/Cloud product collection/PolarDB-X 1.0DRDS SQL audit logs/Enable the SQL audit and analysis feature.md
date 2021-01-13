# Enable the SQL audit and analysis feature

This topic describes how to enable the SQL audit and analysis feature in the Distributed Relational Database Service \(DRDS\) console.

-   Log Service is activated.
-   A DRDS instance is purchased. For more information, see [Step 1: Buy a DRDS instance]().
-   A database is created. For more information, see [Step 2: Create a DRDS database]().

1.  Log on to the [DRDS console](https://polardb-x.console.aliyun.com/).

2.  In the upper-left corner of the page, select the region where the instance resides.

3.  On the Instance list name page, click the name of the instance.

4.  In the left-side navigation pane, choose **Diagnostics and Optimization** \> **SQL Audit and Analysis**.

5.  Authorize DRDS as prompted to assume the AliyunDRDSDefaultRole role to access Log Service.

    **Note:**

    -   This operation is required only when you enable the SQL audit and analysis feature for the first time. You must complete the authorization by using your Alibaba Cloud account.
    -   If you use a RAM user to log on to DRDS, you must grant required permissions to the RAM user. For more information, see [RAM user authorization](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).
    -   To ensure that DRDS SQL audit logs can be sent to Log Service, do not delete the RAM role.
6.  Enable the SQL audit and analysis feature.

    1.  Select the database, and then turn on the switch next to the database or turn on the SQL Audit Log Status of Current Database switch.

        ![Enable the SQL audit and analysis feature](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7177050161/p21186.png)

    2.  Import historical data.

        By default, only the logs that are generated after the SQL audit and analysis feature is enabled are sent to Log Service. If you want to analyze logs that are generated before the SQL audit and analysis feature is enabled, you can import historical data. To do so, you can turn on the **Import Historical Data or Not** switch in the Log Storage Period dialog box. Then, set the **Trace Start Time** and **Trace End Time** parameters. You can import up to seven days of historical data.

        You can view the log import progress in the Task Progress dialog box in the upper-right corner of the current page.

        ![Import historical data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7177050161/p21243.png)

    3.  Click **Enable**.


After DRDS SQL audit logs are collected and sent to Log Service, you can query, analyze, download, ship, and transform the collected logs. You can also configure alerts for the logs. For more information, see [Common operations on logs of Alibaba Cloud services](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).

