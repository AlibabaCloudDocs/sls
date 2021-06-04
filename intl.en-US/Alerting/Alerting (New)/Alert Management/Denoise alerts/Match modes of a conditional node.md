# Match modes of a conditional node

When you configure alert policies, action policies, and notification method quotas, you can add a conditional node. If alerts in an alert set meet the specified conditions, alerts are processed based on the specified action.

## Basic logic

If you specify multiple values, the values are associated by the OR operator. The following configuration indicates that a specified action is implemented if the alert is ingested or the alert is triggered by an alert monitoring rule.

![Basic logic](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8239872261/p264365.png)

## Operator

You can use regular expressions or specify a value range when you configure a conditional node.

-   Regular expressions: Use a regular expression to specify match conditions.

    ![Regular expression](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8239872261/p265200.png)

-   Value range match: Specify match conditions by comparing numeric values. For example, you can use the operators such as equal to \(=\) or equal to or greater than \(\>=\).

    ![Numeric values](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9239872261/p265201.png)


## Mode

You can add multiple conditions in standard mode or advanced mode.

-   Standard mode: If you specify multiple conditions, the conditions are associated by the AND operator.

    ![Standard mode](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9239872261/p264368.png)

-   Advanced mode: If you specify multiple conditions, you can use the AND or the OR operator to associate the conditions. You can also group multiple conditions into one group by using parentheses.

    ![Advanced mode](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9239872261/p264371.png)


