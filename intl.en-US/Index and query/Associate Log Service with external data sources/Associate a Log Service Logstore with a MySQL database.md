# Associate a Log Service Logstore with a MySQL database

This topic describes how to create an external store and associate a Log Service Logstore with a MySQL database.

-   A project and a Logstore are created. For more information, see [Step 2: Create a project and a Logstore](/intl.en-US/.md).
-   Log data is collected. For more information, see [Log collection method](/intl.en-US/Data Collection/Log collection methods.md).

The external storage feature of Log Service allows you to associate a Log Service Logstore with an on-premises MySQL database or an ApsaraDB RDS for MySQL database. Then, you can write query results to the MySQL database for further processing. For information about the best practices to create MySQL databases for external storage, see [Perform an association query and analysis on the data of a Logstore and a MySQL database](/intl.en-US/Index and query/Best practices/Perform an association query and analysis on the data of a Logstore and a MySQL database.md).

## Procedure

1.  Create an ApsaraDB RDS for MySQL instance in a virtual private cloud \(VPC\). For more information, see [Create an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Create an ApsaraDB RDS for MySQL instance.md).

    In this example, an ApsaraDB RDS for MySQL instance in a VPC is used to describe how to create an external store for a MySQL table.

2.  Configure a whitelist for the ApsaraDB RDS for MySQL instance.

    Configure a whitelist to authorize external devices to access the ApsaraDB RDS for MySQL instance. Add the following CIDR blocks to the whitelist: 100.104.0.0/16, 11.194.0.0/16, and 11.201.0.0/16. For more information, see [Configure an IP address whitelist for an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Data security/Set the whitelist/Configure an IP address whitelist for an ApsaraDB RDS for MySQL instance.md).

3.  Create a table in the ApsaraDB RDS for MySQL instance. For more information, see [CREATE TABLE Statement](https://dev.mysql.com/doc/refman/8.0/en/create-table.html).

4.  Create an external store.

    1.  Install the command-line interface \(CLI\) of Log Service. For more information, see [Command line interface](/intl.en-US/Developer Guide/Command line interface.md).

    2.  Create a configuration file named /root/config.json.

    3.  Add the following script to the /root/config.json file. Replace the parameter values with the actual values.

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
        |region|The region where the RDS instance resides.        -   If your database is deployed on an ApsaraDB RDS for MySQL instance, you must specify the region parameter.
        -   If your database is a self-managed MySQL database, you must set the value of the region parameter to an empty string in the "region": "" format. |
        |vpc-id|VPC ID        -   If your database is deployed on an ApsaraDB RDS for MySQL instance in a VPC, you must specify the vpc-id parameter.
        -   If your database is deployed on an ApsaraDB RDS for MySQL instance in the classic network, you must set the value of the vpc-id parameter to an empty string. If your database is a self-managed MySQL database in the classic network, you must also set this parameter to an empty string. The empty string must be in the "vpc-id": "" format. |
        |instance-id|The ID of the RDS instance.        -   If your database is deployed on an ApsaraDB RDS for MySQL instance, you must specify the instance-id parameter.
        -   If your database is a self-managed MySQL database, you must set the value of the instance-id parameter to an empty string in the "instance-id": "" format. |
        |host|The address of the database.|
        |port|The port of the RDS instance.|
        |username|The username of the database.|
        |password|The password of the database.|
        |db|The name of the database.|
        |table|The name of the table.|

    4.  Run the following code to create an external store.

        Replace the project\_name parameter with the actual name of the project.

        ```
        aliyunlog log create_external_store --project_name="log-rds-demo" --config="file:///root/config.json" 
        ```


## What to do next

-   Update the external store.

    ```
    aliyunlog log update_external_store --project_name="log-rds-demo" --config="file:///root/config.json" 
    ```

-   Delete the external store.

    ```
    aliyunlog log delete_external_store --project_name="log-rds-demo" --store_name=abc
    ```


[Use a JOIN statement to query data from a Logstore and a MySQL database](/intl.en-US/Index and query/Analysis grammar/Use a JOIN statement to query data from a Logstore and a MySQL database.md)

