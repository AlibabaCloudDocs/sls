# Collect RDS SQL audit logs

This topic describes how to collect RDS SQL audit logs by using the Log Service console.

-   An ApsaraDB RDS instance is created.If an ApsaraDB RDS for MySQL instance is created, the SQL Explorer feature of a paid edition is enabled for the instance. For more information, see [Create an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Create an ApsaraDB RDS for MySQL instance.md) and [Use the SQL Explorer feature on an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Audit/Use the SQL Explorer feature on an ApsaraDB RDS for MySQL instance.md).
-   A Log Service project and Logstore are created in the region where the RDS instance resides. For more information, see [Create a project and a Logstore](/intl.en-US/.md).

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, select **RDS SQL Audit - Cloud Products**.

3.  In the Specify Logstore step, select a project and Logstore, and click **Next**.

4.  In the Specify Data Source step, complete RAM authorization, enable the data shipping feature, and then click **Next**.

    **Note:**

    -   If you have not authorized Log Service to ship logs, click **Authorize** next to **RAM**, and complete the authorization as prompted.
    -   The destination ApsaraDB RDS instance may not appear on the prompted page or you may fail to enable the data shipping feature. This issue occurs when your ApsaraDB RDS instance does not meet the required conditions. For more information about how to check whether your ApsaraDB RDS instance meets the required conditions, see the Prerequisites section.
    ![Specify Data Source tab](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7505723061/p47510.png)

5.  In the Configure Query and Analysis step, click **Next**.

    By default, the indexing feature is enabled for the Logstore where RDS SQL audit logs are stored. Indexes are configured for these audit logs. You can modify indexes as needed. For more information, see [Configure indexes](/intl.en-US/Index and query/Configure indexes.md).


After RDS SQL audit logs are collected by Log Service, you can search, analyze, download, ship, and transform these logs. You can also configure alerts for these logs. For more information, see [Common operations on logs of Alibaba Cloud services](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).

