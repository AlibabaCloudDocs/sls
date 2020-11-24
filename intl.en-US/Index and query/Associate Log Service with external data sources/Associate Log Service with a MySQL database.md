# Associate Log Service with a MySQL database

This topic describes how to create an external store and associate Log Service with a MySQL database.

-   A project and a Logstore are created. For more information, see [Step 1: Create a project and a Logstore](/intl.en-US/Quick Start/Quick start.md).
-   Log data is collected. For more information, see [Data collection](/intl.en-US/Data Collection/Log collection methods.md).

The external storage feature of Log Service allows you to associate Log Service with an on-premises MySQL database or an ApsaraDB RDS for MySQL database. Then you can write query results to the MySQL database for further processing. For more information about how to create an external store for a MySQL table, see [Perform an association query and analysis on the data of a Logstore and a MySQL database](/intl.en-US/Index and query/Best practices/Perform an association query and analysis on the data of a Logstore and a MySQL database.md).

## Procedure

1.  Create an ApsaraDB RDS for MySQL instance in a virtual private cloud \(VPC\). For more information, see [Create an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Create an ApsaraDB RDS for MySQL instance.md).

    This section takes an ApsaraDB RDS for MySQL instance in a VPC as an example to describe how to create an external store for a MySQL table.

2.  Configure a whitelist for the RDS instance.

    Configure a whitelist that authorizes external devices to access the RDS instance. Add one of the following CIDR blocks to the whitelist: 100.104.0.0/16, 11.194.0.0/16 or 11.201.0.0/16. For more information, see [Configure a whitelist for an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Data security/Configure a whitelist for an ApsaraDB RDS for MySQL instance.md).

3.  Create an RDS table. For more information, see [CREATE TABLE Statement](https://dev.mysql.com/doc/refman/8.0/en/create-table.html).

4.  Create an external store.

    1.  Install the command-line interface \(CLI\) of Log Service. For more information, see [Command line interface](/intl.en-US/Developer Guide/Command line interface.md).

    2.  Create a configuration file named /root/config.json.

    3.  Add the following script to the /root/config.json file. Replace the parameter values based on your business requirements.

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
        |region|The region where the RDS instance is located.|
        |vpc-id|The ID of the VPC.        -   If the RDS instance resides in a VPC, you must set this parameter.
        -   If the RDS instance resides in the classic network or the MySQL database is an on-premises MySQL database, the value of this parameter is an empty string. |
        |instance-id|The ID of the RDS instance.        -   If you use a RDS instance, you must set this parameter.
        -   If you use an on-premises MySQL database, the value of this parameter is an empty string. |
        |host|The domain name of the RDS instance.|
        |port|The port of the RDS instance.|
        |username|The username of the RDS instance.|
        |password|The password of the RDS instance.|
        |db|The name of the RDS database.|
        |table|The name of the RDS table.|

    4.  Run the following code to create an external store.

        Replace the project\_name parameter with the actual name of the project.

        ```
        aliyunlog log create_external_store --project_name="log-rds-demo" --config="file:///root/config.json" 
        ```


## Related operations

-   Update the external store.

    ```
    aliyunlog log update_external_store --project_name="log-rds-demo" --config="file:///root/config.json" 
    ```

-   Delete the external store.

    ```
    aliyunlog log delete_external_store --project_name="log-rds-demo" --store_name=abc
    ```


[Use a JOIN statement to query data from a Logstore and a MySQL database](/intl.en-US/Index and query/Analysis grammar/Use a JOIN statement to query data from a Logstore and a MySQL database.md)

