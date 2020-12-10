# ORDER BY syntax

You can use the ORDER BY syntax to sort output results. You can sort the output results by only one column.

## Syntax:

```
order by column name [desc|asc]
```

## Example:

```
method :PostLogstoreLogs |select avg(latency) as avg_latency,projectName group by projectName 
HAVING avg(latency) > 5700000
order by avg_latency desc
```

