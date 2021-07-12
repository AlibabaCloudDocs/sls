# unnest语法

## 应用场景

处理数据时，一列数据通常为字符串或数字等primitive类型的数据。在复杂的业务场景下，日志数据的某一列可能会是较为复杂的格式，例如数组（array）、对象（map）、JSON等格式。对这种特殊格式的日志字段进行查询分析，可以使用unnest语法。

例如以下日志：

```
__source__:  1.1.1.1
__tag__:__hostname__:  vm-req-170103232316569850-tianchi111932.tc
__topic__:  TestTopic_4
array_column:  [1,2,3]
double_column:  1.23
map_column:  {"a":1,"b":2}
text_column:  商品
```

其中`array_column`字段为数组类型。如果统计`array_column`中所有数值的汇总值，需要遍历每一行的数组中的每一个元素。

## unnest语法结构

|语法|说明|
|:-|:-|
|`unnest( array) as table_alias(column_name)`|表示把array类型展开成多行，行的名称为`column_name`。|
|`unnest(map) as table(key_name, value_name)`|表示把map类型展开成多行，key的名称为`key_name`，value的名称为`value_name`。|

**说明：** 注意，由于unnest接收的是array或者map类型的数据，如果您的输入为字符串类型，那么要先转化成JSON类型，然后再转化成array类型或map类型，转化的方式为`cast(json_parse(array_column) as array(bigint))`。

## 遍历数组每一个元素

使用SQL把array展开成多行：

```
* | select array_column, a from log, unnest(cast(json_parse(array_column) as array(bigint))) as t(a)
```

上述SQL把数组展开成多行数字，`unnest( cast( json_parse(array_column) as array(bigint) ) ) as t(a)`，unnest语法把数组展开，以t来命名新生成的表，使用a来引用展开后的列。

结果如下图：

![展开数组](../images/p292793.png "展开数组")

-   统计数组中的每个元素的和：

    ```
    * | select sum(a) from log, unnest(cast(json_parse(array_column) as array(bigint))) as t(a)
    ```

    ![sum计算](../images/p292796.png "对数组进行sum计算")

-   按照数组中的每个元素进行group by计算：

    ```
    * | select a, count(1) from log, unnest( cast( json_parse(array_column) as array(bigint))) as t(a) group by a
    ```

    ![group by计算](../images/p292797.png "对数组进行group by计算")


## 遍历Map

-   遍历Map中的元素：

    ```
    * | select map_column , a,b from log, unnest(cast(json_parse(map_column) as map(varchar, bigint))) as t(a,b)
    ```

    ![map遍历](../images/p292800.png "遍历Map")

-   按照Map的key进行group by统计：

    ```
    * | select key, sum(value) from log, unnest(cast(json_parse(map_column) as map(varchar, bigint))) as t(key,value) GROUP BY key
    ```

    ![key group by](../images/p292803.png "对Key进行group by统计")


## 格式化显示histogram,numeric\_histogram的结果

-   histogram

    histogram函数类似于count group by 语法。语法请参考[Map映射函数](/cn.zh-CN/查询与分析/SQL分析语法与功能/Map映射函数.md)。

    通常情况下histogram的结果为一串JSON数据，无法配置视图展示，例如：

    ```
    * | select histogram(method)
    ```

    ![histogram](../images/p292805.png "普通histogram结果")

    您可以通过unnest语法，把JSON展开成多行配置视图，例如：

    ```
    * | select key , value from(select histogram(method) as his from log) , unnest(his ) as t(key,value)
    ```

    ![展开JSON](../images/p292806.png "展开JSON")

    接下来，可以配置可视化视图：

-   numeric\_histogram

    numeric\_histogram语法是为了把数值列分配到多个桶中去，相当于对数值列进行group by，具体语法请参考[估算函数](/cn.zh-CN/查询与分析/SQL分析语法与功能/估算函数.md)。

    ```
    * | select numeric_histogram(10,Latency)
    ```

    numeric\_histogram的输出如下：

    ![numeric_histogram](../images/p292808.png "numeric_histogram查询结果")

    您可以通过以下查询语句格式化展示该结果：

    ```
    * |  select key,value from(select numeric_histogram(10,Latency) as his from log) , unnest(his) as t(key,value)
    ```

    结果如下：

    ![查询结果](../images/p292809.png "查询结果")

    同时配置柱状图的形式展示：

    ![可视化2](../images/p292810.png "可视化视图2")


