# Use an evaluate expression to specify a trigger condition

This topic describes how to specify a trigger condition by using an evaluate expression. It also describes the related notes and limits.

Log Service allows you to use an evaluate expression when you specify the trigger condition in an alert monitoring rule. For example, the **Trigger Condition** parameter is set to **data matches the expression**, an evaluate expression. If the data of query results meets the specified evaluate expression, an alert is triggered. You can use variables and logical operators in an evaluate expression. For information about the syntax of evaluate expressions, see [Syntax of evaluate expressions](/intl.en-US/Alerting/Alerting (New)/Alert monitoring/Orchestration of monitoring rules/Syntax of evaluate expressions.md).

## Variables

**Note:** The result of set operations that are performed on query results are evaluated. You can perform the CROSS JOIN and LEFT JOIN operations.

-   If multiple query results do not contain duplicate fields, you can quote specific fields. In this case, you do not need to add prefixes such as $0, $1, or $2, to the fields in an evaluate expression. Example: `name == 'sls'`.
-   If multiple query results contain duplicate fields, you can quote specific fields. In this case, you must add prefixes such as $0, $1, or $2, to the fields in an evaluate expression. Example: `$0.name == 'sls' || $1.name == 'ecs'`.

    For more information, see [Multi-set operations](/intl.en-US/Alerting/Alerting (New)/Alert monitoring/Associated monitoring/Multi-set operations.md).


|Data type|Query statement|Description|
|---------|---------------|-----------|
|Logs|A statement that contains no SELECT clause|Only a search statement is used to query logs. The fields in the query result can be quoted when you specify an evaluate expression for a trigger condition. |
|A query statement that contains SELECT \* FROM log|All the fields whose Enable Analytics switch is turned on can be quoted when you specify an evaluate expression for a trigger condition.|
|A query statement that contains SELECT \* FROM \(SELECT...\)|The fields in the specified SELECT subquery can be quoted when you specify an evaluate expression for a trigger condition.|
|Time series data|A query statement that contains SELECT promql\(...\)|Fixed fields can be returned by using the promql\_query\(string\) or promql\_query\_range\(string, string\) function. The fixed fields include the metric, labels, time, and value fields. Log Service expands the labels field in the map format. The metric, labels, time, and value fields can be quoted when you specify an evaluate expression for a trigger condition. You can also quote the subfields of the labels field. |
|A query statement that contains SELECT a, b FROM \(SELECT promql\(...\)\)|All the fields whose Enable Analytics switch is turned on can be quoted when you specify an evaluate expression for a trigger condition.|
|A query statement that contains SELECT \* FROM log|All the fields whose Enable Analytics switch is turned on can be quoted when you specify an evaluate expression for a trigger condition.|
|Resource data|None|The value of a field in resource data can be a string, floating-point number, or numeric value. The JSON type is not supported. The ID of a field in resource data can be quoted when you specify an evaluate expression for a trigger condition. |

## Examples

-   Example 1: If the success rate of a task is lower than 90% and the delay exceeds 60 seconds within 1 day \(relative\), an alert is triggered.

    Set the **Trigger Condition** parameter to **data matches the expression**,**success < 90 && delay \> 60**.

    ![Trigger Condition](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7624542261/p265425.png)

-   Example 2: If the number of HTTP status code 500 error responses within 15 minutes exceeds 10, an alert is triggered.

    Set the **Trigger Condition** parameter to **data matches the expression**,**status == 500 && total \> 10**.

    ![Example](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6308812261/p265434.png)


