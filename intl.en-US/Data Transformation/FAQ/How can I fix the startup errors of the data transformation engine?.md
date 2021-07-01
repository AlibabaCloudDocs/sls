# How can I fix the startup errors of the data transformation engine?

This topic describes the causes of the startup errors that are related to the data transformation engine. This topic also provides methods that you can use to troubleshoot these errors.

Before Log Service runs a data transformation task based on a data transformation rule, the data transformation engine must be started first. If domain-specific language \(DSL\) rules fail the security check in the data transformation engine, errors may occur in this step.

![Start the data transformation engine](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9453749951/p59300.png)

## Error log

If invalid Log Service DSL rules are detected during the startup process of the transformation engine, the following error message is returned:

```
{
  "errorMessage": "ETL config doesn't pass security check, detail: XXXXXX"
}
```

**Note:** You can view error logs in the exception details of the diagnostic report for data transformation or in the internal-etl-log Logstore.

-   If an error occurs during the transformation engine startup phase, Log Service retries the data transformation task until a retry is successful or until you manually stop retries.
-   After you modify the data transformation rule and a retry is successful, the data transformation engine works as expected. No data is dropped or duplicated.

## Troubleshoot common errors

-   The basic syntax is invalid.

    The compiled data transformation rules do not conform to the Log Service DSL syntax. For example, the rules contain unpaired parentheses \(\), or commas \(,\) are incorrectly written as colons \(:\).

    -   Error logs

        ```
        {
          "errorMessage": "ETL config doesn't pass security check, detail: invalid syntax"
        }
        {
          "errorMessage": "ETL config doesn't pass security check, detail: unexpected EOF while parsing"
        }
        ...
        ```

    -   Troubleshooting method

        Locate the specific syntax error based on the `traceback` information in the related error log. For example, `e_set("test", v("status"))` is incorrectly written as `e_set("test": v("status"))`, as shown in the following figure.

        ![Invalid syntax](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0553749951/p59301.png)

-   Invalid operators are used.

    All operations in Log Service DSL must be specified by using the functions supported by Log Service DSL. For example, numerical operations or size comparisons must be specified by using the op\_\* function. You cannot use only operators.

    -   Error log

        ```
        {
          "errorMessage": "ETL config doesn't pass security check, detail: invalid type detected: <class `_ast.BinOp`> "
        }
        ```

    -   Troubleshooting method

        Check Log Service DSL rules. Make sure that all operations such as numerical operations and size comparisons are specified by using the functions supported by Log Service DSL.

    -   Examples

        ```
        e_set("b", v("a") - 10) # Invalid example
        e_set("b", op_sub(v("a"), 10)) # Valid example
        
        e_set("b", v("a") >= v("c")) # Invalid example
        e_set("b", op_ge(v("a"), v("c"))) # Valid example
        ```

-   If the type of the parameter that is passed to a function is invalid or the called function does not exist, an error occurs.

    If the type of the parameter that is passed to a function is different from the type of the parameter that is received by the function or the called function does not exist, an error occurs.

    -   Error log

        ```
        {
          "errorMessage": "ETL config doesn't pass security check, detail: invalid call in detected: function_name"
        }
        ```

    -   Troubleshooting method
        -   Check whether the called function exists and the function name is correct. If the function exists and the name is valid, check whether the type of the parameter that is passed to the function is valid.
        -   Locate the error function based on the `traceback` information in the related error log. For example, if the returned error log indicates that the dt\_totimestamp function is invalid, you must check whether the function exists, and then check whether the type of the parameter that is passed to the dt\_totimestamp function is valid.

            ![Function error](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0553749951/p59302.png)

    -   Examples

        The type of the parameter received by the dt\_totimestamp function is a datetime object, and `v("time1")` in the code is a string. An error occurs because the type of the parameter that is passed to the function is invalid.

        To fix the error, use the dt\_parse function to convert the string to a datetime object before the parameter is passed to the dt\_totimestamp function. You can also use the dt\_parsetimestamp function that can receive strings instead of the dt\_totimestamp function.

        ```
        # Invalid examples
        e_set("time1", "2019-06-03 2:41:26")
        e_set("time2", dt_totimestamp(v("time1")))
        
        # Valid examples
        e_set("time1", "2019-06-03 2:41:26")
        e_set("time2", dt_totimestamp(dt_parse(v("time1"))))
        
        # Valid examples
        e_set("time1", "2019-06-03 2:41:26")
        e_set("time2", dt_parsetimestamp(v("time1")))
        ```

-   Expression functions are globally called.

    The Log Service DSL syntax supports two types of functions: global operation functions and expression functions. Only global operation functions can be globally called in the data transformation process. If an expression function is globally called, an error occurs.

    -   Error log

        ```
        {
          "errorMessage": "ETL config doesn't pass security check, detail:  invalid type detected: <class '_ast.Expr'>"
        }
        ```

    -   Troubleshooting method

        Check whether an expression function is globally called in the data transformation process.

    -   Examples

        ```
        # Invalid examples
        op_add(v("a"), v("b"))
        str_lower(v("name"))
        
        # Valid examples
        e_set("add", op_add(v("a"), v("b")))
        e_set("lower", str_lower(v("name")))
        ```

-   Parameters are specified by using variable values.

    The Log Service DSL syntax does not support value assignment by using variables. Variable values can only be passed in stateless mode.

    -   Error log

        ```
        {
          "errorMessage": "ETL config doesn't pass security check, detail: invalid assign detected: variable_name"
        }
        ```

    -   Troubleshooting method
        -   Check whether variables are used to assign values in the Log Service DSL rule.
        -   Locate the error based on the `traceback` information in the related error log.
    -   Examples

        ```
        # Invalid examples
        sum_value = op_add(v("a"), v("b"))
        e_set("sum", sum_value)
        
        # Valid example
        e_set("sum", op_add(v("a"), v("b")))
        ```


