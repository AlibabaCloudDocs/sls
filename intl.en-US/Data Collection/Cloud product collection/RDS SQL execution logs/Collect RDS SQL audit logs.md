# Collect RDS SQL audit logs

This topic describes how to collect RDS SQL audit logs by using the Log Service console.

-   An ApsaraDB for RDS instance is created. If an ApsaraDB RDS for SQL Server instance or ApsaraDB RDS for PostgreSQL instance is created, the SQL audit feature is enabled for the instance. If an ApsaraDB RDS for MySQL instance is created, the SQL explorer feature of a paid edition is enabled for the instance.
    -   To create an ApsaraDB RDS for MySQL instance, see the [Create an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Create an ApsaraDB RDS for MySQL instance.md) and [SQL Explorer](/intl.en-US/RDS MySQL Database/Audit/SQL Explorer.md) topics
    -   To create an ApsaraDB RDS for SQL Server instance, see the [Create an ApsaraDB RDS for SQL Server instance](/intl.en-US/RDS SQL Server Database/Quick start/Create an ApsaraDB RDS for SQL Server instance.md) topic.
    -   To create an ApsaraDB RDS for PostgreSQL instance, see the [Create an ApsaraDB RDS for PostgreSQL instance](/intl.en-US/RDS PostgreSQL Database/Quick start/Create an ApsaraDB RDS for PostgreSQL instance.md) and [Enable and disable SQL Audit \(database audit\) on an ApsaraDB RDS for PostgreSQL instance](/intl.en-US/RDS PostgreSQL Database/Audit/Enable and disable SQL Audit (database audit) on an ApsaraDB RDS for PostgreSQL instance.md) topics.
-   A Log Service project and Logstore are created in the region where the ApsaraDB for RDS instance resides. For more information, see [Step 1: Create a project and a Logstore](/intl.en-US/Quick Start/Quick start.md).

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, select **RDS SQL Audit - Cloud Products**.

3.  In the Specify Logstore step, select a project and Logstore, and click **Next**.

4.  In the Specify Data Source step, complete RAM authorization, enable the data shipping feature, and then click **Next**.

    **Note:**

    -   If you have not authorized Log Service to dispatch logs, click **Authorize** next to **RAM**, and complete the authorization as prompted.
    -   The destination ApsaraDB for RDS instance may not appear on the prompted page or you may fail to enable the data shipping feature. This issue occurs when your ApsaraDB for RDS instance does not meet the required conditions. For more information about how to check whether your ApsaraDB for RDS instance meets the required conditions, see the Prerequisites section.
    ![Specify Data Source tab](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7505723061/p47510.png)

5.  In the Configure Query and Analysis step, click **Next**.

    The indexing feature is enabled for the Logstore where RDS SQL audit logs reside and indexes are configured for these audit logs. You can modify indexes as required. For more information, see [Enable and configure the index feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).


After RDS SQL audit logs are collected by using Log Service, you can search, analyze, download, ship, and transform these logs. You can also configure alerts for these logs. For more information, see [Common operations on logs of Alibaba Cloud services](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).

