# ORDER BY语法

ORDER BY语法用于对输出结果进行排序，目前只支持按照一列进行排序。

## 语法格式

```
order by 列名 [desc|asc]
```

## 示例

```
method :PostLogstoreLogs |select avg(latency) as avg_latency,projectName group by projectName 
HAVING avg(latency) > 5700000
order by avg_latency desc
```

