# How can I fix errors of the syntax used to pull data from ApsaraDB RDS for MySQL?

If the data transformation rule of a Logstore also pulls data from ApsaraDB for RDS, data pull or update errors may occur. This topic introduces such errors and their corresponding resolutions.

After reading data from a Logstore, the data transformation engine starts to transform log events in the Logstore. If the transformation rule involves data pulls from external resources such as Object Storage Service \(OSS\), ApsaraDB for RDS, and other Logstores, data pull or update errors may occur.

## Incorrect use of a resource function in the console

-   Transformation rule

    ```
    res_rds_mysql(address="xx",username="xx",password="xx",database="xx")
    ```

-   Error message

    ```
    aliyun.log.logexception.LogException: {"errorCode": "InvalidEtlConfig", "errorMessage": "ETL config doesn't pass security check, detail: invalid type detected: <class '_ast.Expr'>", "requestId": ""}
    ```

-   Cause

    This error occurs because the syntax is incorrect. The error may occur when you use the `res_rds_mysql`, `res_log_logstore_pull`, or `res_oss_file` function alone in the console.

-   Resolution

    Use the resource function together with the `e_table_map` or `e_search_table_map` function in the console.


## Incorrect parameter settings

-   Transformation rule

    ```
    e_table_map(res_rds_mysql(address="xx",username="xx",password="xx",database="xx"),field="processid",output_fields=["name","xixi"])
    ```

-   Error message

    ```
    "errorMessage\": \"When sql is not set, table must be set\\nDetail: None\"
    ```

-   Cause

    This error occurs because the `table` and `sql` parameters are not set.

-   Resolution

    If you do not set the `table` parameter, set the `sql` parameter. If you do not set the `sql` parameter, set the `table` parameter.


## Incorrect output fields

-   Transformation rule

    ```
    e_table_map(res_rds_mysql(address="x",username="xx",password="xx",database="xx",table="test"),field="processid",output_fields=["name","xixi"])
    ```

-   Error message

    ```
    "errorMessage\": \"trans_comp_lookup: output field xixi doesn't exist in lookup table\\nDetail: None\
    ```

-   Cause

    This error occurs because the `output_fields` settings do not exist in tables stored in ApsaraDB RDS for MySQL.

-   Resolution

    Confirm the fields contained in the MySQL database, and then enter the correct fields in the `output_fields` parameter.


## Incorrect parameter type settings

-   Transformation rule

    ```
    e_table_map(res_rds_mysql(address="xxx",username=1234,password="xx",database="xx",table="xx"),field="processid",output_fields=["name","xixi"])
    ```

-   Error message

    ```
    "errorMessage\": \"username not a string type\\nInvalid Settings
    ```

-   Cause

    This error occurs because the specified `username` is not a string.

-   Resolution

    Set correct parameter types.


## Network or permission error

-   Transformation rule

    ```
    e_table_map(res_rds_mysql(address="xxx",username=xxx,password="xx",database="test999",table="xx"),field="processid",output_fields=["name","xixi"])
    ```

-   Error message 1

    ```
    message:  Database connection failed, cause: (1045, "Access denied for user 'root'@'47.99.57.53' (using password: YES)"
    ```

-   Error message 2

    ```
    message:  Database connection failed, cause: (1049, "Unknown database 'test999'")
    ```

-   Cause

    The `Database connection failed` error occurs because the network configurations are incorrect, the network is disconnected, or your account is not in the whitelist of ApsaraDB RDS for MySQL. If a connection failure occurs, the cause is included in the error message. The cause can be a permission error, incorrect password, or incorrect IP address.

    **Note:** VPC cannot connect to ApsaraDB RDS for MySQL. If a VPC IP address is included in the transformation rule, a network connection failure occurs. The `address` parameter must be set to the public IP address of ApsaraDB RDS for MySQL. VPC connections to ApsaraDB RDS for MySQL are under development.

-   Resolution

    Modify the function parameters and restart the data transformation task.


## SQL syntax error

-   Transformation rule

    ```
    e_table_map(res_rds_mysql(address="xxx",username=xxx,password="xx",database="xx",sql="inset into test values(1,"aini")",field="processid",output_fields=["name","xixi"])
    ```

-   Error message

    ```
    "errorMessage\": \"The sql_query field only supports database query syntax\\nInvalid Settings \\\"insert into test values(1,aini)\\\"\\nDetail: None\", \"requestId\": \"\"}
    ```

-   Cause

    This error occurs because the specified SQL syntax is incorrect. The SQL syntax only supports reading data from a database by using the SELECT statement. If the SELECT statement is incorrect, the `fetch data error` message is reported, indicating the error cause.

-   Resolution

    Fix the SQL statement error. Make sure that only the SELECT statement is used.


## Continuous-transformation error

-   Transformation rule

    ```
    e_table_map(res_rds_mysql(address="xxx",username=xxx,password="xx",database="xx",table="test,field="processid",output_fields=["name","xixi"],refresh_interval=30)
    ```

-   Error message

    ```
    "errorMessage\": \"Database connection failed, cause: (2003, \\\"Can't connect to MySQL server on 'rm-wz9z68i4itrk4v8d9yo.mysql.rds.aliyuncs.com' (timed out)
    ```

-   Cause

    This error occurs because the IP address used for data transformation is removed from the whitelist of ApsaraDB RDS for MySQL. If an error \(for example, a network connection failure\) occurs during continuous transformation, automatic retries are conducted. If access permissions are removed, you must re-grant them manually. You can find the cause of the connection error based on the reported error message.

-   Resolution

    Check whether tables or user permissions are changed in ApsaraDB RDS for MySQL.


