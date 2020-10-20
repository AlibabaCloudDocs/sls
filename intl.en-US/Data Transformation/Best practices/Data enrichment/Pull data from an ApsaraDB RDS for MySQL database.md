# Pull data from an ApsaraDB RDS for MySQL database

The data transformation feature of Log Service allows you to pull data from an ApsaraDB RDS for MySQL database to enrich the data in Log Service.

When you analyze data, you may need to obtain the data from separate storage resources. For example, data of user operations and user behavior is stored in Log Service, while data of user properties, registration, funds, and props is stored in an ApsaraDB RDS for MySQL database. In this case, you can use the data transformation feature to pull data from the database and store the data in a Logstore.

You can use the [res\_rds\_mysql](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.md) function to pull data from an ApsaraDB RDS for MySQL database and then use the [e\_table\_map](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md) or [e\_search\_table\_map](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md) function to enrich data.

**Note:**

-   The RDS database and the Log Service project must reside in the same region. Otherwise, data cannot be obtained from the database.
-   For more information about how to use an RDS internal endpoint to access an RDS database and enrich data, see [Obtain data from an ApsaraDB RDS for MySQL database over the internal network](/intl.en-US/Data Transformation/Best practices/Data enrichment/Obtain data from an ApsaraDB RDS for MySQL database over the internal network.md).

## Enrich data by using e\_table\_map functions

This example describes how to use the e\_table\_map and res\_rds\_mysql functions to enrich data.

-   Raw data
    -   The following table shows sample data entries in an RDS table.

        |province|city|population|cid|eid|
        |--------|----|----------|---|---|
        |Shanghai|Shanghai|2000|1|00001|
        |Tianjin|Tianjin|800|1|00002|
        |Beijing|Beijing|4000|1|00003|
        |Henan|Zhengzhou|3000|2|00004|
        |Jiangsu|Nanjing|1500|2|00005|

    -   The following content shows sample log entries in the Logstore of Log Service.

        ```
        time:"1566379109"
        data:"test-one"
        cid:"1"
        eid:"00001"
        
        time:"1566379111"
        data:"test_second"
        cid:"1"
        eid:"12345"
        
        time:"1566379111"
        data:"test_three"
        cid:"2"
        eid:"12345"
        
        time:"1566379113"
        data:"test_four"
        cid:"2"
        eid:"12345"
        ```

-   Transformation rule

    Log Service maps the cid field in the specified Logstore to the cid field in the specified RDS table. If the value of the cid field in the specified Logstore and table equals each other, the two fields are matched and the province, city, and population fields and the field values in the RDS table are concatenated with the data fields in the specified Logstore.

    **Note:**

    -   If the value of multiple cid fields is 1, the e\_table\_map function maps the first data entry.
    -   The e\_table\_map function supports only single-row matching. To implement multi-row matching, you can use the e\_search\_table\_map function. For more information, see [Enrich data by using the e\_search\_map\_table function](#section_e98_4bk_03e).
    ```
    e_table_map(res_rds_mysql(address="rds-host", username="mysql-username",password="xxx",database="xxx",table="xx",refresh_interval=60),"cid",["province","city","population"])
    ```

    For more information about how to configure an ApsaraDB RDS for MySQL database in the res\_rds\_mysql function, see [res\_rds\_mysql](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.md).

-   Result

    ```
    time:"1566379109"
    data:"test-one"
    cid:"1"
    eid:"00001"
    province:"Shanghai"
    city:"Shanghai"
    population:"2000"
    
    time:"1566379111"
    data:"test_second"
    cid:"1"
    eid:"12345"
    province:"Shanghai"
    city:"Shanghai"
    population:"2000"
    
    time:"1566379111"
    data:"test_three"
    cid:"2"
    eid:"12345"
    province:"Henan"
    city:"Zhengzhou"
    population:"3000"
    
    time:"1566379113"
    data:"test_four"
    cid:"2"
    eid:"12345"
    province:"Henan"
    city:"Zhengzhou"
    population:"3000"
    ```


## Enrich data by using the e\_search\_map\_table function

This example describes how to use the e\_search\_map\_table and res\_rds\_mysql functions to enrich data.

-   Raw data
    -   The following table shows sample data entries in an RDS table.

        |content|name|age|
        |-------|----|---|
        |city~=n\*|aliyun|10|
        |province~=su$|Maki|18|
        |city:nanjing|vicky|20|

    -   The following content shows a sample log entry in the Logstore of Log Service.

        ```
        time:1563436326
        data:123
        city:nanjing
        province:jiangsu
        ```

-   Transformation rule

    A transformation rule matches log fields based on the value of the field in the specified RDS table. In this example, the field is content. The value of the field in the specified RDS table is in the key-value pair format. The key specifies the name of a log field and the value specifies the value of a log field. The value is a regular expression. After the field in the specified RDS table and a log field is matched, relevant fields and field values are concatenated with the log entry.

    **Note:**

    -   For more information about how to configure an ApsaraDB RDS for MySQL database in the res\_rds\_mysql function, see [res\_rds\_mysql](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.md).
    -   The content field is in the specified RDS table. You can use the value of this field to match a log field. You can use the regular expression matching, exact matching, or fuzzy matching method. For more information about the matching rules, see [e\_search](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Event check functions.md).
    -   Single-row matching

        Matches a single field value in the specified RDS table.

        ```
        e_search_table_map(res_rds_mysql(address="rds-host", username="mysql-username",password="xxx",database="xxx",table="xx",refresh_interval=60),"content","name")
        ```

    -   Multi-row matching

        Matches all field values in the specified RDS table and add matched field values to the specified field.

        **Note:** In multi-row matching mode, you must specify the following two parameters:

        -   `multi_match=True`: enables multi-row matching.
        -   `multi_join=,"`: multiple matched values are separated by commas \(,\).
        ```
        e_search_table_map(res_rds_mysql(address="rds-host", username="mysql-username",password="xxx",database="xxx",table="xx",refresh_interval=60),"content","name",multi_match=True,multi_join=",")
        ```

-   Result
    -   Single-row matching

        Matches the city field whose field value matches the n\* expression. If matched, returns the name field and field value from the specified RDS table and concatenates the field and field value with a log entry in Log Service.

        ```
        time:1563436326
        data:123
        city:nanjing
        province:jiangsu
        name:aliyun
        ```

    -   Multi-row matching

        Matches the city field whose field value matches the expression n\*, the province field whose field value matches the expression su$, and the city field whose field value matches the expression nanjing. A regular expression is preceded by the expression ~= . The colon \(:\) indicates whether the field is included. If matched, returns the name field and the three field values from the specified RDS table and concatenates the field and field value with a log entry in Log Service.

        ```
        time:1563436326
        data:123
        city:nanjing
        province:jiangsu
        name:aliyun,Maki,vicky
        ```


