# JSON函数

本文介绍JSON函数的使用及示例。

**说明：**

-   如果字符串被解析成JSON类型失败，则返回null。
-   在日志服务分析语句中，使用单引号（''）包裹的JSON数组表示字符串。
-   如果日志字段的值为JSON类型且需要展开为多行，请使用unnest语法。更多信息，请参见[unnest语法](/cn.zh-CN/查询与分析/SQL分析语法与功能/unnest语法.md)。

## json\_parse\(\)函数

json\_parse\(\)函数用于将字符串转化成JSON类型，返回结果为JSON类型。

-   语法

    ```
    json_parse(string)
    ```

-   示例

    将字符串\[1, 2, 3\]转换为JSON数组\[1,2,3\]。

    ```
     * | SELECT json_parse('[1, 2, 3]')
    ```

    返回结果为\[1,2,3\]。


## json\_format\(\)

json\_format\(\)函数用于将JSON类型转化成字符串，返回结果为字符串。

-   语法

    ```
    json_format(json)
    ```

-   示例

    将JSON数组\[1,2,3\]转换为字符串\[1, 2, 3\]。

    ```
    * | SELECT json_format(json_parse('[1, 2, 3]'))
    ```

    返回结果为\[1,2,3\]。


## json\_array\_contains\(\)

json\_array\_contains\(\)函数用于判断JSON数组或JSON字符串中是否包含某个值，返回结果为true或者false。

-   语法

    ```
    json_array_contains(json , value)
    ```

-   示例
    -   判断JSON数组\[1, 2, 3\]中，是否包含2。

        ```
        * | SELECT json_array_contains(json_parse('[1, 2, 3]'), 2)
        ```

        返回结果为true。

    -   判断JSON字符串\[1, 2, 3\]中，是否包含2。

        ```
        * | SELECT json_array_contains('[1, 2, 3]', 2)
        ```

        返回结果为true。


## json\_array\_get\(\)

json\_array\_get\(\)函数用于获取JSON数组下标对应的元素。

-   语法

    ```
    json_array_get(json_array, index)
    ```

-   示例

    返回JSON数组\["status", "request\_time", "request\_method"\]下标为0的元素。

    ```
    * | SELECT json_array_get('["status", "request_time", "request_method"]', 0)
    ```

    返回结果为status。


## json\_array\_length\(\)

json\_array\_length\(\)函数用于计算JSON数组中元素的数量。

-   语法

    ```
    json_array_length(json array)
    ```

-   示例

    计算JSON数组\["status", "request\_time", "request\_method"\]中的元素数量。

    ```
    * | SELECT json_array_length('["status", "request_time", "request_method"]')
    ```

    返回结果为3。


## json\_extract\(\)

json\_extract\(\)函数用于从JSON对象中提取目标字段的值，返回结果为JSON类型。

**说明：** 针对非法的JSON类型，json\_extract\(\)函数会抛出错误，建议您使用json\_extract\_scalar\(\)函数。

-   语法

    ```
    json_extract(json, json_path)
    ```

    json\_path格式为`$.store.book[0].title`。

-   示例
    -   content字段是一个JSON对象，从该对象中获取status字段的值。

        ```
        * | SELECT json_extract(content, '$.status')
        ```

        返回结果为status字段的值，例如**"200"**。

    -   request\_time字段值为JSON数组，将该数组展开并使用row引用展开后的列。然后从row中获取status字段的值进行求和。

        ```
        * | select sum(cast (json_extract_scalar(row, '$.status') as bigint) ) from  log, unnest(cast(json_parse(request_time)  as array(json) ) as  t(row)
        ```

        返回结果为求和结果。


## json\_extract\_scalar\(\)

json\_extract\_scalar\(\)函数用于从JSON对象中提取目标字段的值，返回结果为字符串。

-   语法

    ```
    json_extract_scalar(json, json_path)
    ```

    json\_path格式为`$.store.book[0].title`。

-   示例
    -   content字段是一个JSON对象，从该对象中获取status字段的值。

        ```
        * | SELECT json_extract_scalar(content, '$.status')
        ```

        返回结果为status字段的值，例如**200**。

    -   content字段是一个JSON对象，从该对象中获取status字段的值，并将该值转换为BIGINT类型进行求和。

        ```
        * | select sum( cast (json_extract_scalar(content, '$.status') as bigint) )
        ```

        返回结果为求和结果。


## json\_size\(\)

json\_size\(\)函数用于计算JSON对象或JSON数组中元素的数量。

-   语法

    ```
    json_size(json,json_path)
    ```

-   示例

    返回status字段中元素的数量。

    ```
    * | SELECT json_size('{"status":[1, 2, 3]}','$.status') 
    ```

    返回结果为3。


