# URL请求分类函数

URL请求分类函数会自动将您输入的URL请求路径进行归类打标签，并提供类别的正则表达式，帮助您更好的归类URL，查询结果可供ETL使用。

**说明：** 目前，URL请求分类函数只支持华北2（北京）、华东2（上海）地域。

-   调用方式

    ```
    select url_classify(url_path varchar);
    select url_classify(url_path varchar, weight long);
    ```

-   输入参数

    |参数|说明|
    |--|--|
    |url\_path|URL请求路径。|
    |weight|URL请求路径的数量。|

-   输出参数

    |参数|说明|
    |--|--|
    |url\_path|URL请求路径。|
    |api\_path|通过函数推导出URL请求路径对应的接口。|
    |regex\_tpl|通过算法推导出的正则表达式。|

-   输出结果

    ```
         url_path                          |     api_path                             |         regex_tpl
    -------------------------------------+------------------------------+-------------------------------------
     /gl/balance/666398186799140           | /gl/balance/*                             | \/gl\/balance\/[0-9].+
     /gl/glaccount/30579281472076          | /gl/glaccount/*                         | \/gl\/glaccount\/[0-9].+
     /gl/balance/709016207098025           | /gl/balance/*                            | \/gl\/balance\/[0-9].+
    ```

-   示例
    -   查询分析语句

        ```
        * | select url_classify(uri, num) from (select uri, COUNT(*) as num from log group by uri limit 1000)
        ```

    -   查询分析结果

        ![URL请求分类函数](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5636703061/p96745.png)


