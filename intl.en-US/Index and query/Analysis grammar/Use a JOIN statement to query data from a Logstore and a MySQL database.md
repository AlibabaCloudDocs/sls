# Use a JOIN statement to query data from a Logstore and a MySQL database

This topic describes how to query data from a Logstore and a MySQL database at the same time by using a JOIN statement. This topic also describes how to store the query results to the MySQL database.

-   Logs are collected to a Logstore. For more information, see [Log collection methods](/intl.en-US/Data Collection/Log collection methods.md).
-   Indexes are created for the log fields. For more information, see [Enable and configure the index feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).
-   A MySQL database is available.

    For more information about ApsaraDB RDS for MySQL databases, see [Create accounts and databases for an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Create accounts and databases for an ApsaraDB RDS for MySQL instance.md).


1.  Configure a whitelist for a MySQL database.

    -   If you use an ApsaraDB RDS for MySQL database, add the following CIDR blocks to the whitelist: 100.104.0.0/16, 11.194.0.0/16, and 11.201.0.0/16. For more information, see [Configure a whitelist for an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Data security/Configure a whitelist for an ApsaraDB RDS for MySQL instance.md).
    -   If you use a user-created MySQL database, add the following CIDR blocks to the security group: 100.104.0.0/16, 11.194.0.0/16, and 11.201.0.0/16.
2.  Create a user property table in the MySQL database.

3.  Create an ExternalStore.

    1.  Install the Log Service command line interface \(CLI\). For more information, see [Command line interface](/intl.en-US/Developer Guide/Command line interface.md).

    2.  Create a configuration file named /root/config.json and add the following script to the file.

        Replace the value of each parameter as needed.

        ```
        {
        "externalStoreName":"storeName",
        "storeType":"rds-vpc",
        "parameter":
           {
           "region":"cn-qingdao",
           "vpc-id":"vpc-m5eq4irc1pucp*******",
           "instance-id":"i-m5eeo2whsn*******",
           "host":"localhost",
           "port":"3306",
           "username":"root",
           "password":"****",
           "db":"scmc",
           "table":"join_meta"
           }
        }
        ```

        |Parameter|Description|
        |:--------|:----------|
        |region|The region where the RDS instance resides.        -   If you use an ApsaraDB RDS for MySQL database, you must specify the region parameter.
        -   If you use a user-created MySQL database, you must set the value of the region parameter to an empty string in the "region": "" format. |
        |vpc-id|VPC ID        -   If you use an ApsaraDB RDS for MySQL database in a VPC, you must specify the vpc-id parameter.
        -   If you use an ApsaraDB RDS for MySQL database or a user-created MySQL database in the classic network, you must set the value of the vpc-id parameter to an empty string in the "vpc-id": "" format. |
        |instance-id|The ID of the RDS instance.        -   If you use an ApsaraDB RDS for MySQL database, you must specify the instance-id parameter.
        -   If you use a user-created MySQL database, you must set the value of the instance-id parameter to an empty string in the "instance-id": "" format. |
        |host|The address of the database.|
        |port|The port number of the database.|
        |username|The username of the database.|
        |password|The password of the database.|
        |db|The name of the database.|
        |table|The name of the database table.|

    3.  Create an ExternalStore.

        Replace the value of the project\_name parameter with the actual project name.

        ```
        aliyunlog log create_external_store --project_name="log-rds-demo" --config="file:///root/config.json" 
        ```

4.  Use a JOIN statement to query logs.

    1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

    2.  In the Projects section, click the name of the project.

    3.  Choose **Log Management** \> **Logstores**, and then click the Logstore.

    4.  Execute a query statement.

        You can use the INNER JOIN, LEFT JOIN, RIGHT JOIN, or FULL JOIN syntax.

        ```
        [ INNER ] JOIN
        LEFT [ OUTER ] JOIN
        RIGHT [ OUTER ] JOIN
        FULL [ OUTER ] JOIN
        ```

        The following example shows a JOIN statement:

        ```
        method:postlogstorelogs | select count(1) , histogram(logstore) from log  l join join_meta m on l.projectid = cast( m.ikey as varchar)
        ```

        **Note:**

        -   JOIN statements apply only to MySQL database tables and Logstores. The data size of the table must be less than 20 MB
        -   In a query statement, the Logstore name must be written before the join parameter, and the ExternalStore name must be written after the join parameter.
        -   In a query statement, the name of the ExternalStore must be specified. The system replaces the name with the combination of the MySQL database name and the table name. Do not enter only the name of the MySQL database table.
5.  Save the query result to the MySQL database.

    Log Service allows you to insert query results into a MySQL database by using an INSERT statement. The following example shows an INSERT statement:

    ```
    method:postlogstorelogs | insert into method_output  select cast(method as varchar(65535)),count(1) from log group by method
    ```


Sample Python script:

```
# encoding: utf-8
from __future__ import print_function
from aliyun.log import *
from aliyun.log.util import base64_encodestring
from random import randint
import time
import os
from datetime import datetime
    endpoint = os.environ.get('ALIYUN_LOG_SAMPLE_ENDPOINT', 'cn-chengdu.log.aliyuncs.com')
    accessKeyId = os.environ.get('ALIYUN_LOG_SAMPLE_ACCESSID', '')
    accessKey = os.environ.get('ALIYUN_LOG_SAMPLE_ACCESSKEY', '')
    logstore = os.environ.get('ALIYUN_LOG_SAMPLE_LOGSTORE', '')
    project = "ali-yunlei-chengdu"
    client = LogClient(endpoint, accessKeyId, accessKey, token)
    # Create an ExternalStore.
    res = client.create_external_store(project,ExternalStoreConfig("rds_store","region","rds-vpc","vpc id","instance id","instance ip","instance port","username","password","database","table"));
    res.log_print()
    # Retrieve the details of the ExternalStore.
    res = client.get_external_store(project,"rds_store");
    res.log_print()
    res = client.list_external_store(project,"");
    res.log_print();
    # Execute the JOIN statement.
    req = GetLogsRequest(project,logstore,From,To,"","select count(1) from  "+ logstore +"  s join  meta m on  s.projectid = cast(m.ikey as varchar)");
    res = client.get_logs(req)
    res.log_print();
     # Save the query results to the MySQL database.
    req = GetLogsRequest(project,logstore,From,To,""," insert into rds_store select count(1) from  "+ logstore );
    res = client.get_logs(req)
    res.log_print();
```

