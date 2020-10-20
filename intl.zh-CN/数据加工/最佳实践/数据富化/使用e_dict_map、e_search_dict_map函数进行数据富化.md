# 使用e\_dict\_map、e\_search\_dict\_map函数进行数据富化

本文介绍使用映射富化函数e\_dict\_map、e\_search\_dict\_map进行数据富化的实践案例。

日志服务数据加工映射富化函数包括普通映射函数和搜索映射函数，两者区别如下所示：

-   普通映射函数使用文本完全匹配方式来映射。普通映射函数包括[e\_dict\_map](/intl.zh-CN/数据加工/数据加工语法/全局操作函数/映射富化函数.md)函数和[e\_table\_map](/intl.zh-CN/数据加工/数据加工语法/全局操作函数/映射富化函数.md)函数，两者区别在于e\_dict\_map函数接收的是dict类型的数据，e\_table\_map函数接收的是通过[资源函数](/intl.zh-CN/数据加工/数据加工语法/表达式函数/资源函数.md)获取的table类型的数据。

    例如：在NGNIX日志中，将特定的状态码转换为文本格式，可以使用普通映射函数e\_dict\_map。

    |状态码|文本|
    |---|--|
    |200|成功|
    |300|跳转|
    |400|请求错误|
    |500|服务器错误|

-   搜索映射函数的映射关键字是[查询字符串](/intl.zh-CN/数据加工/数据加工语法/通用参考/查询字符串语法.md)，支持正则表达式匹配、完全匹配、模糊匹配等形式。搜索映射函数包括[e\_search\_dict\_map](/intl.zh-CN/数据加工/数据加工语法/全局操作函数/映射富化函数.md)函数和[e\_search\_table\_map](/intl.zh-CN/数据加工/数据加工语法/全局操作函数/映射富化函数.md)函数，两者区别在于e\_search\_dict\_map函数接收的是dict类型的数据，而e\_search\_table\_map函数接收的是通过[资源函数](/intl.zh-CN/数据加工/数据加工语法/表达式函数/资源函数.md)获取的table类型的数据。

    例如：在NGNIX日志中，将一定范围内的状态码转换为文本格式，可以使用搜索映射函数e\_search\_dict\_map。

    |状态码|文本|
    |---|--|
    |2XX|成功|
    |3XX|跳转|
    |4XX|请求错误|
    |5XX|服务器错误|


## 使用e\_dict\_map函数进行数据富化

本案例介绍使用e\_dict\_map函数完成数据富化的方法。

-   原始日志

    ```
    http_host:  m1.abcd.com
    http_status:  300
    request_method:  GET
    
    http_host:  m2.abcd.com
    http_status:  200
    request_method:  POST
    
    http_host:  m3.abcd.com
    http_status:  400
    request_method:  GET
    
    http_host:  m4.abcd.com
    http_status:  500
    request_method:  GET
    ```

-   加工需求

    将http\_status字段中的请求状态码转化为文本格式，并添加到status\_desc字段中。

-   加工规则

    ```
    e_dict_map({"400": "请求错误", "500": "服务器错误", "300": "跳转", "200": "成功"}, "http_status", "status_desc")
    ```

    **说明：** 在实际情况中，HTTP请求状态码不止以上4种，详情请参见[HTTP请求状态码](https://www.restapitutorial.com/httpstatuscodes.html)。当http\_status字段的值为401、404时，需要更新字典覆盖，否则无法匹配。

-   加工结果

    ```
    http_host:  m1.abcd.com
    http_status:  300
    request_method:  GET
    status_desc: 跳转
    
    http_host:  m2.abcd.com
    http_status:  200
    request_method:  POST
    status_desc: 成功
    
    http_host:  m3.abcd.com
    http_status:  400
    request_method:  GET
    status_desc: 请求错误
    
    http_host:  m4.abcd.com
    http_status:  500
    request_method:  GET
    status_desc: 服务器错误
    ```


## 使用e\_search\_dict\_map函数进行数据富化

本案例介绍使用e\_search\_dict\_map函数完成数据富化的方法。

-   原始日志

    ```
    http_host:  m1.abcd.com
    http_status:  200
    request_method:  GET
    body_bytes_sent: 740
    
    http_host:  m2.abcd.com
    http_status:  200
    request_method:  POST
    body_bytes_sent: 1123
    
    http_host:  m3.abcd.com
    http_status:  404
    request_method:  GET
    body_bytes_sent: 711
    
    http_host:  m4.abcd.com
    http_status:  504
    request_method:  GET
    body_bytes_sent: 1822
    ```

-   加工需求

    根据日志中的http\_status字段和body\_bytes\_sent字段的值的不同，为每条日志添加不同的type信息。

    -   为http\_status为2XX且body\_bytes\_sent长度小于1000的日志，添加type字段，并将字段值设置为正常。
    -   为http\_status为2XX且body\_bytes\_sent长度大于等于1000的日志，添加type字段，并将字段值设置为过长警告。
    -   为http\_status为3XX的日志，添加type字段，并将字段值设置为重定向。
    -   为http\_status为4XX的日志，添加type字段，并将字段值设置为错误。
    -   为其余所有日志，添加type字段，并将字段值设置为其他。
-   加工规则

    ```
    e_search_dict_map({'http_status~="2\d+" and body_bytes_sent < 1000': "正常", 'http_status~="2\d+" and body_bytes_sent >= 1000': "过长警告", 'http_status~="3\d+"': "重定向", 'http_status~="4\d+"': "错误",  "*": "其他"}, "http_status", "type")
    ```

    基于字典的富化，除了可以使用大括号（\{\}）直接构建字典外，还可以基于任务配置资源、外部OSS资源、表格资源等来构建字典，详情请参见[字典构建](/intl.zh-CN/数据加工/最佳实践/数据富化/构建字典与表格做数据富化.md)。

-   加工结果

    ```
    type: 正常
    http_host:  m1.abcd.com
    http_status:  200
    request_method:  GET
    body_bytes_sent: 740
    
    type: 过长警告
    http_host:  m2.abcd.com
    http_status:  200
    request_method:  POST
    body_bytes_sent: 1123
    
    type: 错误
    http_host:  m3.abcd.com
    http_status:  404
    request_method:  GET
    body_bytes_sent: 711
    
    type: 其他
    http_host:  m4.abcd.com
    http_status:  504
    request_method:  GET
    body_bytes_sent: 1822
    ```


