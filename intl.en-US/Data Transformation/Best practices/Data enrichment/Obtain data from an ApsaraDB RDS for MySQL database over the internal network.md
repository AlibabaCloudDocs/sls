# Obtain data from an ApsaraDB RDS for MySQL database over the internal network

If your data is stored in Log Service and ApsaraDB RDS for MySQL, you can use the data transformation feature of Log Service to obtain data from ApsaraDB RDS for MySQL. This topic describes how to configure data transformation rules and advanced parameters to obtain data from an ApsaraDB RDS for MySQL database over the internal network.

The dynamic data of shared bicycles generated in Shanghai, August 2019 is stored in a Logstore of Log Service. The data includes the order number, bicycle number, user ID, geographical location, and bicycle rider behavior. The static data such as bicycle number, brand, and batch number of the shared bicycles is stored in an ApsaraDB RDS for MySQL database. The supplier of the shared bicycles wants to analyze the static and dynamic data to enrich data and optimize bicycle scheduling.

Accessing the ApsaraDB RDS for MySQL database over a public endpoint may compromise data security and system stability. Log Service provides the VPC reverse proxy feature, which allows you to access the RDS database over the internal network in a fast and secure manner.

**Note:**

-   The RDS database and the Log Service project must reside in the same region. Otherwise, data cannot be obtained from the database.
-   The RDS database and the Log Service project can belong to different accounts.
-   Before you access the RDS database over the internal network, you must specify a whitelist of Classless Inter-Domain Routing \(CIDR\) blocks for the database. In this example, the CIDR block is 100.104.0.0/16. For more information, see [Control access to an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Set the whitelist/Control access to an ApsaraDB RDS for MySQL instance.md).
-   Log Service allows you to access ApsaraDB RDS for MySQL databases over the internal network. Log Service also allows you to access AnalyticDB for MySQL and PolarDB for MySQL databases over the internal network. For more information, see [Access an AnalyticDB for MySQL database or PolarDB for MySQL database over the internal network](#section_gyj_cgp_l9r).

## Configurations

The following figures show how to configure data transformation rules and advanced parameters to obtain data from the ApsaraDB RDS for MySQL database over the internal network.

-   Raw data
    -   The following figure shows sample data records in a table of the ApsaraDB RDS for MySQL database.

        ![RDS data records](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8804813061/p135927.png)

    -   The following figure shows a sample log entry in a Logstore of Log Service.

        ![Log entry](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8804813061/p135928.png)

-   Data transformation procedure
    1.  Enable the data transformation feature in the source Logstore.
    2.  Use the res\_rds\_mysql function to obtain data from the ApsaraDB RDS for MySQL database.
    3.  Save the data transformation result to the destination Logstore.
-   Transformation rule:

    ```
    e_table_map(res_rds_mysql(str_format("{}:{}",res_local("config.vpc.instance_id.test1"),res_local("config.vpc.instance_port.test1")), "your rds username", "your rds password", "your database",table="your table",primary_keys="bikeid"), "bikeid",["brand","batch"])
    ```

    **Note:** You must use the following transformation syntax to access the ApsaraDB RDS for MySQL database over the internal network.

    Transformation syntax:

    ```
    e_table_map(res_rds_mysql(str_format("{}:{}",res_local("config.vpc.instance_id.name"),res_local("config.vpc.instance_port.name")), "database account", "database password", "database name",table="database table name"), "field", "output_fields")
    ```

    -   You must set the same value for the name parameter in config.vpc.instance\_id.name and config.vpc.instance\_port.name, and the name parameter in the **Advanced Parameter Settings** section. You can customize the value.
    -   The field parameter specifies the field that you want to map. This field exists in the Logstore and ApsaraDB RDS for MySQL database. If the value of the field in the Logstore is different from the value of the field in the ApsaraDB RDS for MySQL database, the data mapping fails.
    -   The output\_fields parameter specifies the output fields. If the data mapping succeeds, a new log entry is generated.
-   Advanced parameter settings

    You must set **Advanced Parameter Settings** when you configure the preview mode and a transformation rule. For more information about how to configure other parameters, see [Create a data transformation rule](/intl.en-US/Data Transformation/Create a data transformation rule.md).

    ![Add preview settings](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3621950161/p100061.png)

    You must set the same value for the name parameter in the config.vpc.vpc\_id.name, config.vpc.instance\_id.name, and config.vpc.instance\_port.name parameters. The value must be the same as the name specified in the transformation rule. You can customize the value.

    |Key format|Sample key|Sample value|Description|
    |----------|----------|------------|-----------|
    |config.vpc.vpc\_id.name|config.vpc.vpc\_id.test1|vpc-uf6mskb0b\*\*\*\*n9yj|The vpc\_id parameter specifies the ID of the VPC where the ApsaraDB RDS for MySQL database resides.|
    |config.vpc.instance\_id.name|config.vpc.instance\_id.test1|rm-uf6e61k\*\*\*\*ahd7|The instance\_id parameter specifies the ID of the ApsaraDB RDS for MySQL instance.|
    |config.vpc.instance\_port.name|config.vpc.instance\_port.test1|3306|The instance\_port parameter specifies the internal endpoint of the ApsaraDB RDS for MySQL instance.|


## Troubleshooting

-   A whitelist of IP addresses is not configured for the RDS instance.

    If the `reason: {"errorCode": "InvalidConfig", "errorMessage": "error when calling : res_rds_mysql\nDetail: {\"errorCode\": \"InvalidConfig\", \"errorMessage\": \"Database connection failed, cause: (2003, \\\"Can't connect to MySQL server on '192.168.1.1' (timed out)\\\")\\nDetail: None\", \"requestId\": \"\"}", "requestId": ""}` error occurs, Log Service is authorized to access the VPC where the RDS database resides. However, a whitelist of IP addresses is not configured for the RDS database. Therefore, Log Service cannot connect to the RDS database.

-   Advanced parameters are invalid.
    -   Different names are specified in the advanced parameters.

        If the `reason: {"errorCode": "InvalidConfig", "errorMessage": "error when calling : res_rds_mysql\nDetail: {\"errorCode\": \"InvalidConfig\", \"errorMessage\": \"address check failed, please check the configuration of address. address: rm-bp***r5\\nDetail: None\", \"requestId\": \"\"}", "requestId": ""}` error occurs, the name specified in config.vpc.instance\_port.name parameter is inconsistent with the name specified in the config.vpc.instance\_id.name, and config.vpc.vpc\_id.name parameters.

    -   Some advanced parameters are not specified.

        If the `reason: {"errorCode": "InvalidConfig", "errorMessage": "error when calling : res_rds_mysql\nDetail: {\"errorCode\": \"InvalidConfig\", \"errorMessage\": \"address check failed, please check the configuration of address. address: rm-bp1***9r5\\nDetail: None\", \"requestId\": \"\"}", "requestId": ""}` error occurs, the config.vpc.vpc\_id.name parameter is not specified.

-   The syntax of the transformation rule is invalid.

    If the `reason: {"errorCode": "InvalidConfig", "errorMessage": "error when calling : res_rds_mysql\nDetail: {\"errorCode\": \"InvalidConfig\", \"errorMessage\": \"Database connection failed, cause: (2003, \\\"Can't connect to MySQL server on 'rm-bp***r5.mysql.rds.aliyuncs.com' (timed out)\\\")\\nDetail: None\", \"requestId\": \"\"}", "requestId": ""}` error occurs, the syntax of the transformation rule is invalid.


## Access an AnalyticDB for MySQL database or PolarDB for MySQL database over the internal network

Log Service allows you to access ApsaraDB RDS for MySQL databases over the internal network. Log Service also allows you to access PolarDB for MySQL and AnalyticDB for MySQL databases over the internal network. You must complete the following configurations:

-   PolarDB for MySQL databases

    Before you access a PolarDB for MySQL database over the internal network, you must specify a whitelist of Classless Inter-Domain Routing \(CIDR\) blocks for the database. In this example, the CIDR block is 100.104.0.0/16. For more information, see [Configure a whitelist](/intl.en-US/User Guide/Data Security and Encryption/Configure a whitelist for a cluster.md). For information about the related configurations, see [Access the configurations of ApsaraDB RDS for MySQL](#section_ke7_52c_4wm).

-   AnalyticDB for MySQL databases

    Before you access an AnalyticDB for MySQL database over the internal network, you must specify a whitelist of Classless Inter-Domain Routing \(CIDR\) blocks for the database. In this example, the CIDR block is 100.104.0.0/16. For more information, see [Configure a whitelist](). For information about the related configurations, see [Access the configurations of ApsaraDB RDS for MySQL](#section_ke7_52c_4wm). When you set the parameters in **Advanced Parameter Settings**, the value of the config.vpc.instance\_id.name parameter must be in the \(the name of the AnalyticDB for MySQL instance + -controller\) format. The following figure shows sample settings.

    ![ADB](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3621950161/p187384.png)


