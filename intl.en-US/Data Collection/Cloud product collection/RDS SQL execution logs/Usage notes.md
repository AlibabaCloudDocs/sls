# Usage notes

Log Service allows you to import data from other cloud resources. After you enable the SQL Explorer feature in the ApsaraDB RDS console, you can import SQL audit logs to Log Service. Then, you can query, ship, and transform the logs in real time in the Log Service console. You can also visualize log analysis results and configure alerts for the logs. This topic describes the assets, billing, and limits that are related to RDS SQL audit logs.

## RDS SQL audit logs

The RDS SQL audit logs record all the operations that are performed on the ApsaraDB RDS database. The database collects SQL audit logs based on network listening, which consumes only a few CPU resources of the system. This does not affect the execution of SQL statements. RDS SQL audit logs include but are not limited to the following types of operation data:

-   Database logon and logoff.
-   Data definition language \(DDL\): SQL statements that define the database structure, such as CREATE, ALTER DROP, TRUNCATE, and COMMENT.
-   Data manipulation language \(DML\): SQL statements that perform operations, such as SELECT, INSERT, UPDATE, and DELETE.
-   Other operations that are performed by executing SQL statements, such as rollback and control.
-   SQL execution latency, execution results, and the number of affected rows.

## Assets

-   Custom projects and Logstores

    **Note:** To ensure that logs can be collected to Log Service, we recommend that you do not delete the projects or Logstores that are associated with RDS SQL audit logs.

-   Dedicated dashboards

    After you enable the SQL Explorer feature, Log Service generates three dashboards by default.

    **Note:** We recommend that you do not make changes to the dedicated dashboards because this may affect the usability of the dashboards. You can create a custom dashboard to visualize log analysis results. For more information, see [Create a dashboard](/intl.en-US/Visualization/Dashboard/Create a dashboard.md).

    |Dashboard|Equivalent|
    |---------|----------|
    |RDS Operation Center|Displays the access statistics of active databases. The statistics include the number of databases, tables, and execution errors. The statistics also include the total number of inserted rows, updated rows, deleted rows, and queried rows.|
    |RDS Performance Center|Displays the performance metrics that are related to O&M reliability. These metrics include the peak SQL execution, peak query bandwidth, peak insertion bandwidth, peak update bandwidth, peak deletion bandwidth, average SQL execution time, average SQL query time, average SQL update time, and average SQL deletion time.|
    |RDS Security Center|Displays the security metrics of the ApsaraDB RDS databases. These metrics include the number of errors, logon failures, major deletion events, major modification events, and risky SQL execution. The metrics also include the distribution of execution error types, distribution of external clients that have errors, and the clients that have the largest number of errors.|


## Billing

-   After you enable the SQL Explorer feature for ApsaraDB RDS for MySQL instances, you are billed for the used instances. Billing is based on the hourly usage \(hourly fee = Amount of consumed audit logs x Unit price\).

    **Note:** The SQL Explorer feature is provided free of charge for the RDS Enterprise Edition.

-   After logs are shipped to Log Service, you are charged for the storage space that the log data occupies and the number of read and write operations. You are also charged when you read, transform, and ship the data. For more information, see [Pay-as-you-go](/intl.en-US/Pricing/Pay-as-you-go.md).

## Limits

-   You can import SQL audit logs from only the following types of RDS instances to Log Service:

    ApsaraDB RDS for MySQL instances: All available versions except the basic version are supported.

-   You can import SQL audit logs to Log Service only after you enable the SQL Explorer feature for the RDS instances.
-   The Log Service project that stores RDS SQL audit logs must reside in the same region as the RDS instance.
-   You can import SQL audit logs from only the RDS instances that reside in the China \(Qingdao\), China \(Beijing\), China \(Hangzhou\), China \(Shanghai\), China \(Shenzhen\), and China \(Hong Kong\) regions.

    You can also use the Log Audit Service application of Log Service to collect RDS SQL audit logs. In addition, the Log Audit Service application can collect audit logs from more regions. For more information, see [Log Audit Service](/intl.en-US/Application/Log Audit Service/Background information.md).


