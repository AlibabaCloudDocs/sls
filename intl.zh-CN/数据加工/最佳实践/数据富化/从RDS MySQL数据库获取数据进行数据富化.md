# 从RDS MySQL数据库获取数据进行数据富化

日志服务数据加工功能支持从阿里云RDS MySQL数据库获取数据，结合数据加工规则，进行数据富化。

在数据存储场景中，您可能经常遇到数据分散存储的问题，例如用户操作、行为等相关数据存储在日志服务中，用户属性、注册信息等相关数据存储在RDS MySQL数据库中。面对类似问题，您可以通过日志服务数据加工功能从RDS MySQL数据库获取数据，存储到日志服务Logstore中。

您可以使用[res\_rds\_mysql](/intl.zh-CN/数据加工/数据加工语法/表达式函数/资源函数.md)函数从RDS MySQL数据库获取数据，然后使用[e\_table\_map](/intl.zh-CN/数据加工/数据加工语法/全局操作函数/映射富化函数.md)函数或[e\_search\_table\_map](/intl.zh-CN/数据加工/数据加工语法/全局操作函数/映射富化函数.md)函数进行数据富化。

**说明：**

-   RDS MySQL实例与日志服务Project需处于同一地域，否则无法获取数据。
-   如果您要使用RDS内网地址访问RDS MySQL数据库获取数据，进行数据富化，可参见[使用RDS内网地址访问RDS MySQL数据库](/intl.zh-CN/数据加工/最佳实践/数据富化/使用RDS内网地址访问RDS MySQL数据库.md)。

## 结合e\_table\_map函数进行数据富化

本示例介绍使用e\_table\_map函数和res\_rds\_mysql函数完成数据富化的方法。

-   原始数据
    -   RDS MySQL数据库表中的数据样例如下表所示。

        |province|city|population|cid|eid|
        |--------|----|----------|---|---|
        |上海|上海|2000|1|00001|
        |天津|天津|800|1|00002|
        |北京|北京|4000|1|00003|
        |河南|郑州|3000|2|00004|
        |江苏|南京|1500|2|00005|

    -   日志服务Logstore中的日志样例如下所示。

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

-   加工规则

    通过日志服务Logstore中的cid字段和RDS MySQL数据库表中cid字段进行匹配，只有cid字段的值完全相同，才能匹配成功。匹配成功后，返回RDS MySQL数据库表中的province、city和population字段和字段值，与Logstore中的数据拼接，生成新的数据。

    **说明：**

    -   日志字段的值和RDS MySQL数据库表字段的值进行等值匹配时，如果RDS MySQL数据库表字段存在多个相同的值（例如RDS MySQL数据库表有多个值为1的cid字段。），e\_table\_map函数只获取匹配到的第一行数据。
    -   e\_table\_map函数只支持单行匹配，如果您要实现多行匹配，将匹配到的数据组合成新的日志，可使用e\_search\_table\_map函数，详情请参见[结合e\_search\_map\_table函数进行数据富化](#section_e98_4bk_03e)。
    ```
    e_table_map(res_rds_mysql(address="rds-host", username="mysql-username",password="xxx",database="xxx",table="xx",refresh_interval=60),"cid",["province","city","population"])
    ```

    在res\_rds\_mysql函数中配置RDS MySQL数据库相关信息，详情请参见[res\_rds\_mysql](/intl.zh-CN/数据加工/数据加工语法/表达式函数/资源函数.md)。

-   加工结果

    ```
    time:"1566379109"
    data:"test-one"
    cid:"1"
    eid:"00001"
    province:"上海"
    city:"上海"
    population:"2000"
    
    time:"1566379111"
    data:"test_second"
    cid:"1"
    eid:"12345"
    province:"上海"
    city:"上海"
    population:"2000"
    
    time:"1566379111"
    data:"test_three"
    cid:"2"
    eid:"12345"
    province:"河南"
    city:"郑州"
    population:"3000"
    
    time:"1566379113"
    data:"test_four"
    cid:"2"
    eid:"12345"
    province:"河南"
    city:"郑州"
    population:"3000"
    ```


## 结合e\_search\_map\_table函数进行数据富化

本示例介绍使用e\_search\_map\_table函数和res\_rds\_mysql函数完成数据富化的方法。

-   原始数据
    -   RDS MySQL数据库表中的数据样例如下表所示。

        |content|name|age|
        |-------|----|---|
        |city~=n\*|aliyun|10|
        |province~=su$|Maki|18|
        |city:nanjing|vicky|20|

    -   日志服务Logstore中的日志样例如下所示。

        ```
        time:1563436326
        data:123
        city:nanjing
        province:jiangsu
        ```

-   加工规则

    根据指定的RDS MySQL数据库表中的字段值（例如content字段的值）去匹配日志字段，其中指定的RDS MySQL数据库表中的字段值为Key-Value形式，Key对应日志字段，Value为正则表达式，对应日志字段的值。根据匹配结果，将相关字段和字段值与Logstore中的数据拼接，生成新的数据。

    **说明：**

    -   在res\_rds\_mysql函数中配置RDS MySQL数据库相关信息，详情请参见[res\_rds\_mysql](/intl.zh-CN/数据加工/数据加工语法/表达式函数/资源函数.md)。
    -   content字段为RDS MySQL数据库表中的字段，使用该字段的值去匹配日志字段，支持正则匹配、完全匹配、模糊匹配等形式，具体匹配规则请参见[e\_search](/intl.zh-CN/数据加工/数据加工语法/表达式函数/事件检查函数.md)。
    -   单行匹配

        匹配到RDS MySQL数据库表中一行数据就返回。

        ```
        e_search_table_map(res_rds_mysql(address="rds-host", username="mysql-username",password="xxx",database="xxx",table="xx",refresh_interval=60),"content","name")
        ```

    -   多行匹配

        遍历RDS MySQL数据库表中的所有数据行，将匹配到的数据全部添加到指定字段中。

        **说明：** 语法中增加如下两个参数。

        -   `multi_match=True`：开启多行匹配。
        -   `multi_join=","`：匹配到多个值时使用逗号（,）进行组合。
        ```
        e_search_table_map(res_rds_mysql(address="rds-host", username="mysql-username",password="xxx",database="xxx",table="xx",refresh_interval=60),"content","name",multi_match=True,multi_join=",")
        ```

-   加工结果
    -   单行匹配

        例如：匹配日志中字段值符合n\*表达式的city字段，匹配成功后，返回RDS MySQL数据库表中的name字段和字段值，生成一条新的日志。

        ```
        time:1563436326
        data:123
        city:nanjing
        province:jiangsu
        name:aliyun
        ```

    -   多行匹配

        例如：匹配日志中字段值符合n\*表达式的city字段，符合su$表达式的province和是否包含nanjing的city字段。其中~=后面是正则表达式，冒号（:）表示是否包含该字段。匹配成功后，返回RDS MySQL数据库表中的name字段和对应的三个字段值，多个字段值用逗号（,），生成一条新的日志。

        ```
        time:1563436326
        data:123
        city:nanjing
        province:jiangsu
        name:aliyun,Maki,vicky
        ```


