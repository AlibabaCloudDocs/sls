# Cleanse data by using functions

The data transformation feature of Log Service allows you to cleanse raw data. You can use one or more functions to cleanse a large amount of data. This way, the logs collected to Log Service can be converted to a standard format. This topic describes how to use functions to cleanse data in various scenarios.

## Scenario 1: Filter logs by using the e\_keep function and e\_drop function

You can use the [e\_drop](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Event processing functions.md) or [e\_keep](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Event processing functions.md) function to filter logs. You can also specify the DROP parameter and use the [e\_if](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Flow control functions.mdsection_dhk_ius_2q8) or [e\_if\_else](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Flow control functions.md) function to filter logs.

The following common transformation rules can be used:

-   `e_keep(e_search(...))`: The log entries that meet the conditions are retained. Otherwise, the log entries are dropped.
-   `e_drop(e_search(...))`: The log entries that meet the conditions are dropped. Otherwise, the log entries are retained.
-   `e_if_else(e_search("..."), KEEP, DROP)`: The log entries that meet the conditions are retained. Otherwise, the log entries are dropped.
-   `e_if(e_search("not ..."), DROP)`: The log entries that meet the conditions are dropped. Otherwise, the log entries are retained.
-   `e_if(e_search("..."), KEEP)`. This transformation rule is invalid.

Example:

-   Raw log entries

    ```
    # Log entry 1
    __source__:  192.168.0.1
    __tag__:__client_ip__:  192.168.0.2
    __tag__:__receive_time__:  1597214851
    __topic__: app 
    class:  test_case
    id:  7992
    test_string:  <function test1 at 0x1027401e0>
    
    # Log entry 2
    __source__:  192.168.0.1
    class:  produce_case
    id:  7990
    test_string:  <function test1 at 0x1020401e0>
    ```

-   Transformation rule

    Drop the log entries whose \_\_topic\_\_ and \_\_tag\_\_:\_\_receive\_time\_\_ fields are empty.

    ```
    e_if(e_not_has("__topic__"),e_drop())
    e_if(e_not_has("__tag__:__receive_time__"),e_drop())
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

You can use the [e\_set](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value assignment function.md) function to assign values to empty fields in a log entry.

-   Sub-scenario 1: Assign a value to a field if the field does not exist or is empty.

    ```
    e_set("result", "......value......", mode="fill")
    ```

    For information about the mode parameter, see [Field check and overwrite modes](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).

    Example:

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

-   Sub-scenario 2: Use [Grok function](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Grok function.md) to simplify a regular expression and extract field values.

    Example:

    -   Raw log entry

        ```
        content: "ip address: 192.168.1.1"
        ```

    -   Transformation rule

        Use the Grok function to extract the IP address in the content field.

        ```
        e_regex("content", grok(r"(%{IP})"),"addr")
        ```

    -   Result

        ```
        addr:  192.168.1.1
        content: "ip address: 192.168.1.1"
        ```

-   Sub-scenario 3: Assign values to multiple fields.

    ```
    e_set("k1", "v1", "k2", "v2", "k3", "v3", ......)
    ```

    Example:

    -   Raw log entry

        ```
        __source__:  192.168.0.1
        __topic__:
        __tag__:
        __receive_time__:
        id:  7990
        test_string:  <function test1 at 0x1020401e0>
        ```

    -   Transformation rule

        Assign values to the \_\_topic\_\_, \_\_tag\_\_, and \_\_receive\_time\_\_ fields.

        ```
        e_set("__topic__","app", "__tag__","stu","__receive_time__","1597214851")
        ```

    -   Result

        ```
        __source__:  192.168.0.1
        __topic__:  app
        __tag__:  stu
        __receive_time__:  1597214851
        id:  7990
        test_string:  <function test1 at 0x1020401e0>
        ```


## Scenario 3: Delete a field and rename a field by using the e\_search, e\_rename, and e\_compose functions

We recommend that you use the [e\_compose](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Flow control functions.mdsection_zr0_ghx_vie) function to check whether the data meets the conditions and then perform operations based on the check result.

Example:

-   Raw log entry

    ```
    content: 123
    age: 23
    name: twiss
    ```

-   Transformation rule

    If the value of the content field is 123, delete the age and name fields. Then, rename the content field to ctx.

    ```
    e_if(e_search("content==123"),e_compose(e_drop_fields("age|name"), e_rename("content", "ctx")))
    ```

-   Result

    ```
    ctx: 123
    ```


## Scenario 4: Convert the type of fields in a log entry by using the v, cn\_int, and dt\_totimestamp functions

The fields and values in log entries are processed as strings during the data transformation process. Data of a non-string type is automatically converted to data of the string type. Therefore, you must be familiar with the types of fields that a function can receive when you invoke the function. For more information, see [Syntax introduction](/intl.en-US/Data Transformation/Data processing syntax/Syntax introduction.md).

-   Sub-scenario 1: Use the [op\_add](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Operator functions.md) function to concatenate strings or add numbers.

    The op\_add function can receive data of both the string and numeric types. Therefore, no field type needs to be converted.

    Example:

    -   Raw log entry

        ```
        a : 1
        b : 2
        ```

    -   Transformation rule

        ```
        e_set("d",op_add(v("a"), v("b")))
        e_set("e",op_add(ct_int(v("a")), ct_int(v("b"))))
        ```

    -   Result

        ```
        a : 12
        b : 13
        ```

-   Sub-scenario 2: Use the [v](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Event check functions.md) function and [ct\_int](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Conversion functions.md) function to convert data types and use the [op\_mul](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Arithmetic functions.md) function to multiply data.

    Example:

    -   Raw log entry

        ```
        a:2
        b:5
        ```

    -   Transformation rule

        The values of both v\("a"\) and v\("b"\) are of the string type. However, the second field of the op\_mul function can receive only numeric values. Therefore, you must use the ct\_int function to convert a string to an integer, and then pass the value to the op\_mul function.

        ```
        e_set("c",op_mul(ct_int(v("a")), ct_int(v("b"))))
        e_set("d",op_mul(v("a"), ct_int(v("b"))))
        ```

    -   Result

        ```
        a: 2
        b: 5
        c: 10
        d: 22222
        ```

-   Sub-scenario 3: Use the [dt\_parse](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Date and time functions.mdsection_yug_wml_n2u) function or [dt\_parsetimestamp](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Date and time functions.md) function to convert a string or datetime object to standard time.

    The dt\_totimestamp function receives only datetime objects. Therefore, you must use the dt\_parse function to convert the string value of time1 to a datetime object. You can also use the dt\_parsetimestamp function because it can receive both datetime objects and strings. For more information, see [Date and time functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Date and time functions.md).

    Example:

    -   Raw log entry

        ```
        time1: 2020-09-17 9:00:00
        ```

    -   Transformation rule

        Convert the time indicated by time1 to a UNIX timestamp.

        ```
        e_set("time1", "2019-06-03 2:41:26")
        e_set("time2", dt_totimestamp(dt_parse(v("time1")))) or e_set("time2", dt_parsetimestamp(v("time1"))) 
        ```

    -   Result:

        ```
        time1:  2019-06-03 2:41:26 
        time2:  1559529686 
        ```


## Scenario 5: Fill the default values in log fields that do not exist by specifying the default parameter

Some expression functions that are used to transform data in Log Service have specific requirements for input parameters. If the input parameters do not meet the requirements, the data transformation rule returns the default values or an error. If a necessary log field is incomplete, you can fill the default value in the log field by using the [op\_len](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Operator functions.md) function.

**Note:** If default values are passed to subsequent functions, errors may occur. We recommend that you resolve the exceptions returned by the data transformation rules at the earliest opportunity.

-   Raw log entry

    ```
    data_len: 1024
    ```

-   Transformation rule

    ```
    e_set("data_len", op_len(v("data", default="")))
    ```

-   Result

    ```
    data: 0
    data_len: 0
    ```


## Scenario 6: Add one or more fields based on conditions by using the e\_if and e\_switch functions

We recommend that you use the [e\_if](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Flow control functions.mdsection_dhk_ius_2q8) or [e\_switch](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Flow control functions.md) function to add one or more fields to log entries based on specified conditions. For more information, see [Flow control functions](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Flow control functions.md).

-   e\_if function syntax

    ```
    e_if(condition 1, operation 1, condition 2, operation 2, condition 3, operation 3, ...)
    ```

-   e\_switch function syntax

    When you use the e\_switch function, you must specify condition-operation pairs. The conditions are checked in sequence. An operation is performed only after its paired condition is met. After a condition is met, the corresponding operation result is returned and no more conditions are checked. If a condition is not met, its paired operation is not performed and the next condition is checked. If no conditions are met and the default field is specified, the operation that is specified by default is performed and the corresponding result is returned.

    ```
    e_switch(condition 1, operation 1, condition 2, operation 2, condition 3, operation 3, ..., default=None)
    ```


Example:

-   Raw log entry

    ```
    status1: 200
    status2: 404
    ```

-   e\_if function
    -   Transformation rule

        ```
        e_if(e_match("status1", "200"), e_set("status1_info", "normal"),
             e_match("status2", "404"), e_set("status2_info", "error"))
        ```

    -   Result

        ```
        status1: 200
        status2: 404
        status1_info: normal
        status2_info: error
        ```

-   e\_switch function
    -   Transformation rule

        ```
        e_switch(e_match("status1", "200"), e_set("status1_info", "normal"), 
                 e_match("status2", "404"), e_set("status2_info", "error"))
        ```

    -   Result

        The e\_switch function checks the conditions in sequence. After a condition is met, the corresponding operation result is returned and no more conditions are checked.

        ```
        status1: 200
        status2: 404
        status1_info: normal
        ```


