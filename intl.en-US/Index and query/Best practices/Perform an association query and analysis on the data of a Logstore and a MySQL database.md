# Perform an association query and analysis on the data of a Logstore and a MySQL database

This topic describes how to perform an association query and analysis on the data stored in a Logstore and a MySQL database. The logs of a gaming company are used as an example.

-   Logs are collected to the Logstore. For more information, see [Log collection methods](/intl.en-US/Data Collection/Log collection methods.md).
-   Indexes are configured for the log fields. For more information, see [Enable and configure the indexing feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).
-   A MySQL database is available. For more information, see [Create accounts and databases for an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Create accounts and databases for an ApsaraDB RDS for MySQL instance.md).

Company A is a gaming company that has two types of data: user game logs and user metadata. Log Service can collect user game logs in real time. A user game log contains properties such as operation, targets, health points \(HP\), magic points \(MP\), network, payment method, click location, status code, and user ID. User metadata contains the user information such as the gender, registration time, and region. In most cases, user metadata is stored in a database because metadata cannot be printed in logs. Company A wants to analyze the user game logs and user metadata.

The query and analysis engine of Log Service can perform an association query and analysis on the data stored in Logstores and external stores. External stores include MySQL databases and Object Storage Service \(OSS\) buckets. To analyze the metrics related to user properties, you can use the SQL JOIN syntax to associate the user game logs with the user metadata. You can also write analysis results to external stores to further process the results.

![Background information](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9784688951/p41587.png)

1.  Create a user property table in the MySQL database.

    Create a table named chiji\_user to store user properties. The user properties include the user ID, username, gender, age, account balance, registration time, and registration region.

    ```
    CREATE TABLE `chiji_user` ( 
      `uid` int(11) NOT NULL DEFAULT '0', 
      `user_nick` text, 
      `gender` tinyint(1) DEFAULT NULL, 
      `age` int(11) DEFAULT NULL, 
      `register_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, 
      `balance` float DEFAULT NULL, 
      `region` text, PRIMARY KEY (`uid`) 
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8
    ```

2.  Add IP addresses to the whitelist.

    -   If you use an ApsaraDB RDS for MySQL database, add the following CIDR blocks to the whitelist: 100.104.0.0/16, 11.194.0.0/16, and 11.201.0.0/16. For more information, see [Configure an IP address whitelist for an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Data security/Set the whitelist/Configure an IP address whitelist for an ApsaraDB RDS for MySQL instance.md).
    -   If you use a self-managed MySQL database, add the following CIDR blocks to the security group: 100.104.0.0/16, 11.194.0.0/16, and 11.201.0.0/16.
3.  Create an external store.

    1.  Install the Log Service command line interface \(CLI\). For more information, see [Command line interface](/intl.en-US/Developer Guide/Command line interface.md).

    2.  Create a configuration file named /root/config.json.

    3.  Add the following script to the /root/config.json file. Replace the parameter values based on your business requirements.

        ```
        { 
             "externalStoreName": "chiji_user", 
             "storeType": "rds-vpc", 
             "parameter": { 
             "vpc-id": "vpc-m5eq4irc1pucpk85f****", 
             "instance-id": "rm-m5ep2z57814qs****", 
             "host": "example.com", 
             "port": "3306", 
             "username": "testroot", 
             "password": "****", 
             "db": "chiji", 
             "table": "chiji_user", 
             "region": "cn-qingdao" 
          } 
        }
        ```

        |Parameter|Description|
        |:--------|:----------|
        |region|The region where the RDS instance resides.        -   If you use an ApsaraDB RDS for MySQL database, you must set the region parameter.
        -   If you use a self-managed MySQL database, you must set the region parameter to an empty string in the "region": "" format. |
        |vpc-id|VPC ID        -   If you use an ApsaraDB RDS for MySQL database in a virtual private cloud \(VPC\), you must set the vpc-id parameter.
        -   If you use an ApsaraDB RDS for MySQL database or a self-managed MySQL database in the classic network, you must set the vpc-id parameter to an empty string in the "vpc-id": "" format. |
        |instance-id|The ID of the RDS instance.        -   If you use an ApsaraDB RDS for MySQL database, you must set the instance-id parameter.
        -   If you use a self-managed MySQL database, you must set the instance-id parameter to an empty string in the "instance-id": "" format. |
        |host|The address of the database.|
        |port|The port number of the database.|
        |username|The username of the database.|
        |password|The password of the database.|
        |db|The name of the database.|
        |table|The table of the database.|

    4.  Create an external store.

        Run the following code. Replace the project\_name parameter based on the actual scenario.

        ```
        aliyunlog log create_external_store --project_name="log-rds-demo" --config="file:///root/config.json" 
        ```

4.  Use the SQL JOIN syntax to perform an association query and analysis.

    1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

    2.  In the Projects section, click the project in which you want to query and analyze logs.

    3.  On the **Log Storage** \> **Logstores** tab, click the Logstore where logs are stored.

    4.  Execute a query statement.

        To associate the Logstore with the MySQL database, specify the userid field in Log Service and the uid field in the database table.

        -   Analyze the gender distribution of active users.

            ```
            * | select case gender when 1 then 'male' else 'female' end as gender , count(1) as pv from log l join chiji_user u on l.userid = u.uid group by gender order by pv desc
            ```

            ![Association analysis](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1812470061/p41594.png)

        -   Analyze the activity level of users in different regions.

            ```
            * | select region , count(1) as pv from log l join chiji_user u on l.userid = u.uid group by region order by pv desc
            ```

            ![Activity level](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1812470061/p41596.png)

        -   Analyze the consumption trends among users of different genders.

            ```
            * | select case gender when 1 then 'male' else 'female' end as gender , sum(money) as money from log l join chiji_user u on l.userid = u.uid group by gender order by money desc
            ```

5.  Save the query and analysis result to the MySQL database.

    1.  To store the number of page views \(PVs\) per minute, create a data table named report in the MySQL database.

        ```
        CREATE TABLE `report` ( 
          `minute` bigint(20) DEFAULT NULL, 
          `pv` bigint(20) DEFAULT NULL 
        ) ENGINE=InnoDB DEFAULT CHARSET=utf8
        ```

    2.  Create an external store for the report table. For more information, see [Step 3](#step_oi6_fge_vyf).

    3.  On the Search & Analysis page of the Logstore, execute the following query statement to save the analysis result to the report table:

        ```
        * | insert into report select __time__- __time__ % 300 as min, count(1) as pv group by min
        ```

        After the result is saved, you can view the result in the MySQL database.

        ![SQL result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9784688951/p41597.png)


