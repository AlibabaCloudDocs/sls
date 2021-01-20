# GROUP BY语法

GROUP BY语法用于结合聚合函数，根据一个或多个列对分析结果进行分组。

## 语法格式

```
* | SELECT 列名, 聚合函数 GROUP BY [ 列名 | 别名 | 序号 ]
```

**说明：** 在SQL语句中，如果您使用了GROUP BY语法，则在执行SELECT语句时，只能选择GROUP BY的列或者对任意列进行聚合计算，不允许选择非GROUP BY的列。例如`* | SELECT status, request_time, COUNT(*) AS PV GROUP BY status`为非法分析语句，因为request\_time不是GROUP BY的列。

GROUP BY语法支持按照列名、别名或序号进行分组，详细说明如下表所示。

|参数|说明|
|--|--|
|列名|列名即为日志字段名称或聚合函数计算结果列，即支持按照日志字段名称（KEY）或聚合函数计算结果列进行分组。GROUP BY语法支持单列或多列。 |
|别名|按照日志字段或聚合函数计算结果的别名进行分组。您可以在字段索引中配置日志字段的别名。更多信息，请参见[列的别名](/cn.zh-CN/查询与分析/SQL分析语法与功能/列的别名.md)。 |
|序号|某列在SELECT语句中的序号（从1开始）。例如status列的序号为1，所以下面两个语句为等同关系。

-   ```
* | SELECT status, count(*) AS PV GROUP BY status
```

-   ```
* | SELECT status, count(*) AS PV GROUP BY 1
``` |
|聚合函数|GROUP BY语法常与MIN、MAX、AVG、SUM、COUNT等聚合函数搭配使用。更多信息，请参见[通用聚合函数](/cn.zh-CN/查询与分析/SQL分析语法与功能/通用聚合函数.md)。|

## 示例

-   统计不同状态码的访问次数。

    ```
    * | SELECT status, count(*) AS PV GROUP BY status
    ```

-   按照每小时的时间粒度，计算PV。

    ```
    * | SELECT count(*) AS PV , date_trunc('hour', __time__) AS time GROUP BY time ORDER BY time limit 1000                            
    ```

    \_\_time\_\_字段为日志服务中的保留字段，表示时间列。time为date\_trunc\('hour', \_\_time\_\_\)的别名。date\_trunc\(\)函数的更多信息，请参见[时间截断函数](/cn.zh-CN/查询与分析/SQL分析语法与功能/日期和时间函数.md)。

    **说明：**

    -   limit 1000表示最多获取1000行结果。如果不使用LIMIT语法，则默认最多获取100行结果。
    -   在索引配置中，当您开启任意字段的统计功能后，日志服务会自动开启\_\_time\_\_字段的统计功能。
-   按照每5分钟的时间粒度，计算PV。

    因为date\_trunc\(\)函数只能按照固定时间间隔统计。如果您需要按照自定义的时间进行统计分析，请按照数学取模方法进行分组。例如下述语句中的%300表示按照5分钟的时间粒度进行取模对齐。

    ```
    * | SELECT count(*) AS PV,  __time__ - __time__%300 AS time GROUP BY time limit 1000
    ```

-   提取未被分组（GROUP BY）的列

    在标准SQL语句中，如果您使用了GROUP BY语法，则在执行SELECT语句时，只能选择已被分组（GROUP BY）的列或者对任意列进行聚合计算，不允许选择未被分组（GROUP BY）的列。例如`* | SELECT status, request_time, COUNT(*) AS PV GROUP BY status`为非法分析语句，因为request\_time未被分组。您可以执行如下查询和分析语句：

    ```
    * | SELECT status, arbitrary(request_time), count(*) AS PV GROUP BY status
    ```


