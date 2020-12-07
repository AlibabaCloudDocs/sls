# Resource functions

This topic describes the syntax and parameters of resource functions and provides usage examples.

## Functions

|Function|Description|
|--------|-----------|
|[res\_local](#section_7nx_87v_uit)|Obtains the values of the advanced parameters of a data transformation task.|
|[res\_rds\_mysql](#section_49h_ufh_ptu)|Pulls data from a specified table in an ApsaraDB RDS for MySQL database or obtains the query result of an SQL statement. The data can be refreshed at regular intervals.|
|[res\_log\_logstore\_pull](#section_b3c_kth_p0t)|Pulls data from another Logstore when you transform data in a Logstore. You can pull data in a continuous manner.|
|[res\_oss\_file](#section_mlb_osw_xzd)|Obtains the content of an object in a specified bucket from Object Storage Service \(OSS\). The object can be refreshed at regular intervals.|

## res\_local

Use the res\_local function to obtain the values of the advanced parameters of a data transformation task.

-   Syntax

    ```
    res_local(param, default=None, type="auto")
    ```

-   Parameters

    |Parameter|Date type|Required|Description|
    |---------|---------|--------|-----------|
    |param|String|Yes|The keys of the advanced parameters that are specified in **Advanced Parameter Settings** for a data transformation task.|
    |default|Arbitrary|No|The value of the default parameter is returned if the specified param value does not exist. Default value: None.|
    |type|String|No|The format of the output data. Valid values:     -   auto: Default. Raw data is converted into a JSON string. If the conversion fails, the data is returned as a raw string.
    -   JSON: Raw data is converted into a JSON string. If the conversion fails, the value of the default parameter is returned.
    -   raw: A raw string is returned. |

-   Response

    A JSON or raw string is returned based on the parameter settings.

-   Conversions to JSON strings
    -   Examples of successful conversions

        |Raw string|Return value|Return value type|
        |----------|------------|-----------------|
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

        The following table shows the examples of failed conversions. The raw strings are returned if they fail to be converted into JSON strings.

        |Raw string|Return value|Description|
        |----------|------------|-----------|
        |\(1,2,3\)|"\(1,2,3\)"|Tuples are not supported. The list data type must be used.|
        |True|"True"|The value of the Boolean data type can only be true or false.|
        |\{1: 2, 3: 4\}|"\{1: 2, 3: 4\}"|A dictionary key can only be a string.|

-   Example: The following example shows how to assign the value in the **Advanced Parameter Settings** field to the local parameter.
    -   Advanced parameter settings

        The key in the **Advanced Parameter Settings** field is endpoint. The value is hangzhou.

        ![Advanced parameter](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3211237061/p56312.png)

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

Use the res\_rds\_mysql function to pull data from a specified table in an ApsaraDB RDS for MySQL database or obtain the query result of an SQL statement. Log Service allows you to pull data by using three methods.

-   Pull all data only once

    When you run a transformation task for the first time, Log Service pulls all data from a specified database table only once. We recommend that you choose this method if your database is not updated.

-   Pull all data at regular intervals

    When you run a transformation task, Log Service pulls all data from a specified database table at regular intervals. This way, Log Service synchronizes all data in the specified database. However, this may consume a large amount of time if the data volume is large. We recommend that you use this method if the data volume of your database is less than or equal to 2 GB and the value of the refresh\_interval parameter is greater than or equal to 300s.

    ![Pull all data at regular intervals](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3211237061/p181953.png)

-   Pull incremental data at regular intervals

    When you run a transformation task, Log Service pulls only incremental data based on the timestamp field in the database. In this method, Log Service pulls only the newly added data. This makes the data pull more efficient. You can also set the refresh\_interval parameter to 1 second to synchronize the data in near real time. We recommend that you use this method if your data volume is large and your data is frequently updated. You can also use this method if you want to synchronize data in near real time.

    ![Pull incremental data at regular intervals](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3211237061/p181954.png)


-   Syntax

    ```
    res_rds_mysql("the address of the database from which data is pulled", "the username of the database", "the password of the database", "the name of the database", table=None, sql=None, fields=None, fetch_include_data=None, fetch_exclude_data=None, refresh_interval=0, base_retry_back_off=1, max_retry_back_off=60, primary_keys=None, use_ssl=false, update_time_key=None, deleted_flag_key=None)
    ```

    **Note:**

    -   The whitelist in an ApsaraDB RDS for MySQL database: When you configure the whitelist in an ApsaraDB RDS for MySQL database, set the IP Addresses parameter to `0.0.0.0` so that all IP addresses can access the database. However, this may expose the database to higher potential risks. If you want to configure a specified Log Service IP address, you can [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm). Log Service will provide a more secure method to support the whitelist feature in the future.
    -   Network access: Log Service allows you to pull data from an ApsaraDB RDS for MySQL database over the Internet or the internal network. For more information, see [Obtain data from an ApsaraDB RDS for MySQL database over the internal network](/intl.en-US/Data Transformation/Best practices/Data enrichment/Obtain data from an ApsaraDB RDS for MySQL database over the internal network.md).
-   Parameters

    |Parameter|Date type|Required|Description|
    |---------|---------|--------|-----------|
    |address|String|Yes|The domain name or IP address. If the port number is not 3306, set this parameter in the format of IP address: port. For more information, see [View the internal and public endpoints and port numbers of an RDS instance](/intl.en-US/RDS SQL Server Database/Database connection/View and change the internal and public endpoints and port numbers of an ApsaraDB RDS for SQL Server instance.md).|
    |username|String|Yes|The username that is used to access the database.|
    |password|String|Yes|The password of the username.|
    |database|String|Yes|The name of the database to be connected.|
    |table|String|Yes|The name of the database table to be accessed. This parameter is not required if the sql parameter is specified.|
    |sql|String|Yes|The SQL SELECT statement that is used to query data. This parameter is not required if the table parameter is specified.|
    |fields|The string list or string alias list|No|The string list or string alias list. If you do not set this parameter, all columns returned by the sql or table parameter are used. For example, if you want to change the name of the name column in the \["user\_id", "province", "city", "name", "age"\] list to user\_name, you can set the fields parameter to \["user\_id", "province", "city", \("name", "user\_name"\), \("nickname", "nick\_name"\), "age"\]. **Note:** If you set the sql, table, and fields parameters at the same time, the SQL statement in the sql parameter is executed. The table and fields parameters become invalid. |
    |fetch\_include\_data|Search string|No|The string whitelist. Strings that match the fetch\_include\_data parameter are retained. Strings that do not match this parameter are dropped.|
    |fetch\_exclude\_data|Search string|No|The string blacklist. Strings that match the fetch\_exclude\_data parameter are dropped. Strings that do not match this parameter are retained.**Note:** If you set both the fetch\_include\_data and fetch\_exclude\_data parameters, the string blacklist is executed first. |
    |refresh\_interval|Numeric string or number|No|The interval at which data is pulled from ApsaraDB RDS for MySQL. Unit: seconds. The default value is 0. This indicates that all data that matches the search conditions is pulled only once.|
    |base\_retry\_back\_off|Number|No|The interval at which data is re-pulled after a pull failure. Default value: 1. Unit: seconds.|
    |max\_retry\_back\_off|int|No|The maximum interval between retries after a transformation task fails. We recommend that you use the default value. Default value: 60. Unit: seconds.|
    |primary\_keys|String/List|No|The primary key. If you set this parameter, data in the database table is saved to the memory as a dictionary in the Key : Value format. The key of the dictionary is the value of the primary\_keys parameter. The value of the dictionary is an entire row of data in the database table.**Note:**

    -   We recommend that you set this parameter if the database table contains a large amount of data.
    -   The value of the primary\_keys parameter must exist in the fields that are pulled from the database table. |
    |use\_ssl|Bool|No|Indicates whether to use the SSL protocol to connect to ApsaraDB RDS for MySQL. Default value: false. **Note:** If SSL is enabled for ApsaraDB RDS for MySQL and this parameter is specified, the SSL channel is used to connect to ApsaraDB RDS for MySQL, but the CA certificate of the server is not verified. You cannot use the CA certificate provided by the server to establish the connection. |
    |update\_time\_key|String|No|Pulls incremental data. If you do not set this parameter, all data is pulled. For example, the value update\_time in the update\_time\_key="update\_time" parameter indicates the time field of the data update time in the ApsaraDB RDS for MySQL database. The time field supports the following data types: Datetime, Timestamp, Integer, Float, and Decimal. Make sure that the value of the time field increases in chronological order.**Note:** Log Service pulls incremental data based on the value of the time field. Make sure that you have indexed this field in the database table. Otherwise, a full table scan will occur. If you do not configure indexes for this field, an error occurs and you cannot update incremental data. |
    |deleted\_flag\_key|String|No|Indicates the data that is dropped when you pull incremental data. For example, if the value of the key field in update\_time\_key="key" meets the following condition, the data is parsed as deleted data:    -   Boolean: true
    -   Datetime and Timestamp: not empty
    -   Char and Varchar: 1, true, t, yes, and y
    -   Int: not zero
**Note:**

    -   You must set the deleted\_flag\_key and update\_time\_key parameters at the same time.
    -   If you set the update\_time\_key parameter but do not set the deleted\_flag\_key parameter, no data is dropped when you pull incremental data. |

-   Response

    A table that contains multiple columns is returned. The columns are defined by the fields parameter.

-   Error handling

    If an error occurs during the data pull process, an error message is displayed, and the data transformation task continues. Retries are performed based on the value of the base\_retry\_back\_off parameter. For example, assume that the first retry interval is 1 second and the first retry fails. The second retry interval is twice the length of the first one, and so on until the interval reaches the value of the max\_retry\_back\_off parameter. If the failure persists, retries are performed based on the value of the max\_retry\_back\_off parameter. If a retry succeeds, the retry interval resets to the initial value \(1 second\).

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

        -   Example 5: Pull data by using the primary\_keys parameter.

            If you set the primary\_keys parameter, its value is extracted as a key and the data pulled from the database table is stored in the memory in the \{"10001":\{"userid":"10001","city\_name":"beijing","city\_number":"12345"\}\} format. This is applicable to scenarios where you want to pull a large amount of data in an efficient manner. If you do not set the primary\_keys parameter, the function traverses each row of data in the database table, pulls data that meets the matching conditions, and then stores the data in the memory in the format of \[\{"userid":"10001","city\_name":"beijing","city\_number":"12345"\}\]. In this case, the data pull is slow and occupies only a small portion of memory. This is applicable to scenarios where you want to pull only a small amount of data.

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

    -   Pull incremental data
        -   Example 1: Pull incremental data.

            **Note:** To pull incremental data, the following conditions must be met:

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
                e_table_map(res_rds_mysql(address="rm-uf6wjk5****.mysql.rds.aliyuncs.com", username="test_username", password="****", database="test_db",table="test_table",primary_key="item_id",refresh_interval=1,update_time_key="update_time"),"item_id",["item_name","price"])
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
                e_table_map(res_rds_mysql(address="rm-uf6wjk5****.mysql.rds.aliyuncs.com", username="test_username", password="****", database="test_db",table="test_table",primary_key="item_id",refresh_interval=1,update_time_key="update_time", deleted_flag_key="is_deleted"),"item_id",["item_name","price"])
                ```

            -   Result:

                Three data records are pulled from the database table and send to Log Service by using the res\_rds\_mysq function. These data records are compared with the existing data records in the source Logstore to check whether they are matched. If you want to drop the record whose item\_id is 1001, you can find the data record whose item\_id is 1001 in the database table. Then, you can set the value of the Is\_deleted field to true. This way, the 1001 data record is dropped next time when the dimension table is updated.

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

Use the res\_log\_logstore\_pull function to pull data from another Logstore.

-   Syntax

    ```
    res_log_logstore_pull(endpoint, ak_id, ak_secret, project, logstore, fields, from_time="begin", to_time="end", fetch_include_data=None, fetch_exclude_data=None, primary_keys=None, upsert_data=None, delete_data=None,max_retry_back_off=60,fetch_interval=2,base_retry_back_off=1)
    ```

-   Parameters

    |Parameter|Date type|Required|Description|
    |---------|---------|--------|-----------|
    |endpoint|String|Yes|The endpoint. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). The default value is an HTTPS address. This parameter can also be set to an HTTP address. You can use a custom port that is not port 80 or 443.|
    |ak\_id|String|Yes|The AccessKey ID of your Alibaba Cloud account. To ensure data security, we recommend that you set this parameter in **Advanced Parameter Settings**. For more information about how to set the advanced parameters, see [Create a data transformation rule](/intl.en-US/Data Transformation/Create a data transformation rule.md).|
    |ak\_secret|String|Yes|The AccessKey secret of your Alibaba Cloud account. To ensure data security, we recommend that you set this parameter in **Advanced Parameter Settings**. For more information about how to set the advanced parameters, see [Create a data transformation rule](/intl.en-US/Data Transformation/Create a data transformation rule.md).|
    |project|String|Yes|The name of the project from which you want to pull data.|
    |logstore|String|Yes|The name of the Logstore in the project from which you want to pull data.|
    |fields|The string list or string alias list|Yes|The string list or string alias list. If the Logstore does not contain a specified field, the value of this field is empty. For example, if you want to change the name of the name column in the \["user\_id", "province", "city", "name", "age"\] list to user\_name, you can set the fields parameter to \["user\_id", "province", "city", \("name", "user\_name"\), \("nickname", "nick\_name"\), "age"\].|
    |from\_time|String|No|The server time at which the data pull from the Logstore starts. The default value is begin. This value indicates that the data pull starts from the first log entry. The following time formats are supported:     -   UNIX timestamp.
    -   Time string.
    -   Custom string, for example, begin or end.
    -   Expression: The time returned by the dt\_ function. For example, the expression dt\_totimestamp\(dt\_truncate\(dt\_today\(tz="Asia/Shanghai"\), day=op\_neg\(-1\)\)\) indicates the start time of the data pull is one day before the current day. If the current time is 2019-5-5 10:10:10 \(UTC+8\), this expression indicates the time 2019-5-4 0:0:0 \(UTC+8\). |
    |to\_time|String|No|The server time at which the data pull from the Logstore ends. The default value is end, which indicates to end the data pull at the last log entry. The following time formats are supported:     -   UNIX timestamp.
    -   Time string.
    -   Custom string, for example, begin or end.
    -   Expression: The time returned by the dt\_ function.
If this parameter is not specified or is set to None, data is pulled from the latest log entry in a continuous manner.

**Note:** If this parameter is set to a future time, only the existing data in the Logstore is pulled. Data that is written after the pull begins is not pulled. |
    |fetch\_include\_data|Search string|No|The string whitelist. Strings that match the fetch\_include\_data parameter are retained. Strings that do not match this parameter are dropped.|
    |fetch\_exclude\_data|Search string|No|The string blacklist. Strings that match the fetch\_exclude\_data parameter are dropped. Strings that do not match this parameter are retained.**Note:** If you set both the fetch\_include\_data and fetch\_exclude\_data parameters, the string blacklist is executed first. |
    |primary\_keys|String list|No|The list of primary key fields used to maintain the table. If you modify the name of a primary key field in the fields parameter, the primary\_key parameter must be set to the modified field as the primary key. **Note:**

    -   The value of the fields parameter must be a single string specified in the fields parameter.
    -   The parameter is valid when only one shard exists in the Logstore from which data is pulled. |
    |fetch\_interval|Int|No|The interval between continuous data pull requests. Default value: 2. Unit: seconds. The value must be at least 1 second.|
    |delete\_data|Search string|No|Deletes data from the table. Log entries whose `primary_keys` parameter is not specified cannot be deleted. For more information, see [Query string syntax](/intl.en-US/Data Transformation/Data processing syntax/General reference/Query string syntax.md).|
    |base\_retry\_back\_off|Number|No|The interval at which data is re-pulled after a pull failure. Default value: 1. Unit: seconds.|
    |max\_retry\_back\_off|Int|No|The maximum interval between retries after a data pull failure. Default value: 60. Unit: seconds. We recommend that you use the default value.|

-   Response

    A table that contains multiple columns is returned.

-   Error handling

    If an error occurs during the data pull process, an error message is displayed, and the data transformation task continues. Retries are performed based on the value of the base\_retry\_back\_off parameter. For example, assume that the first retry interval is 1 second and the first retry fails. The second retry interval is twice the length of the first one, and so on until the interval reaches the value of the max\_retry\_back\_off parameter. If the failure persists, retries are performed based on the value of the max\_retry\_back\_off parameter. If a retry succeeds, the retry interval resets to the initial value \(1 second\).

-   Examples
    -   The following example pulls data of fields key1 and key2 from the test\_logstore Logstore of the test\_project project. The data pull starts when log data is written to the Logstore and ends when the data write is finished. The data pull is performed only once.

        ```
        res_log_logstore_pull("cn-hangzhou.log.aliyuncs.com", "LT****Gw", "ab****uu", "test_project", "test_logstore", ["key1","key2"], from_time="begin", to_time="end")
        ```

    -   The following example pulls data of fields key1 and key2 from the test\_logstore Logstore of the test\_project project. The data pull starts when log data is written to the Logstore and ends when the data write is finished. The data is pulled every 30 seconds in a continuous manner.

        ```
        res_log_logstore_pull("cn-hangzhou.log.aliyuncs.com", "LT****Gw", "ab****uu", "test_project", "test_logstore", ["key1","key2"], from_time="begin", to_time=None,fetch_interval=30)
        ```

    -   The following example pulls data from the Logstore and specifies the blacklist to exclude data that contains key1:value1.

        ```
        res_log_logstore_pull("cn-hangzhou.log.aliyuncs.com", "LT****Gw", "ab****uu", "test_project", "test_logstore", ["key1","key2"], from_time="begin", to_time=None,fetch_interval=30,fetch_exclude_data="key1:value1")
        ```

    -   The following example specifies the whitelist and pulls data that contains key1:value1 from the Logstore.

        ```
        res_log_logstore_pull("cn-hangzhou.log.aliyuncs.com", "LT****Gw", "ab****uu", "test_project", "test_logstore", ["key1","key2"], from_time="begin", to_time=None,fetch_interval=30,fetch_include_data="key1:value1")
        ```


## res\_oss\_file

Use the res\_oss\_file function to obtain the object content of a specified bucket from OSS. The object can be refreshed at regular intervals.

**Note:** We recommend that you create the OSS bucket and the data transformation project in the same region. Then, the OSS bucket and the Logstore can communicate with each other over an internal network, which is stable and fast.

-   Syntax

    ```
    
    res_oss_file(endpoint, ak_id, ak_key, bucket, file, format='text', change_detect_interval=0,base_retry_back_off=1, max_retry_back_off=60,encoding='utf8',error='ignore')
    ```

-   Parameters

    |Parameter|Date type|Required|Description|
    |---------|---------|--------|-----------|
    |endpoint|String|Yes|The endpoint of OSS. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md). The default value is an HTTPS address. This parameter can also be set to an HTTP address. You can use a custom port that is not port 80 or 443.|
    |ak\_id|String|Yes|The AccessKey ID of your Alibaba Cloud account. To ensure data security, we recommend that you set this parameter in **Advanced Parameter Settings**. For more information about how to set the advanced parameters, see [Create a data transformation rule](/intl.en-US/Data Transformation/Create a data transformation rule.md).|
    |ak\_key|String|Yes|The AccessKey secret of your Alibaba Cloud account. To ensure data security, we recommend that you set this parameter in **Advanced Parameter Settings**. For more information about how to set the advanced parameters, see [Create a data transformation rule](/intl.en-US/Data Transformation/Create a data transformation rule.md).|
    |bucket|String|Yes|The name of the bucket from which you want to pull object data.|
    |file|String|Yes|The path of the OSS file, for example, test/data.txt. This parameter cannot start with a forward slash \(/\).|
    |format|String|Yes|The format of the output file. Valid values:     -   Text: The output file is a text file.
    -   Binary: The output file is in the byte stream format. |
    |change\_detect\_interval|String|No|The interval at which objects are pulled from OSS. During the data pull, the system checks whether the objects are updated. If an object is updated, the pulled object is refreshed. The default value is 0, which indicates that the pulled object is not refreshed. The data pull is performed only once.|
    |base\_retry\_back\_off|Number|No|The interval at which data is re-pulled after a pull failure. Default value: 1. Unit: seconds.|
    |max\_retry\_back\_off|int|No|The maximum interval between retries after a data pull failure. Default value: 60. Unit: seconds. We recommend that you use the default value.|
    |encoding|String|No|The encoding format. If the format parameter is set to text, this parameter is set to utf8 by default.|
    |error|String|No|The handling method of errors. This parameter is valid only when the UnicodeError message is reported. For example:    -   The value ignore indicates that incorrectly encoded data is ignored and the encoding continues.
    -   The value xmlcharrefreplace indicates that the characters that cannot be encoded are replaced by XML characters.
For more information, see [Error handlers](https://docs.python.org/3/library/codecs.html#error-handlers).|
    |decompress|String|No|Indicates whether to decompress the obtained OSS file. Valid values: None and gizp. Default value: None.    -   decompress=None indicates that the file is not decompressed.
    -   decompress=gizpindicates that Gizp is used to decompress the file. |

-   Response

    Object data is returned in the byte stream or text format.

-   Error handling

    If an error occurs during the data pull process, an error message is displayed, and the data transformation task continues. Retries are performed based on the value of the base\_retry\_back\_off parameter. For example, assume that the first retry interval is 1 second and the first retry fails. The second retry interval is twice the length of the first one, and so on until the interval reaches the value of the max\_retry\_back\_off parameter. If the failure persists, retries are performed based on the value of the max\_retry\_back\_off parameter. If a retry succeeds, the retry interval resets to the initial value \(1 second\).

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

-   Example 2: Pull the content of the text file from OSS.
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

-   Example 3: Pull and decompress a compressed file from OSS.
    -   The content of the compressed OSS file:

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

-   Example 4: Use an AccessKey pair to access OSS to read and write data.
    -   The content of the compressed OSS file:

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


