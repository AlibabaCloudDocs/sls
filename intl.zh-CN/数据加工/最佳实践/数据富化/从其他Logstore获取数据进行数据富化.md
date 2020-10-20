# 从其他Logstore获取数据进行数据富化

本文档介绍如何通过资源函数从其他Logstore中获取数据对数据进行富化。

某酒店将客人个人信息存储在名为user\_logstore的Logstore中，将客人入住信息存储在名为check-in\_logstore的Logstore中，现在酒店希望从check-in\_logstore中获取部分字段数据，与user\_logstore中的数据拼接。针对该需求，日志服务提供[res\_log\_logstore\_pull](/intl.zh-CN/数据加工/数据加工语法/表达式函数/资源函数.md)函数从check-in\_logstore中获取数据，提供[e\_table\_map](/intl.zh-CN/数据加工/数据加工语法/全局操作函数/映射富化函数.md)函数或[e\_search\_table\_map](/intl.zh-CN/数据加工/数据加工语法/全局操作函数/映射富化函数.md)函数实现数据富化。

## 数据加工

-   原始数据
    -   用于存储个人信息的Logstore（user\_logstore）

        ```
        topic:xxx
        city:xxx
        cid:12345
        name:maki
        
        
        topic:xxx
        city:xxx
        cid:12346
        name:vicky
        
        topic:xxx
        city:xxx
        cid:12347
        name:mary
        ```

    -   用于存储入住信息的Logstore（check-in\_logstore）

        ```
        time:1567038284
        status:check in
        cid:12345
        name:maki
        room_number:1111
        
        time:1567038284
        status:check in
        cid:12346
        name:vicky
        room_number:2222
        
        time:1567038500
        status:check in
        cid:12347
        name:mary
        room_number:3333
        
        time:1567038500
        status:leave
        cid:12345
        name:maki
        room_number:1111
        ```

-   加工规则

    **说明：** res\_log\_logstore\_pull函数支持设置时间范围，您可以设置一个时间区间，也可以只设置开始时间。

    -   在加工规则中，设置from\_time=1567038284,to\_time=1567038500，则表示只获取该时间范围内的Logstore数据。
    -   在加工规则中，设置from\_time="begin"，则表示持续获取Logstore数据。
    res\_log\_logstore\_pull函数中的各个字段详情请参见[res\_log\_logstore\_pull](/intl.zh-CN/数据加工/数据加工语法/表达式函数/资源函数.md)。

    -   e\_table\_map函数

        通过两个Logstore中相同的cid字段进行匹配，只有cid字段的值完全相同，才能匹配成功。匹配成功后，返回Logstore（check-in\_logstore）中的room\_number字段和字段值，与Logstore（check-in\_logstore）中的数据拼接，生成新的数据。

        ```
        e_table_map(res_log_logstore_pull(endpoint, ak_id, ak_secret, project, logstore, 
                fields=["cid","room_number"],
                from_time="begin",
                ), "cid","room_number")
        ```

    -   e\_search\_table\_map函数

        使用e\_search\_table\_map函数对Logstore（check-in\_logstore）和Logstore（user\_logstore）做搜索匹配，搜索Logstore（check-in\_logstore）中cid为12346的数据，返回该数据中的room\_number字段和字段值，与Logstore（user\_logstore）中的数据拼接，生成新的数据。

        ```
        e_search_table_map(res_log_logstore_pull(endpoint, ak_id, ak_secret, project, logstore, 
                fields=["cid","room_number"],
                from_time="begin",
                ), "cid=12346","room_number")
        ```

-   加工结果
    -   e\_table\_map函数

        ```
        topic:xxx
        city:xxx
        cid:12345
        name:maki
        room_nuber:1111
        
        topic:xxx
        city:xxx
        cid:12346
        name:vicky
        room_number:2222
        
        topic:xxx
        city:xxx
        cid:12347
        name:mary
        room_number:3333
        ```

    -   e\_search\_table\_map函数

        ```
        topic:xxx
        city:xxx
        cid:12346
        name:vicky
        room_number:2222
        ```


## 设置黑白名单过滤数据

-   设置白名单
    -   加工规则

        通过fetch\_include\_data设置白名单，例如fetch\_include\_data="room\_number:1111"表示在获取数据过程中，只获取room\_number值为1111的数据。

        ```
        res_log_logstore_pull(endpoint, ak_id, ak_secret, project, logstore, ["cid","name","room_number"，"status"],from_time=1567038284,to_time=1567038500,fetch_include_data="room_number:1111")
        ```

    -   获取到的数据

        ```
        status: check in
        cid:12345
        name:maki
        room_number:1111
        
        status:leave
        cid:12345
        name:maki
        room_number:1111
        ```

-   设置黑名单
    -   加工规则

        通过fetch\_exclude\_data设置黑名单，例如fetch\_exclude\_data="room\_number:1111"表示在获取数据过程中，丢弃room\_number值为1111的数据。

        ```
        res_log_logstore_pull(endpoint, ak_id, ak_secret, project, logstore, ["cid","name","room_number"，"status"],from_time=1567038284,to_time=1567038500,fetch_exclude_data="room_number:1111")
        ```

    -   获取到的数据

        ```
        status:check in
        cid:12346
        name:vicky
        room_number:2222
        
        
        status:check in
        cid:12347
        name:mary
        room_number:3333
        ```

-   同时设置黑白名单
    -   加工规则

        同时设置黑白名单时，优先匹配黑名单，再匹配白名单。例如fetch\_exclude\_data="time:1567038285",fetch\_include\_data="status:check in"表示在数据获取过程中，先匹配time值为1567038285的数据进行丢弃，再匹配status值为check in的数据进行获取。

        ```
        res_log_logstore_pull(endpoint, ak_id, ak_secret, project, logstore, ["cid","name","room_number"，"status"],from_time=1567038284,to_time=1567038500,fetch_exclude_data="time:1567038285",fetch_include_data="status:check in")
        ```

    -   获取到的数据

        ```
        status:check in
        cid:12345
        name:maki
        room_number:1111
        
        
        status:check in
        cid:12346
        name:vicky
        room_number:2222
        
        
        status:check in
        cid:12347
        name:mary
        room_number:3333
        ```


## 开启主键维护获取目标Logstore数据

当您获取到数据，还没加工时，您希望删除已获取到数据，不再加工，您可以开启主键维护功能。例如：您要在名为check-in\_logstore的Logstore中，获取已入住还未离开的客户入住信息，如果获取到的数据中包含status:leave表示客人已经离开，则开启主键维护功能不加工该数据。

**说明：**

-   primary\_keys参数只支持设置单个字符串，且必须存在于fields字段中。
-   开启主键维护功能时，待拉取数据的Logstore中只能存在一个Shard。
-   开启主键维护功能时，delete\_data字段必须不为None。

-   加工规则

    ```
    res_log_logstore_pull(endpoint, ak_id, ak_secret, project, logstore, ["cid","name","room_number"，"status","time"],from_time=1567038284,to_time=None,primary_keys="cid",delete_data="status:leave")
    ```

-   获得数据

    name为maki的客人，最后的入住信息为status:leave表示已离开酒店，则不加工该客人的相关数据。

    ```
    time:1567038284
    status:check in
    cid:12346
    name:vicky
    room_number:2222
    
    time:1567038500
    status:check in
    cid:12347
    name:mary
    room_number:3333
    ```


