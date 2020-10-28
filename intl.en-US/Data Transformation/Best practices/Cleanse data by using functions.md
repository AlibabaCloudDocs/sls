# Cleanse data by using functions

The data transformation feature of Log Service allows you to cleanse raw data. You can use one or more functions to cleanse a large amount of data. This way, the logs collected to Log Service can be in a standard format. This topic describes how to use functions to cleanse data in various scenarios.

## Scenario 1: Filter logs by using the e\_keep function and e\_drop function

-   Solution

    For more information, see [Event processing functions](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Event processing functions.md).

    -   To discard log entries, you can use the e\_drop function or e\_keep function. You can also use the e\_if function or e\_if\_else function in combination with the DROP parameter.
    -   By default, log entries that are not processed are retained. Therefore, the KEEP parameter is used only in specific scenarios.
    -   Common functions and their differences
        -   `e_keep(e_search(...) )`. The log entries that meet the conditions are retained. Otherwise, the logs are discarded.
        -   `e_drop(e_search(...) )`. The log entries that meet the conditions are discarded. Otherwise, the logs are retained.
        -   `e_if(e_search("..."), KEEP)`. This transformation rule is meaningless.
        -   `e_if_else(e_search("..."), KEEP, DROP)`. This transformation rule is valid.
        -   `e_if(e_search("not ..."), DROP)`. This transformation rule is valid.
-   Example
    -   Raw log entries

        ```
        Log 1
        __source__:  192.168.0.1
        __tag__:__client_ip__:  192.168.0.2
        __tag__:__receive_time__:  1597214851
        __topic__: app 
        class:  test_case
        id:  7992
        test_string:  <function test1 at 0x1027401e0>
        Log 2
        __source__:  192.168.0.1
        class:  produce_case
        id:  7990
        test_string:  <function test1 at 0x1020401e0>
        ```

    -   Transformation rule

        Filter out invalid log entries that do not contain the topic or tag\_\_receive\_time\_\_ fields.

        ```
        e_if(e_not_has("topic"),e_drop())
        e_if(e_not_has("__tag___receive_time__"),e_drop())
        ```

    -   Result

        ```
        __source__:  192.168.0.1
        __tag__:__client_ip__:  192.168.0.2
        __tag__:__receive_time__:  1597214851
        __topic__: app 
        class:  test_case
        id:  7992
        test_string:  <function test1 at 0x1027401e0>
        ```


## Scenario 2: Assign values to empty fields in a log entry by using the e\_set function

-   Sub-scenario 1: Assign a value to a field if the field does not exist or is empty.
    -   Solution

        For information about how to extract and overwrite a field, see [Field check and overwrite modes](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).

        -   Recommended transformation rule

            ```
            e_set("result", "......value......", mode="fill")
            ```

        -   Transformation rule that is not recommended

            ```
            e_if(op_not(v("result")), e_set("result", "......value......"))
            ```

    -   Example
        -   Raw log entry

            ```
            name:
            ```

        -   Transformation rule

            ```
            e_set("name", "aspara2.0", mode="fill")
            ```

        -   Result

            ```
            name:  aspara2.0
            ```

-   Sub-scenario 2: Use the Grok function to simplify the field value that is extracted by regular expressions.
    -   Solution

        Use the Grok function to extract the IP address in the content field. For more information about the Grok function, see [Grok function](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Grok function.md).

    -   Example
        -   Raw log entry

            ```
            content: "ip address: 192.168.1.1"
            ```

        -   Recommended transformation rule

            ```
            e_regex("content", grok(r"(%{IP})"),"addr")
            ```

        -   Transformation rule that is not recommended

            ```
            e_regex("content", grok(r"\w+: (\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})"), "addr")
            ```

        -   Result

            ```
            addr:  192.168.1.1
            content: "ip address: 192.168.1.1"
            ```

-   Sub-scenario 3: Assign values to multiple fields.
    -   Solution
        -   Recommended transformation rule

            ```
            e_set("k1", "v1", "k2", "v2", "k3", "v3", ......)
            ```

        -   Transformation rule that is not recommended

            ```
            e_set("k1", "v1")
            e_set("k2", "v2")
            e_set("k3", "v3")
            ......
            ```

    -   Example
        -   Raw log entry

            ```
            __source__:  192.168.0.1
            class:  produce_case
            topic:
            _tag_:
            _receive_time_:
            id:  7990
            test_string:  <function test1 at 0x1020401e0>
            ```

        -   Transformation rule

            ```
            e_set("topic","app", "_tag__","stu","__receive_time__","1597214851")
            ```

        -   Result

            ```
            __source__:  192.168.0.1
            class:  produce_case
            topic:  app
            _tag_:  stu
            _receive_time_:  1597214851
            id:  7990
            test_string:  <function test1 at 0x1020401e0>
            ```


## Scenario 3: Delete a field and rename a field by using the e\_search function, e\_rename function, and e\_compose function

-   Solution

    We recommend that you use the e\_compose function to check whether the data meet the conditions and then perform operations based on the check result. For more information, see [e\_compose](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Flow control functions.mdsection_zr0_ghx_vie).

-   Example
    -   Raw log entry

        ```
        content: 123
        age: 23
        name: twiss
        ```

        If the value of the content field is 123, delete the age and name fields, and then rename the content field as ctx.

    -   Recommended transformation rule

        ```
        e_if(e_search("content==123"),e_compose(e_drop_fields("age|name"), e_rename("content", "ctx")))
        ```

    -   Transformation rule that is not recommended

        The following code performs multiple checks. This reduces the efficiency of the transformation rule.

        ```
        e_if(e_search("content==123"), e_drop_fields("age|name"))
        e_if(e_search("content==123"), e_rename("content", "ctx"))
        ```

    -   Result

        ```
        ctx: 123
        ```


## Scenario 4: Convert the type of fields in a log entry by using the v function, cn\_int function, and dt\_totimestamp function

The fields and values in log entries are passed to functions as strings. Data of a non-string type is automatically converted to data of the string type. Therefore, you must be familiar with the types of fields that a function can receive when you invoke the function. For more information, see [Syntax introduction](/intl.en-US/Data Transformation/Data processing syntax/Syntax introduction.md).

-   Sub-scenario 1: Use the op\_add function to concatenate strings or add numbers.
    -   Solution

        The op\_add function can receive data of both the string and numeric types. Therefore, no field type needs to be converted.

    -   Example
        -   Raw log entries

            ```
            a:  1024
            b:  2048
            ```

        -   Transformation rule

            ```
            e_set("a", 1)
            e_set("b", 2)
            op_add(v("a"), v("b"))
            op_add(ct_int(v("a")), ct_int(v("b")))
            ```

        -   Result

            ```
            a: 12 # Valid
            b: 3 # Valid
            ```

-   Sub-scenario 2: Use the v function and ct\_int function to convert data types and use the op\_mul function to multiply data.
    -   Solution

        The second field of the op\_mul function can receive only numeric values. Therefore, you must convert a value of the string type to a value of the numeric type for the second field.

    -   Example
        -   Raw log entries

            ```
            a: 1024
            b: 2048
            ```

        -   Recommended transformation rule

            When the field values in log entries are passed to functions, they are automatically converted to the values of the string type. Therefore, the values of both v\("a"\) and v\("b"\) are of the string type. However, the second field of the op\_mul can receive only numeric values. In this case, you must use the ct\_int function to convert a string to an integer, and then pass the value to other functions.

            ```
            e_set("a", 2)
            e_set("b", 5)
            op_mul("c",ct_int(v("a"),ct_int(v("b")))
            op_mul("d",v("a"),ct_int(v("b")))
            ```

        -   Transformation rule that is not recommended

            ```
            e_set("a", 2)
            e_set("b", 5) 
            op_mul(v("a"),v("b")) # An error will occur because an invalid parameter is passed. The second parameter must be set to a numeric value.
            ```

        -   Results
            -   Result of the valid rule

                ```
                a: 2
                b: 5
                c: 10
                d: 22222
                ```

            -   Result of the invalid rule

                ```
                __ERROR__:  err when process dsl , line number:3   
                ```

-   Sub-scenario 3: Use the dt\_parse function or dt\_parsetimestamp function to convert a string or datetime object to standard time.
    -   Solution
        -   The dt\_totimestamp function receives only datetime objects. Therefore, you must use the dt\_parse function to convert the string value of time1 to a datetime object.
        -   You can also use the dt\_parsetimestamp function because it can receive both datetime objects and strings.
        -   For more information, see [Date and time functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Date and time functions.md).
    -   Example
        -   Raw log entry

            ```
            time1: 2020-09-17 9:00:00
            ```

        -   Recommended transformation rule

            Convert the time indicated by time1 to a UNIX timestamp.

            ```
            e_set("time1", "2019-06-03 2:41:26")
            e_set("time2", dt_totimestamp(dt_parse(v("time1")))) # This transformation rule is valid.
            e_set("time2", dt_parsetimestamp(v("time1"))) # This transformation rule is invalid.
            ```

        -   Transformation rule that is not recommended

            ```
            e_set("time1", "2019-06-03 2:41:26")
            e_set("time2", dt_totimestamp(v("time1"))) # This transformation rule is invalid.
            ```

        -   Results
            -   Result of the valid rule

                ```
                time1:  2019-06-03 2:41:26 
                time2:  1559529686 
                ```

            -   Result of the invalid rule

                ```
                __ERROR__:  err when process dsl , line number:2   
                ```


## Scenario 5: Fill the default values in log fields that do not exist by specifying the default parameter

Many expressions functions that are used to transform data in Log Service have specific requirements for input parameters. If the input parameters do not meet the requirements, the data transformation rule returns the preset responses or an error. We recommend that you fill the default values in the necessary but incomplete log fields.

**Note:** If these preset responses are passed to subsequent functions, errors may occur. We recommend that you handle the exceptions returned by the data transformation rules at the earliest opportunity.

-   Raw log entry

    ```
    data_len: 1024
    ```

-   Recommended transformation rule

    Errors may occur if the preset responses are passed to subsequent functions.

    ```
    e_set("data_len", op_len(v("data", default=""))) # This transformation rule is valid.
    ```

-   Transformation rule that is not recommended

    ```
    e_set("data_len", op_len(v("data"))) # This transformation rule is invalid.
    ```

    If the data field does not exist, the `v("data")` function returns `None`, and an error is returned by the `e_set("data_len", op_len(v("data")))` function. If you use the `e_set("data_len", op_len(v("data", default="")))` function, the data field receives a default value that is specified by the `default` parameter. Then, the function runs as expected. For more information about the op\_len function, see [Operator functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Operator functions.md).

-   Results
    -   Result of the valid rule

        ```
        data: 0
        data_len: 0
        ```

    -   Result of the invalid rule

        ```
         __ERROR__:  err when process dsl , line number:1
          data_len:    
        ```


## Scenario 6: Add one or more fields based on conditions by using the e\_if function and e\_switch function

-   Solution

    We recommend that you use the e\_if and e\_switch functions to add one or more fields to log entries. For more information, see [Flow control functions](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Flow control functions.md).

    -   e\_if function syntax

        ```
        e_if(condition 1, operation 1, condition 2, operation 2, condition 3, operation 3, ...)
        ```

    -   e\_switch function syntax

        You must specify the condition-operation pairs. Conditions are checked in sequence for condition-based judgments. An operation is performed only after its paired condition is met. After a condition is met, the corresponding operation result is returned and no further condition-based judgment will be triggered. If a condition is not met, its paired operation is not performed and the next condition-based judgment is triggered. If no conditions are met and the `default` field is specified, the operation that is specified by `default` is performed and the corresponding result is returned.

        ```
        e_switch(condition 1, operation 1, condition 2, operation 2, condition 3, operation 3, ..., default=None)
        ```

-   Example

-   Raw log entries

    ```
    status1: 200
    status2: 404
    ```

-   e\_if function

    ```
    e_if(e_match("status1", "200"), e_set("status1_info", "normal"),
         e_match("status2", "404"), e_set("status2_info", "error"))
    ```

-   Result of the e\_if function

    The e\_if function checks all the conditions. An operation is performed only after its paired condition is met.

    ```
    status1: 200
    status2: 404
    status1_info: normal
    status2_info: error
    ```

-   e\_switch function

    ```
    e_switch(e_match("status1", "200"), e_set("status1_info", "normal"), 
             e_match("status2", "404"), e_set("status2_info", "error"))
    ```

-   Result of the e\_switch function

    The e\_switch function checks the conditions in sequence. After a condition is met, the corresponding operation result is returned and no further condition-based judgment will be triggered.

    ```
    status1: 200
    status2: 404
    status1_info: normal
    ```


