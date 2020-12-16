# How can I fix errors related to data transformation rules?

This topic describes the causes of errors that are related to data transformation rules and provides solutions.

After the data transformation engine reads data from a source Logstore, the engine starts to transform the log events of the Logstore.

![Errors related to data transformation rules](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1553749951/p59311.png)

-   During the transformation process, logic errors may occur because the transformation rules may not apply to all log events.
-   If the transformation rules involve data loading from external resources such as ApsaraDB RDS or Logstores, errors may also occur during the data loading or updating process.

This topic describes how to resolve logic errors. For information about how to resolve resource loading errors, see [How can I fix data pull errors?](/intl.en-US/Data Transformation/FAQ/How can I fix data pull errors?.md).

## Error impact

During the transformation process, log events that conflict with the transformation rules cause errors. These errors are divided into WARNING and ERROR levels. The error levels are identified by the logging.levelname field.

-   For ERROR-level errors, the relevant log events are discarded. The transformation process continues and no retry is performed. The transformation result does not contain these log events.
-   For WARNING-level errors, data is not transformed in the current domain-specific language \(DSL\). For example, if the log events do not match the specified regular expression, the next step is performed.

## Solution

-   Check the `logging.levelname` field of the error logs to determine the error levels.
-   Check the `message` field to locate the error log events. For more information, see [View error logs](/intl.en-US/Data Transformation/FAQ/Troubleshooting overview.md).
-   Check the `reason` field of the error logs to identify the error causes.

Add logic to transform these error log events based on the error causes. You can use process control functions, such as the `e_if` and `e_switch` functions to identify and resolve the errors.

![Error log](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1553749951/p59312.png)

## Error handling examples

-   Log events contain abnormal values.

    Sample transformation rule 1:

    ```
    # The value of the b field in some log events is 0. This value is used as a divisor of a numeric operation and causes an error.
    e_set("c", op_div_floor(v("a"), v("b")))
    ```

    -   Error log:

        ```
        {
          "reason": "error when calling : floordiv\nDetail: integer division or modulo by zero", 
        }
        ```

    -   Troubleshooting method:

        Check whether the value of the `b` field is 0 in the error logs. If the value of the b field is 0, this value is used as a divisor of a numeric operation and causes an error.

    -   Solution:

        The error occurs only when the value of the `b` field is 0. In this case, you can use the `e_if` function to capture the `b` field whose value is 0.

        ```
        e_if_else(op_eq(v("b"), "0"), e_set("c", v("a")), e_set("c", op_div_floor(v("a"), v("b")))
        ```

    Sample transformation rule 2:

    ```
    # The value of the a field in some log events is an invalid timestamp, which causes an error.
    e_set("b", dt_fromtimestamp(v("a")))
    ```

    -   Error log:

        ```
        {
          "reason": "error when calling : int\nDetail: invalid literal for int() with base 10: 'invalid value'", 
        }
        ```

    -   Troubleshooting method:

        Check whether the value of the `a` field is a valid timestamp \(numeric string\).

    -   Solution:

        Add logic to check whether the value of the `a` field is a valid timestamp. If not, export the log event to `target2`.

        ```
        e_if_else(str_isdigit(v("a")))，e_set("b", dt_fromtimestamp(v("a")))， e_output("target2"))
        ```

-   The data type is not converted before a numeric operation.

    Sample transformation rule:

    ```
    e_set("a", 10)
    e_set("b", 10)
    e_set("c", op_mul(v("a"), v("b")))
    ```

    -   Error log:

        ```
        {
          "reason": "error when calling : mul\nDetail: can't mulltiply sequence by non-int of type' str'", 
        }
        ```

    -   Error cause:

        The values of all fields in log events are stored as strings during the DSL transformation process. In the preceding sample transformation rule, the values of `v("a")` and `v("b")` are strings. If they are directly passed to the `op_mul` function, an error occurs.

    -   Troubleshooting method:

        Check the DSL rules to determine whether the data type is converted before the numeric operation.

    -   Solution:

        Use the `ct_int` function to convert the values from the string type to the integer type, and then pass them to the `op_mul` function.

        ```
        e_set("a", 10)
        e_set("b", 10)
        e_set("c", op_mul(c_int(v("a")), c_int(v("b"))))
        ```


