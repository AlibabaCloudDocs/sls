# Map映射函数

本文档为您介绍Map映射函数的语法说明及示例。

日志服务查询分析功能支持通过映射函数进行日志分析，详细语句及含义如下所示。

|函数|含义|示例|
|:-|:-|:-|
|下标运算符\[\]|获取map中某个Key对应的结果。|-|
|histogram\(x\)|按照x的每个值进行GROUP BY并计算count。语法相当于`select count group by x`。 **说明：** 返回结果为JSON格式。

|`latency > 10 | select histogram(status)`，等同于`latency > 10 | select count(1) group by status`。|
|histogram\_u\(x\)|按照x的每个值进行GROUP BY并计算count。 **说明：** 返回结果为多行多列。

|`latency > 10 | select histogram_u(status)`，等同于`latency > 10 | select count(1) group by status`。|
|map\_agg\(Key,Value\)|返回Key、Value组成的map，并返回每个Key随机的Value。|`latency > 100 | select map_agg(method,latency)`|
|multimap\_agg\(Key,Value\)|返回Key、Value组成的多Value map，并返回每个Key的所有的Value。|`latency > 100 | select multimap_agg(method,latency)`|
|cardinality\(x\) → bigint|获取map的大小。|-|
|element\_at\(map`<K, V>`, key\) → V|获取Key对应的Value。|-|
|map\(\) → map`<unknown, unknown>`|返回一个空的map。|-|
|map\(array`<K>`, array`<V>`\) → map`<K,V>`|将两个数组转换成Key、Value组成的Map。|`SELECT map(ARRAY[1,3], ARRAY[2,4]); — {1 -> 2, 3 -> 4}`|
|map\_from\_entries\(array`<row<K, V>>`\) → map`<K,V>`|把一个多维数组转化成map。|`SELECT map_from_entries(ARRAY[(1, ‘x’), (2, ‘y’)]); — {1 -> ‘x’, 2 -> ‘y’}`|
|map\_concat\(map1`<K, V>`, map2`<K, V>`, …, mapN`<K, V>`\) → map`<K,V>`|求多个map的并集，如果某个Key存在于多个map中，则取最后一个。|-|
|map\_filter\(map`<K, V>`, function\) → map`<K,V>`|请参见[Lambda函数](/cn.zh-CN/查询和分析/SQL分析语法与功能/Lambda函数.md)中map\_filter函数。|-|
|map\_keys\(x`<K, V>`\) → array`<K>`|获取map中所有的Key并以array的形式返回。|-|
|map\_values\(x`<K, V>`\) → array`<V>`|获取map中所有的Value并以array的形式返回。|-|

