# Obtain data from an ApsaraDB RDS for MySQL database over the internal network

If your data is separately stored in Log Service and ApsaraDB RDS for MySQL, you can use the data transformation feature of Log Service to obtain data from ApsaraDB RDS for MySQL. This topic describes how to configure data transformation rules to obtain data from an ApsaraDB RDS for MySQL database over the internal network.

The dynamic data of shared bikes in Shanghai such as order number, user ID, geographical location, and bicycle riding behavior generated in August 2019 is stored in a Logstore of Log Service. The static data such as bicycle number, brand, and batch number of the shared bikes is stored in an ApsaraDB RDS for MySQL database. The supplier of the shared bikes wants to use the static data of the shared bikes to enrich the dynamic data and achieve optimal scheduling.

The shared-bike supplier can access the RDS database over the Internet. However, this is not secure and stable. Log Service provides the VPC reverse proxy feature. You can use this feature to access the RDS database in a fast and secure manner.

**Note:**

-   The RDS database and the Log Service project must reside in the same region. Otherwise, data cannot be obtained from the database.
-   The RDS database and the Log Service project can belong to different accounts.
-   Before you access the RDS database over the internal network, you must specify a whitelist of Classless Inter-Domain Routing \(CIDR\) blocks for the database. In this example, the CIDR block is 100.104.0.0/16. For more information, see [Control access to an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Control access to an ApsaraDB RDS for MySQL instance.md).

## Data transformation

-   Raw data
    -   The following figure shows sample data entries in the RDS database table.

        ![RDS data](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8804813061/p135927.png)

    -   The following figure shows a sample log entry in the Logstore of Log Service.

        ![Log](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8804813061/p135928.png)

-   Transformation rule

    ```
    e_table_map(res_rds_mysql(str_format("{}:{}",res_local("config.vpc.instance_id.test1"),res_local("config.vpc.instance_port.test1")), "your rds username", "your rds password", "your database",table="your table",primary_keys="bikeid"), "bikeid",["brand","batch"])
    ```

    **Note:** You must use the following transformation syntax to access the RDS database over the internal network.

    Syntax

    ```
    e_table_map(res_rds_mysql(str_format("config.vpc.instance_id.name: USERNAME}",res_local("config.vpc.instance_port.name"),res_local("")), "database account", "database password", "database name",table="database table name"), "field", "output_fields")
    ```

    -   You must specify the config.vpc.instance\_id.name and config.vpc.instance\_port.name parameters in the Advanced Parameter Settings field in the Log Service console. The **name** variable in the config.vpc.instance\_id.name parameter and the name variable in the config.vpc.instance\_port.name parameter must be the same. You can customize the variable value.
    -   The field parameter specifies the field that you want to map. This field exists in the Logstore and RDS database. If the value of the field in the Logstore and RDS database is not the same, the data mapping fails.
    -   The output\_fields parameter specifies the output fields. If the data mapping succeeds, a new log entry is generated.
-   Advanced parameter settings

    You must set the **Advanced Parameter Settings** parameter when you configure the preview mode and configure a transformation rule. For more information about how to configure other parameters, see [Create a data transformation rule](/intl.en-US/Data Transformation/Create a data transformation rule.md).

    ![Add preview settings](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2390380061/p100061.png)

    The name variable in the config.vpc.vpc\_id.name, config.vpc.instance\_id.name and config.vpc.instance\_port.name parameters must be the same as the name variable in the transformation rule. You can customize the variable value.

    |Key format|Key example|Value example|Description|
    |----------|-----------|-------------|-----------|
    |config.vpc.vpc\_id.name|config.vpc.vpc\_id.test1|vpc-uf6mskb0b\*\*\*\*n9yj|The vpc\_id variable specifies the ID of the VPC where the RDS database resides.|
    |config.vpc.instance\_id.name|config.vpc.instance\_id.test1|rm-uf6e61k\*\*\*\*ahd7|The instance\_id variable specifies the ID of the RDS instance to which the RDS database belongs.|
    |config.vpc.instance\_port.name|config.vpc.instance\_port.test1|3306|The instance\_port variable specifies the internal port of the RDS instance.|


## Troubleshooting

-   A whitelist of IP addresses is not configured for the RDS instance.

    If the `reason: {"errorCode": "InvalidConfig", "errorMessage": "error when calling : res_rds_mysql\nDetail: {\"errorCode\": \"InvalidConfig\", \"errorMessage\": \"Database connection failed, cause: (2003, \\\"Can't connect to MySQL server on '192.168.1.1' (timed out)\\\")\\nDetail: None\", \"requestId\": \"\"}", "requestId": ""}` is prompted, Log Service is authorized to access the VPC where the RDS database resides. However, a whitelist of IP addresses is not configured for the RDS database. Therefore, Log Service cannot connect to the RDS database.

-   Advanced parameters are invalid.
    -   The name variable in the advanced parameters is inconsistent.

        If the `reason: {"errorCode": "InvalidConfig", "errorMessage": "error when calling : res_rds_mysql\nDetail: {\"errorCode\": \"InvalidConfig\", \"errorMessage\": \"address check failed, please check the configuration of address. address: rm-bp***r5\\nDetail: None\", \"requestId\": \"\"}", "requestId": ""}` is prompted, the name variable in the config.vpc.instance\_port.\{name\}, config.vpc.vpc\_id.\{name\}, and config.vpc.instance\_id.\{name\} parameters is inconsistent.

    -   Some advanced parameters are not configured.

        If the `reason: {"errorCode": "InvalidConfig", "errorMessage": "error when calling : res_rds_mysql\nDetail: {\"errorCode\": \"InvalidConfig\", \"errorMessage\": \"address check failed, please check the configuration of address. address: rm-bp1***9r5\\nDetail: None\", \"requestId\": \"\"}", "requestId": ""}` is prompted, the config.vpc.vpc\_id.\{name\} parameter is not configured.

-   The syntax of the transformation rule is invalid.

    If the `reason: {"errorCode": "InvalidConfig", "errorMessage": "error when calling : res_rds_mysql\nDetail: {\"errorCode\": \"InvalidConfig\", \"errorMessage\": \"Database connection failed, cause: (2003, \\\"Can't connect to MySQL server on 'rm-bp***r5.mysql.rds.aliyuncs.com' (timed out)\\\")\\nDetail: None\", \"requestId\": \"\"}", "requestId": ""}` is prompted, the syntax of the transformation rule is invalid.


