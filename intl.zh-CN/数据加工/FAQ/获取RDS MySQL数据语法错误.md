# 获取RDS MySQL数据语法错误

如果加工规则中涉及RDS资源的加载，则有可能会产生资源的加载或刷新错误。本文介绍从RDS MySQL获取数据出错的原因以及排查处理方法。

在成功读取源Logstore数据后，加工引擎开始对源Logstore的日志事件进行加工。如果加工规则中涉及OSS、RDS、Logstore等外联资源的加载，则也有可能会产生资源的加载或刷新错误。

## 控制台单独使用资源函数

-   加工规则

    ```
    res_rds_mysql(address="xx",username="xx",password="xx",database="xx")
    ```

-   错误日志

    ```
    aliyun.log.logexception.LogException: {"errorCode": "InvalidEtlConfig", "errorMessage": "ETL config doesn't pass security check, detail: invalid type detected: <class '_ast.Expr'>", "requestId": ""}
    ```

-   排查方法

    该错误信息为语法配置错误，这种错常见于用户在控制台单独使用res\_rds\_mysql、res\_log\_logstore\_pull或res\_oss\_file函数。

-   解决方案

    不可以单独在控制台上使用资源函数，需结合e\_table\_map或者e\_search\_table\_map等函数使用。


## 参数填写错误

-   加工规则

    ```
    e_table_map(res_rds_mysql(address="xx",username="xx",password="xx",database="xx"),field="processid",output_fields=["name","xixi"])
    ```

-   错误日志

    ```
    "errorMessage\": \"When sql is not set, table must be set\\nDetail: None\"
    ```

-   排查方法

    确保已配置table参数或sql参数。

-   解决方案

    当您未配置table参数时，必须配置指定的sql语句，否则找不到数据库表无法进行数据获取。table参数和sql参数，必须配置其中一个。


## 输出字段错误

-   加工规则

    ```
    e_table_map(res_rds_mysql(address="x",username="xx",password="xx",database="xx",table="test"),field="processid",output_fields=["name","xixi"])
    ```

-   错误日志

    ```
    "errorMessage\": \"trans_comp_lookup: output field xixi doesn't exist in lookup table\\nDetail: None\
    ```

-   排查方法

    output\_fields参数中配置的字段并不存在于MySQL数据表中。

-   解决方案

    请确认MySQL数据表所包含的字段，将正确的字段填入output\_fields中。


## 参数类型配置错误

-   加工规则

    ```
    e_table_map(res_rds_mysql(address="xxx",username=1234,password="xx",database="xx",table="xx"),field="processid",output_fields=["name","xixi"])
    ```

-   错误日志

    ```
    "errorMessage\": \"username not a string type\\nInvalid Settings
    ```

-   排查方法

    username的值不是String类型。

-   解决方案

    配置正确的数据类型。


## 网络或者权限错误

-   加工规则

    ```
    e_table_map(res_rds_mysql(address="xxx",username=xxx,password="xx",database="test999",table="xx"),field="processid",output_fields=["name","xixi"])
    ```

-   错误日志1

    ```
    message:  Database connection failed, cause: (1045, "Access denied for user 'root'@'47.99.57.53' (using password: YES)")
    ```

-   错误日志2

    ```
    message:  Database connection failed, cause: (1049, "Unknown database 'test999'）
    ```

-   排查方法

    请检查配置是否错误、网络是否连通、账号是否在RDS MySQL白名单中。当出现无法连接错误时，错误日志的cause字段中，会展示详细的原因。您可以根据错误原因进行排查，一般为权限、密码、地址等错误。

-   解决方案

    排查原因后重新配置函数，重启数据加工服务。


## SQL语法错误

-   加工规则

    ```
    e_table_map(res_rds_mysql(address="xxx",username=xxx,password="xx",database="xx",sql="inset into test values(1,"aini")",field="processid",output_fields=["name","xixi"]))
    ```

-   错误日志

    ```
    "errorMessage\": \"The sql_query field only supports database query syntax\\nInvalid Settings \\\"insert into test values(1,aini)\\\"\\nDetail: None\", \"requestId\": \"\"}
    ```

-   排查方法

    SQL语法错误。首先SQL语法仅支持从数据库中读取数据的模式。其次，当写入数据库的SQL语法错误事，也会引发`fetch data error`错误，此时请根据错误原因分析。

-   解决方案

    更新SQL语句。资源函数中的SQL语法只支持SELECT查询语句。


## 持续刷新错误

-   加工规则

    ```
    e_table_map(res_rds_mysql(address="xxx",username=xxx,password="xx",database="xx",table="test,field="processid",output_fields=["name","xixi"],refresh_interval=30))
    ```

-   错误日志

    ```
    "errorMessage\": \"Database connection failed, cause: (2003, \\\"Can't connect to MySQL server on 'rm-wz9z68i4itrk4v8d9yo.mysql.rds.aliyuncs.com' (timed out))
    ```

-   排查方法

    RDS MySQL数据库将白名单授权取消，导致无法连接的错误。当持续加工过程中遇到错误，例如部分网络震荡，会自动进行重试。如果遇到权限被取消、数据表被删除的错误，需要手动恢复。

-   解决方案

    请根据错误原因确认RDS MySQL数据库中的数据表或者权限发生了变动。


