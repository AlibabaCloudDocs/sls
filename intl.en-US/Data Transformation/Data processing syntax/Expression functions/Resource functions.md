# Resource functions

This topic describes the syntax and parameters of resource functions. This topic also provides some examples.

## Functions

**Note:** When you use the following resource functions, you must configure the Advanced preview mode to pull transformed data. For more information about how to configure the Advanced preview mode, see [Advanced preview](/intl.en-US/Data Transformation/Configure preview modes.md).

|Function|Description|
|--------|-----------|
|[res\_local](#section_7nx_87v_uit)|Pulls the values of the advanced parameters of a data transformation task.|
|[res\_rds\_mysql](#section_49h_ufh_ptu)|Pulls data from a specified table in an ApsaraDB RDS for MySQL database or obtains the query result of an SQL statement. The data can be updated at regular intervals.|
|[res\_log\_logstore\_pull](#section_b3c_kth_p0t)|Pulls data from another Logstore when you transform data in a Logstore. You can pull data in a continuous manner.|
|[res\_oss\_file](#section_mlb_osw_xzd)|Obtains the content of an object in a specified bucket from Object Storage Service \(OSS\). The object can be updated at regular intervals.|

## res\_local

The res\_local function is used to pull the values of the advanced parameters of a data transformation task.

-   Syntax

    ```
    res_local(param,default=None,type="auto")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |param|String|Yes|The keys of the advanced parameters that are specified in **Advanced Parameter Settings** for a data transformation task.|
    |default|Arbitrary|No|If the specified value of the param parameter does not exist, the value of the default parameter is returned. Default value: None.|
    |type|String|No|The format of the output data. Valid values:     -   auto: Raw data is converted to a JSON string. If the conversion fails, the data is returned as a raw string. This is the default value.
    -   JSON: Raw data is converted to a JSON string. If the conversion fails, the value of the default parameter is returned.
    -   raw: A raw string is returned. |

-   Response

    A JSON or raw string is returned based on the parameter settings.

-   Conversions to JSON strings
    -   Examples of successful conversions

        |Raw string|Return value|Data type|
        |----------|------------|---------|
        |1|1|Integer|
        |1.2|1.2|Float|
        |true|True|Boolean|
        |false|False|Boolean|
        |"123"|123|String|
        |null|None|None|
        |\["v1", "v2", "v3"\]|\["v1", "v2", "v3"\]|List|
        |\["v1", 3, 4.0\]|\["v1", 3, 4.0\]|List|
        |\{"v1": 100, "v2": "good"\}|\{"v1": 100, "v2": "good"\}|List|
        |\{"v1": \{"v11": 100, "v2": 200\}, "v3": "good"\}|\{"v1": \{"v11": 100, "v2": 200\}, "v3": "good"\}|List|

    -   Examples of failed conversions

        The following table shows some examples of failed conversions. The raw strings are returned if these raw strings fail to be converted to JSON strings.

        |Raw string|Return value|Description|
        |----------|------------|-----------|
        |\(1,2,3\)|"\(1,2,3\)"|Tuples are not supported. The list data type must be used.|
        |True|"True"|The value of the Boolean data type can only be true or false.|
        |\{1: 2, 3: 4\}|"\{1: 2, 3: 4\}"|A dictionary key can only be a string.|

-   Example: The following example describes how to assign the value in the **Advanced Parameter Settings** field to the local parameter.
    -   Advanced parameter settings

        The key in the **Advanced Parameter Settings** field is endpoint. The value is hangzhou.

        ![Advanced parameters](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3211237061/p56312.png)

    -   Raw log entry:

        ```
        content: 1
        ```

    -   Transformation rule:

        ```
        e_set("local", res_local('endpoint'))
        ```

    -   Result:

        ```
        content: 1
        local: hangzhou
        ```


## res\_rds\_mysql

The res\_rds\_mysql function is used to pull data from a specified table in an ApsaraDB RDS for MySQL database or obtain the query result of an SQL statement. Log Service allows you to pull data by using the following three methods.

**Note:**

-   When you use the res\_rds\_mysql function to pull data from an ApsaraDB RDS for MySQL database, you must create a whitelist and add `0.0.0.0` to the whitelist. This allows access to the database from all IP addresses. However, this may also create potential risks for the database. If you want to add the required IP address to the whitelist, you can [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm). Log Service will improve the whitelist feature point in time.
-   Log Service allows you to pull data from an ApsaraDB RDS for MySQL database over the Internet or the internal network. For more information, see [Obtain data from an ApsaraDB RDS for MySQL database over the internal network](/intl.en-US/Data Transformation/Best practices/Data enrichment/Obtain data from an ApsaraDB RDS for MySQL database over the internal network.md).

-   Pull all data only once

    When you run a transformation task for the first time, Log Service pulls all data from a specified database table only once. We recommend that you use this method if your database is not updated.

-   Pull all data at regular intervals

    When you run a transformation task, Log Service pulls all data from a specified database table at regular intervals. This way, Log Service synchronizes all data in the specified database. However, this may require a long period of time if the data volume is large. If the data volume of your database is less than or equal to 2 GB and the value of the refresh\_interval parameter is greater than or equal to 300 seconds, we recommend that you use this method.

    ![Pull all data at regular intervals](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3211237061/p181953.png)

-   Pull incremental data at regular intervals

    When you run a transformation task, Log Service pulls only incremental data based on the timestamp field in the database. In this method, Log Service pulls only the newly added data. This increases the performance when you pull data. You can also set the refresh\_interval parameter to 1 second to synchronize the data in near real time. If your data volume is large and your data is frequently updated, we recommend that you use this method. If you want to synchronize data in near real time, you can also use this method.

    ![Pull incremental data at regular intervals](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3211237061/p181954.png)


-   Syntax

    `res_rds_mysql(address=the address of the database from which data is pulled,username=the username of the database,password=the password of the database,database=the name of the database,table=None,sql=None,fields=None,fetch_include_data=None,fetch_exclude_data=None,refresh_interval=0,base_retry_back_off=1,max_retry_back_off=60,primary_keys=None,use_ssl=false,update_time_key=None,deleted_flag_key=None)`

    **Note:** Log Service also allows you to use the res\_rds\_mysql function to pull data from AnalyticDB for MySQL and PolarDB for MySQL databases. In this case, you need to replace only the address, username, password, and name of the database in the transformation rule with actual values.

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |address|String|Yes|The domain name or IP address. If the port number is not 3306, set this parameter in the format of IP address: port. For more information, see [View the internal and public endpoints and port numbers of an RDS instance](/intl.en-US/RDS SQL Server Database/Database connection/View and change the internal and public endpoints and port numbers of an ApsaraDB
         RDS for SQL Server instance.md).|
    |username|String|Yes|The username that is used to access the database.|
    |password|String|Yes|The password that is used to access the database.|
    |database|String|Yes|The name of the database to be connected.|
    |table|String|Yes|The name of the database table to be accessed. If the sql parameter is specified, this parameter is not required.|
    |sql|String|Yes|The SQL SELECT statement that is used to query data. if the table parameter is specified, this parameter is not required.|
    |fields|The string list or string alias list|No|The string list or string mapping list. If you do not set this parameter, all columns returned by the sql or table parameter are used. For example, if you want to rename the name column in the \["user\_id", "province", "city", "name", "age"\] list to user\_name, you can set the fields parameter to \["user\_id", "province", "city", \("name", "user\_name"\), \("nickname", "nick\_name"\), "age"\]. **Note:** If you set the sql, table, and fields parameters at the same time, the SQL statement in the sql parameter is executed. The table and fields parameters are invalid. |
    |fetch\_include\_data|String|No|The string whitelist. Strings that match the fetch\_include\_data parameter are retained. Strings that do not match this parameter are dropped.     -   If you do not set this parameter, or set this parameter to None, the string whitelist feature is disabled.
    -   If you set this parameter to a specific field and field value, log entries that contain the field and field value are retained. |
    |fetch\_exclude\_data|String|No|The string blacklist. Strings that match the fetch\_exclude\_data parameter are dropped. Strings that do not match this parameter are retained.     -   If you do not set this parameter, or set this parameter to None, the string blacklist feature is disabled.
    -   If you set this parameter to a specific field and field value, log entries that contain the field and field value are dropped.
**Note:** If you set both the fetch\_include\_data and fetch\_exclude\_data parameters, the fetch\_include\_data parameter is first executed. Then, the fetch\_exclude\_data parameter is executed. |
    |refresh\_interval|Numeric string or number|No|The interval at which data is pulled from ApsaraDB RDS for MySQL. Unit: seconds. Default value: 0. This value indicates that all data that matches the search conditions is pulled only once.|
    |base\_retry\_back\_off|Number|No|The interval at which data is re-pulled after a pulling failure. Default value: 1. Unit: seconds.|
    |max\_retry\_back\_off|int|No|The maximum interval between two consecutive retries after a transformation task fails. We recommend that you use the default value. Default value: 60. Unit: seconds.|
    |primary\_keys|String/List|No|The primary key. If you set this parameter, data in the database table is saved to memory as a dictionary in the Key:Value format. The key of the dictionary is the value of the primary\_keys parameter. The value of the dictionary is an entire row of data in the database table. **Note:**

    -   We recommend that you set this parameter if the database table contains a large amount of data.
    -   The value of the primary\_keys parameter must exist in the fields that are pulled from the database table. |
    |use\_ssl|Boolean|No|Specifies whether to use the SSL protocol to connect to the ApsaraDB RDS for MySQL instance. Default value: false. **Note:** If SSL is enabled for the ApsaraDB RDS for MySQL instance and this parameter is specified, the SSL channel is used to connect to the ApsaraDB RDS for MySQL instance. However, the CA certificate of the server is not verified. You cannot use the CA certificate that is provided by the server to establish the connection. |
    |update\_time\_key|String|No|Pulls incremental data. If you do not set this parameter, all the data is pulled. For example, the value update\_time in update\_time\_key="update\_time" indicates the time field of the data update time in the ApsaraDB RDS for MySQL database. The time field supports the following data types: datetime, timestamp, integer, float, and decimal. Make sure that the value of the time field increases in chronological order. **Note:** Log Service pulls incremental data based on the value of the time field. You must make sure that an index is configured for this field in the database table. Otherwise, a full table scan is performed. If you do not configure indexes for this field, an error occurs and you cannot update incremental data. |
    |deleted\_flag\_key|String|No|The data that is dropped when you pull incremental data. For example, if the value of the key field in update\_time\_key="key" meets the following condition, the data is parsed as deleted data:    -   Boolean: true
    -   Datetime and timestamp: not empty
    -   Char and varchar: 1, true, t, yes, and y
    -   Integer: non-zero
**Note:**

    -   You must set the deleted\_flag\_key and update\_time\_key parameters at the same time.
    -   If you set the update\_time\_key parameter but do not set the deleted\_flag\_key parameter, no data is dropped when you pull incremental data. |
    |connector|String|No|The connector of remote data. Valid values: mysql and pgsql. Default value: mysql.|

-   Response

    A table that contains multiple columns is returned. The columns are defined by the fields parameter.

-   Error handling

    If an error occurs during data pulling, an exception appears, and the data transformation task continues. Retries are performed based on the value of the base\_retry\_back\_off parameter. For example, the first retry interval is 1 second and the first retry fails. The second retry interval is twice the length of the first one. In this case, the process will continue until the interval reaches the value of the max\_retry\_back\_off parameter. If the issue persists, retries are performed based on the value of the max\_retry\_back\_off parameter. If a retry succeeds, the retry interval resets to the initial value \(1 second\).

-   Examples
    -   Pull all data
        -   Example 1: Pull data from the test\_table table in the test\_db database every 300 seconds.

            ```
            res_rds_mysql(address="rm-uf6wjk5****.mysql.rds.aliyuncs.com",username="test_username",password="****",database="test_db",table="test_table",refresh_interval=300)
            ```

        -   Example 2: Pull data from the test\_table table, excluding the data whose status value is delete.

            ```
            res_rds_mysql(address="rm-uf6wjk5****.mysql.rds.aliyuncs.com",username="test_username",password="****",database="test_db",table="test_table",refresh_interval=300,fetch_exclude_data="'status':'delete'")
            ```

        -   Example 3: Pull the data whose status value is exit from the test\_table table.

            ```
            res_rds_mysql(address="rm-uf6wjk5****.mysql.rds.aliyuncs.com",username="test_username",password="****",database="test_db",table="test_table",refresh_interval=300,fetch_include_data="'status':'exit'")
            ```

        -   Example 4: Pull the data whose status value is exit from the test\_table table, excluding the data whose name value is aliyun.

            ```
            res_rds_mysql(address="rm-uf6wjk5****.mysql.rds.aliyuncs.com",username="test_username",password="****",database="test_db",table="test_table",refresh_interval=300,fetch_exclude_data="'status':'exit'",fetch_exclude_data="'name':'aliyun'")
            ```

        -   Example 5: Use the pgsql connector to connect to a Hologres database and pull data from the test\_table table.

            ```
            res_rds_mysql(address="hgpostcn-cn-****-cn-hangzhou.hologres.aliyuncs.com:80",username="test_username",password="****",database="aliyun",table="test_table",connector="pgsql")
            ```

        -   Example 6: Pull data by using the primary\_keys parameter.

            If you set the primary\_keys parameter, its value is extracted as a key. In addition, the data pulled from the database table is stored in memory by using the \{"10001":\{"userid":"10001","city\_name":"beijing","city\_number":"12345"\}\} format. This is applicable to scenarios where you want to pull a large amount of data in an efficient manner. If you do not set the primary\_keys parameter, the function traverses each row of data in the database table. Then, the function pulls eligible data, and stores the data in memory by using the \[\{"userid":"10001","city\_name":"beijing","city\_number":"12345"\}\] format. In this case, data pulling is slow and occupies only a small portion of memory. This is applicable to scenarios where you want to pull only a small amount of data.

            -   Database table

                |userid|city\_name|city\_number|
                |------|----------|------------|
                |10001|beijing|12345|

            -   Raw log entries:

                ```
                # Data record 1
                userid:10001
                gdp:1000
                
                # Data record 2
                userid:10002
                gdp:800
                ```

            -   Transformation rule:

                ```
                e_table_map(res_rds_mysql(address="rm-uf6wjk5****.mysql.rds.aliyuncs.com",username="test_username",password="****",database="test_db",table="test_table", primary_keys="userid"),"userid",["city_name","city_number"])
                ```

            -   Result:

                ```
                # Data record 1
                userid:10001
                gdp:1000
                city_name: beijing
                city_number:12345
                # Data record 2
                userid:10002
                gdp:800
                ```

    -   Pull incremental data at regular intervals
        -   Example 1: Pull incremental data.

            **Note:** You can pull incremental data only in the following case:

            -   The database table must have a unique primary key and a time field, such as the item\_id and update\_time fields.
            -   You must set the primary\_keys, refresh\_interval, and update\_time\_key parameters.
            -   Database table

                |item\_id|item\_name|price|
                |--------|----------|-----|
                |1001|Orange|10|
                |1002|Apple|12|
                |1003|Mango|16|

            -   Raw log entries:

                ```
                # Data record 1
                item_id: 1001
                total: 100
                
                # Data record 2
                item_id: 1002
                total: 200
                
                # Data record 3
                item_id: 1003
                total: 300
                ```

            -   Transformation rule:

                ```
                e_table_map(res_rds_mysql(address="rm-uf6wjk5****.mysql.rds.aliyuncs.com",username="test_username",password="****", database="test_db",table="test_table",primary_key="item_id",refresh_interval=1,update_time_key="update_time"),"item_id",["item_name","price"])
                ```

            -   Result:

                ```
                # Data record 1
                item_id: 1001
                total: 100
                item_name: Orange
                price:10
                
                # Data record 2
                item_id: 1002
                total: 200
                item_name: Apple
                price:12
                
                # Data record 3
                item_id: 1003
                total: 300
                item_name: Mango
                price:16
                ```

        -   Example 2: Set the deleted\_flag\_key parameter to drop historical data when you pull incremental data.
            -   Database table

                |item\_id|item\_name|price|update\_time|Is\_deleted|
                |--------|----------|-----|------------|-----------|
                |1001|Orange|10|1603856138|False|
                |1002|Apple|12|1603856140|False|
                |1003|Mango|16|1603856150|False|

            -   Raw log entries:

                ```
                # Data record 1
                item_id: 1001
                total: 100
                
                # Data record 2
                item_id: 1002
                total: 200
                
                # Data record 3
                item_id: 1003
                total: 300
                ```

            -   Transformation rule:

                ```
                e_table_map(res_rds_mysql(address="rm-uf6wjk5****.mysql.rds.aliyuncs.com",username="test_username",password="****",database="test_db",table="test_table",primary_key="item_id",refresh_interval=1,update_time_key="update_time",deleted_flag_key="is_deleted"),"item_id",["item_name","price"])
                ```

            -   Result:

                Three data records are pulled from the database table and sent to Log Service by using the res\_rds\_mysq function. These data records are compared with the existing data records in the source Logstore to check whether the records are matched. If you want to drop the record whose item\_id is 1001, you can find the data record whose item\_id is 1001 in the database table. Then, you can set the value of the Is\_deleted field to true. This way, the 1001 data record is dropped next time when the dimension table is updated.

                ```
                # Data record 2
                item_id: 1002
                total: 200
                item_name: Apple
                price:12
                
                # Data record 3
                item_id: 1003
                total: 300
                item_name: Mango
                price:1
                ```


## res\_log\_logstore\_pull

The res\_log\_logstore\_pull function is used to pull data from another Logstore when you transform data in a Logstore.

-   Syntax

    `res_log_logstore_pull(endpoint,ak\_id,ak\_secret,project,logstore,fields,from_time="begin",to_time="end",fetch_include_data=None,fetch_exclude_data=None,primary_keys=None,upsert_data=None,delete_data=None,max_retry_back_off=60,fetch_interval=2,base_retry_back_off=1)`

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |endpoint|String|Yes|The endpoint. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). The default value is an HTTPS address. This parameter can also be set to an HTTP address. You can use a custom port that is not port 80 or 443.|
    |ak\_id|String|Yes|The AccessKey ID of your Alibaba Cloud account. To ensure data security, we recommend that you set this parameter in **Advanced Parameter Settings**. For more information about how to set the advanced parameters, see [Create a data transformation rule](/intl.en-US/Data Transformation/Create a data transformation rule.md).|
    |ak\_secret|String|Yes|The AccessKey secret of your Alibaba Cloud account. To ensure data security, we recommend that you set this parameter in **Advanced Parameter Settings**. For more information about how to set the advanced parameters, see [Create a data transformation rule](/intl.en-US/Data Transformation/Create a data transformation rule.md).|
    |project|String|Yes|The name of the project from which you want to pull data.|
    |logstore|String|Yes|The name of the Logstore in the project from which you want to pull data.|
    |fields|The string list or string alias list|Yes|The string list or string mapping list. If the Logstore does not contain a specified field, the value of this field is empty. For example, if you want to rename the name column in the \["user\_id", "province", "city", "name", "age"\] list to user\_name, you can set the fields parameter to \["user\_id", "province", "city", \("name", "user\_name"\), \("nickname", "nick\_name"\), "age"\].|
    |from\_time|String|No|The server time when data pulling for the Logstore starts. Default value: begin. This value indicates that data pulling starts from the first log entry. The following time formats are supported:     -   UNIX timestamp.
    -   Time string.
    -   Custom string, for example, begin or end.
    -   Expression: The time that is returned by the dt\_ function. For example, the expression dt\_totimestamp\(dt\_truncate\(dt\_today\(tz="Asia/Shanghai"\), day=op\_neg\(-1\)\)\) indicates that the start time of data pulling is one day before the current day. If the current time is 2019-5-5 10:10:10 \(UTC+8\), this expression indicates the time 2019-5-4 0:0:0 \(UTC+8\). |
    |to\_time|String|No|The server time when data pulling for the Logstore ends. Default value: end. This value indicates that data pulling ends when the last log entry is generated at the current time. The following time formats are supported:     -   UNIX timestamp.
    -   Time string.
    -   Custom string, for example, begin or end.
    -   Expression: The time that is returned by the dt\_ function.
If this parameter is not specified or is set to None, data is pulled from the latest log entry in a continuous manner.

**Note:** If this parameter is set to a time in the future point of time, only the existing data in the Logstore is pulled. Data that is written after the pull starts is not pulled. |
    |fetch\_include\_data|String|No|The string whitelist. Strings that match the fetch\_include\_data parameter are retained. Strings that do not match this parameter are dropped.     -   If you do not set this parameter, or set this parameter to None, the string whitelist feature is disabled.
    -   If you set this parameter to a specific field and field value, log entries that contain the field and field value are retained. |
    |fetch\_exclude\_data|String|No|The string blacklist. Strings that match the fetch\_exclude\_data parameter are dropped. Strings that do not match this parameter are retained.     -   If you do not set this parameter, or set this parameter to None, the string blacklist feature is disabled.
    -   If you set this parameter to a specific field and field value, log entries that contain the field and field value are dropped.
**Note:** If you set both the fetch\_include\_data and fetch\_exclude\_data parameters, the fetch\_include\_data parameter is first executed. Then, the fetch\_exclude\_data parameter is executed. |
    |primary\_keys|String list|No|The list of primary key fields that are used to maintain the table. If you modify the name of a primary key field in the fields parameter, the primary\_key parameter must be set to the modified field as the primary key. **Note:**

    -   The value of the fields parameter must be a single string that is specified in the fields parameter.
    -   The parameter is valid when only one shard exists in the Logstore from which data is pulled. |
    |fetch\_interval|Int|No|The interval between two consecutive data pulling requests. Default value: 2. Unit: seconds. The value must be at least 1 second.|
    |delete\_data|String|No|Deletes data from the table. Log entries whose `primary_keys` parameter is not specified cannot be deleted. For more information, see [Query string syntax](/intl.en-US/Data Transformation/Data processing syntax/General reference/Query string syntax.md).|
    |base\_retry\_back\_off|Number|No|The interval at which data is re-pulled after a pulling failure. Default value: 1. Unit: seconds.|
    |max\_retry\_back\_off|Int|No|The maximum interval between two consecutive retries after a data pulling failure. Default value: 60. Unit: seconds. We recommend that you use the default value.|

-   Response

    A table that contains multiple columns is returned.

-   Error handling

    If an error occurs during data pulling, an exception appears, and the data transformation task continues. Retries are performed based on the value of the base\_retry\_back\_off parameter. For example, the first retry interval is 1 second and the first retry fails. The second retry interval is twice the length of the first one. In this case, the process will continue until the interval reaches the value of the max\_retry\_back\_off parameter. If the issue persists, retries are performed based on the value of the max\_retry\_back\_off parameter. If a retry succeeds, the retry interval resets to the initial value \(1 second\).

-   Examples
    -   In this example, the data of fields key1 and key2 is pulled from the test\_logstore Logstore of the test\_project project. Data pulling starts when log data is written to the Logstore. Data pulling ends when the data write is finished. The data is pulled only once.

        ```
        res_log_logstore_pull("cn-hangzhou.log.aliyuncs.com", "LT****Gw", "ab****uu", "test_project", "test_logstore", ["key1","key2"], from_time="begin", to_time="end")
        ```

    -   In this example, the data of fields key1 and key2 is pulled from the test\_logstore Logstore of the test\_project project. Data pulling starts when log data is written to the Logstore. Data pulling ends when the data write is finished. The data is pulled every 30 seconds in a continuous manner.

        ```
        res_log_logstore_pull("cn-hangzhou.log.aliyuncs.com", "LT****Gw", "ab****uu", "test_project", "test_logstore", ["key1","key2"], from_time="begin", to_time=None,fetch_interval=30)
        ```

    -   In this example, data is pulled from a Logstore and the blacklist is specified to exclude data that contains key1:value1.

        ```
        res_log_logstore_pull("cn-hangzhou.log.aliyuncs.com", "LT****Gw", "ab****uu", "test_project", "test_logstore", ["key1","key2"], from_time="begin", to_time=None,fetch_interval=30,fetch_exclude_data="key1:value1")
        ```

    -   In this example, the whitelist is specified to pull data that contains key1:value1 from a Logstore.

        ```
        res_log_logstore_pull("cn-hangzhou.log.aliyuncs.com", "LT****Gw", "ab****uu", "test_project", "test_logstore", ["key1","key2"], from_time="begin", to_time=None,fetch_interval=30,fetch_include_data="key1:value1")
        ```


## res\_oss\_file

The res\_oss\_file function is used to obtain the object content of a specified bucket from OSS. The object can be refreshed at regular intervals.

**Note:** We recommend that you create an OSS bucket and a project of Log Service in the same region, and use the Alibaba Cloud internal network to obtain data. The internal network is stable and fast.

-   Syntax

    `res_oss_file(endpoint,ak\_id,ak\_key,bucket,file,format='text',change_detect_interval=0,base_retry_back_off=1,max_retry_back_off=60,encoding='utf8',error='ignore')`

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |endpoint|String|Yes|The endpoint of OSS. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md). The default value is an HTTPS address. This parameter can also be set to an HTTP address. You can use a custom port that is not port 80 or 443.|
    |ak\_id|String|Yes|The AccessKey ID of your Alibaba Cloud account. To ensure data security, we recommend that you set this parameter in **Advanced Parameter Settings**. For more information about how to set the advanced parameters, see [Create a data transformation rule](/intl.en-US/Data Transformation/Create a data transformation rule.md).|
    |ak\_key|String|Yes|The AccessKey secret of your Alibaba Cloud account. To ensure data security, we recommend that you set this parameter in **Advanced Parameter Settings**. For more information about how to set the advanced parameters, see [Create a data transformation rule](/intl.en-US/Data Transformation/Create a data transformation rule.md).|
    |bucket|String|Yes|The name of the bucket from which you want to pull object data.|
    |file|String|Yes|The path of the OSS object, for example, test/data.txt. This parameter cannot start with a forward slash \(/\).|
    |format|String|Yes|The format of the output object. Valid values:     -   Text: The output object is a text file.
    -   Binary: The output object is in the byte stream format. |
    |change\_detect\_interval|String|No|The interval at which objects are pulled from OSS. The system checks whether the objects are updated during data pulling. If an object is updated, the pulled object is refreshed. Default value: 0. This value indicates that the pulled object is not updated. The data is pulled only once.|
    |base\_retry\_back\_off|Number|No|The interval at which data is re-pulled after a pulling failure. Default value: 1. Unit: seconds.|
    |max\_retry\_back\_off|int|No|The maximum interval between two consecutive retries after a data pulling failure. Default value: 60. Unit: seconds. We recommend that you use the default value.|
    |encoding|String|No|The encoding format. If the format parameter is set to text, this parameter is set to utf8 by default.|
    |error|String|No|The method of error handling. This parameter is valid only when the UnicodeError message is reported. Examples:    -   The value ignore indicates that the system continues to encode data even if the data format is invalid.
    -   The value xmlcharrefreplace indicates that the characters that cannot be encoded are replaced by XML characters.
For more information, see [Error handlers](https://docs.python.org/3/library/codecs.html#error-handlers).|
    |decompress|String|No|Specifies whether to decompress the obtained OSS object. Valid values:    -   None: Default. This value indicates that the object is not decompressed.
    -   gzip: indicates that Gzip is used to decompress the object. |

-   Response

    Object data is returned in the byte stream or text format.

-   Error handling

    If an error occurs during data pulling, an exception appears, and the data transformation task continues. Retries are performed based on the value of the base\_retry\_back\_off parameter. For example, the first retry interval is 1 second and the first retry fails. The second retry interval is twice the length of the first one. In this case, the process will continue until the interval reaches the value of the max\_retry\_back\_off parameter. If the issue persists, retries are performed based on the value of the max\_retry\_back\_off parameter. If a retry succeeds, the retry interval resets to the initial value \(1 second\).

-   Example 1: Pull JSON data from OSS.
    -   JSON data:

        ```
        {
          "users": [
            {
                "name": "user1",
                "login_historys": [
                  {
                    "date": "2019-10-10 0:0:0",
                    "login_ip": "1.1.1.1"
                  },
                  {
                    "date": "2019-10-10 1:0:0",
                    "login_ip": "1.1.1.1"
                  }
                ]
            },
            {
                "name": "user2",
                "login_historys": [
                  {
                    "date": "2019-10-11 0:0:0",
                    "login_ip": "1.1.1.2"
                  },
                  {
                    "date": "2019-10-11 1:0:0",
                    "login_ip": "1.1.1.3"
                  },
                  {
                    "date": "2019-10-11 1:1:0",
                    "login_ip": "1.1.1.5"
                  }
                ]
            }
          ]
        }
        ```

    -   Raw log entry:

        ```
        content: 123
        ```

    -   Transformation rule:

        ```
        e_set("json_parse",json_parse(res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',ak_id='LT****Gw',
                                                                   ak_key='ab****uu',
                                                                   bucket='log-etl-staging', file='testjson.json')))
        ```

    -   Result:

        ```
        content: 123
            prjson_parse: 
        '{
          "users": [
            {
                "name": "user1",
                "login_historys": [
                  {
                    "date": "2019-10-10 0:0:0",
                    "login_ip": "1.1.1.1"
                  },
                  {
                    "date": "2019-10-10 1:0:0",
                    "login_ip": "1.1.1.1"
                  }
                ]
            },
            {
                "name": "user2",
                "login_historys": [
                  {
                    "date": "2019-10-11 0:0:0",
                    "login_ip": "1.1.1.2"
                  },
                  {
                    "date": "2019-10-11 1:0:0",
                    "login_ip": "1.1.1.3"
                  },
                  {
                    "date": "2019-10-11 1:1:0",
                    "login_ip": "1.1.1.5"
                  }
                ]
            }
          ]
        }'
        ```

-   Example 2: Pull the content of a text file from an OSS bucket.
    -   Text content:

        ```
        Test bytes
        ```

    -   Raw log entry:

        ```
        content: 123
        ```

    -   Transformation rule:

        ```
        e_set("test_txt",res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',ak_id='LT****Gw',
                                                                   ak_key='ab****uu',
                                                                   bucket='log-etl-staging', file='test.txt'))
        ```

    -   Result:

        ```
        content: 123
        test_txt: Test bytes
        ```

-   Example 3: Pull and decompress a compressed object from an OSS bucket.
    -   The content of the compressed OSS bucket:

        ```
        Test bytes\nupdate\n123
        ```

    -   Raw log entry:

        ```
        content:123
        ```

    -   Transformation rule:

        ```
        e_set("text", res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',ak_id="LT****Gw",ak_key="ab****uu", bucket='log-etl-staging', file='test.txt.gz', format='binary', change_detect_interval=30, decompress="gzip"))
        ```

    -   Result:

        ```
        content:123
        text: Test bytes\nupdate\n123
        ```

-   Example 4: Use an AccessKey pair to access an OSS bucket to read and write data.
    -   The content of the compressed OSS bucket:

        ```
        Test bytes
        ```

    -   Raw log entry:

        ```
        content:123
        ```

    -   Transformation rule:

        ```
        e_set("test_txt",res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com', bucket='log-etl-staging', file='test.txt'))
        ```

    -   Result:

        ```
        content: 123
        test_txt: Test bytes
        ```


