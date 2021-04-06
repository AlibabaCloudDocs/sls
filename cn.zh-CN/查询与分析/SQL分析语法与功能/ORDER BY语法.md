# ORDER BY语法

ORDER BY语法用于根据指定的列名对查询和分析结果进行排序。

## 语法格式

```
ORDER BY 列名 [DESC | ASC]
```

**说明：**

-   您可以指定多个列名，按照不同的排序方式排序。例如`ORDER BY 列名1 [DESC | ASC], 列名2 [DESC | ASC]`。
-   如果您未配置关键字DESC或ASC，则系统默认对查询和分析结果进行升序排列。
-   当排序的目标列中存在相同的值时，每次排序结果可能不同。如果您希望每次序列结果相同，可指定多个列进行排序。

参数说明如下表所示。

|参数|说明|
|--|--|
|列名|列名即为日志字段名称或聚合函数计算结果列，即支持按照日志字段名称（KEY）或聚合函数计算结果列进行排序。|
|DESC|降序排列。|
|ASC|升序排列。|

## 示例

-   统计不同请求状态的数量，并按照请求数量降序排列。

    ```
    * | SELECT count(*) as PV,status GROUP BY status ORDER BY PV DESC
    ```

-   计算写入日志到各个Logstore的平均延迟时间，并按照平均延迟时间进行降序排列。

    ```
    method:PostLogstoreLogs |SELECT avg(latency) AS avg_latency, LogStore GROUP BY LogStore ORDER BY avg_latency DESC
    ```

-   计算不同请求时长对应的请求数量，并按照请求时长进行升序排序。

    其中，content、time和request\_time为JSON日志中的字段。

    **说明：**

    在查询和分析JSON类型的日志时，需注意以下事项：

    -   必须给字段名称加上JSON中父路径前缀，例如content.time.request\_time。
    -   分析语句中的JSON字段名称必须使用双引号（""）包裹，例如"content.time.request\_time"。
    ```
    * | SELECT "content.time.request_time", COUNT(*) AS count GROUP BY "content.time.request_time" ORDER BY "content.time.request_time"
    ```


