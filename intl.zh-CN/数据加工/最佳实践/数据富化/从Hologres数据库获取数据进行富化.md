# 从Hologres数据库获取数据进行富化

本文介绍如何通过资源函数从Hologres数据库获取数据进行数据富化。

-   已获取连接Hologress数据库的访问地址、用户名、密码、数据库名和数据库表等。
-   Hologres数据库实例与日志服务Project处于同一地域。

某电商平台将所有物品的销量、购买人的身份信息存储在Hologres数据库中。当新注册的用户登录电商平台时，电商平台根据新用户的性别，在主页为用户推荐相应的商品。面对此类问题，您可以通过日志服务数据加工功能从Hologres数据库获取数据，并通过富化函数将推荐商品存储到日志服务Logstore中，便于其它服务在Logstore中使用推荐的数据。

在该场景中，使用[res\_rds\_mysql](/intl.zh-CN/数据加工/数据加工语法/表达式函数/资源函数.md)函数从Hologres数据库获取数据，然后使用[e\_search\_table\_map](/intl.zh-CN/数据加工/数据加工语法/全局操作函数/映射富化函数.md)函数进行数据富化。

## 使用e\_search\_table\_map函数进行数据富化

-   原始数据
    -   Hologress数据库表中的数据样例如下表所示。

        |product\_id|product\_name|product\_price|product\_sales\_number|sex|
        |-----------|-------------|--------------|----------------------|---|
        |2|lipstick|288|2219|girl|
        |5|watch|1399|265|boy|
        |6|mac|4200|265|boy|
        |3|mouse|20|2583|boy|
        |1|basketball|35|3658|boy|
        |4|notebook|9|5427|girl|

    -   日志服务Logstore中的日志样例如下所示。

        ```
        __source__:192.168.2.100
        __tag__:__client_ip__:192.168.1.100
        age:22
        name:xiaoli
        profession:students
        sex:girl
        
        __source__:192.168.2.200
        __tag__:__client_ip__:192.168.1.200
        age:21
        name:xiaoming
        profession:students
        sex:boy
        ```

-   加工规则

    通过日志服务Logstore中的sex字段和Hologres数据库表中sex字段进行匹配，只有sex字段的值相同时，才能匹配成功。匹配成功后，返回Hologres数据库表中product\_name，与Logstore中的数据拼接，生成新的数据。

    ```
    e_search_table_map(res_rds_mysql(address="rds-host", 
                                    username="mysql-username",password="yourpassword",database="yourdatabasename",table="yourtablename",
                                    refresh_interval=60,connector='pgsql'),
                                    inpt="sex",output_fields="product_name", multi_match=True, multi_join=",")
    ```

    **说明：** 为了访问Hologress实例的安全性和稳定性，建议通过VPC方式访问Hologress数据库。您可以在Hologress实例的网络配置中，获取Hologress数据库的VPC访问域名，将address配置为该值即可。

-   加工结果

    ```
    __source__:192.168.2.100
    __tag__:__client_ip__:192.168.1.100
    __tag__:__receive_time__:1615518043
    __topic__:
    age:22
    name:xiaoli
    product_name:lipstick,notebook
    profession:students
    sex:girl
    
    __source__:192.168.2.200
    __tag__:__client_ip__:192.168.1.200
    __tag__:__receive_time__:1615518026
    __topic__:
    age:21
    name:xiaoming
    product_name:basketball,watch,mac,mouse
    profession:students
    sex:boy
    ```


