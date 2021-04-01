# Flow control functions

This topic describes the syntax and parameters of flow control functions. This topic also provides several examples of flow control functions.

## Functions

|Function|Description|
|--------|-----------|
|[e\_compose](#section_zr0_ghx_vie)|Combines multiple operations. -   This function is used to combine multiple operations in the e\_if, e\_switch, or e\_if\_else function.
-   The specified operations are performed on a log entry in sequence and a result is returned.
-   If an operation is performed to delete a log entry, no other operations can be performed on the log entry. |
|[e\_if](#section_dhk_ius_2q8)|Performs an operation if a condition is met. Multiple condition-operation pairs can be specified. -   If a condition is met, the paired operation is performed. If the condition is not met, the operation is not performed and the next condition is evaluated.
-   If an operation is performed to delete a log entry, no other operations are performed on the log entry. |
|[e\_if\_else](#section_6dy_m0v_hig)|Performs an operation based on the evaluation result of a condition.|
|[e\_switch](#section_f1t_ukb_ilk)|Performs an operation if a condition is met. Multiple condition-operation pairs can be specified. -   If a condition is met, the paired operation is performed and a result is returned. If the condition is not met, the operation is not performed and the next condition is evaluated.
-   If no specified conditions are met and the default parameter is specified, the default operations specified in the default parameter are performed and a result is returned.
-   If an operation is performed to delete a log entry, no other operations are performed on the log entry. |

## e\_compose

-   Syntax

    ```
    e_compose(Operation 1, Operation 2, ...)     
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Operation 1|Global processing function|Yes|One or more global processing functions.|
    |Operation 2|Global processing function|No|One or more global processing functions.|

-   Response

    A log entry on which the specified operations are performed is returned.

-   Example: If the value of the `content` field is 123, the `age` and `name` fields are deleted and the `content` field is set to ctx.

    Raw log entry:

    ```
    content: 123
    age: 23
    name: twiss
    ```

    Transformation rule:

    ```
    e_if(e_search("content==123"), e_compose(e_drop_fields("age|name"), e_rename("content", "ctx")))
    ```

    Result:

    ```
    ctx:  123
    ```


## e\_if

-   Syntax

    ```
    e_if(Condition, Operation)
    e_if(Condition 1, Operation 1, Condition 2, Operation 2, ...)
    ```

    **Note:** The `Condition` and `Operation` parameters must be used in pairs.

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Condition|Arbitrary|Yes|One or more expressions. If the result is not a Boolean value, the system evaluates whether the condition is true or false.|
    |Operation|Global processing function|No|One or more global processing functions.|

-   Response

    A log entry on which the specified operations are performed is returned.

-   Examples
    -   Example 1: If the value of a specified field meets a condition, the paired operation is performed.

        If the value of the `result` field is `failed` or `failure`, the topic field is set to `login_failed_event`.

        ```
        e_if(e_match("result", r"failed|failure"), e_set("__topic__", "login_failed_event"))
        ```

    -   Example 2: If the value of a specified field meets a condition, the value is extracted and the paired operation is performed.

        If the `request_body` field exists and the value of the field is not null, the field processing function "e\_json" is used to expand the `request_body` field.

        ```
        e_if(v("request_body"), e_json("request_body"))
        ```

    -   Example 3: If the value of a specified field meets a combined condition, the operation is performed.

        If the value of the `valid` field is failed, the log entry is deleted.

        ```
        e_if(op_eq(str_lower(v("valid")), "failed"), DROP)
        ```

    -   Example 4: Multiple operations are performed in sequence based on the specified conditions.

        ```
        e_if(True, e_set("__topic__", "default_login"), 
             e_match("valid", "failed"), e_set("__topic__", "login_failed_event")
          )
        ```


## e\_if\_else

-   Syntax

    ```
    e_if_else(Condition, Operation 1 if Condition is evaluated to be true, Operation 2 if Condition is evaluated to be false)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Condition|Arbitrary|Yes|One or more expressions. If the result is not a Boolean value, the system evaluates whether the condition is true or false.|
    |Operation 1 if Condition is evaluated to be true|Global processing function|Yes|One or more global processing functions.|
    |Operation 2 if Condition is evaluated to be false|Global processing function|Yes|One or more global processing functions.|

-   Response

    A log entry on which an operation is performed based on the evaluation result of a condition is returned.

-   Example: If the value of the `result` field is ok or pass, or the value of the `status` field is 200, the log entry is retained. Otherwise, the log entry is deleted.

    Raw log entries:

    ```
    result: Ok
    status: 400
    ```

    ```
    result: Pass
    status: 200
    ```

    ```
    result: failure
    status: 500
    ```

    Transformation rule:

    ```
    e_if_else(op_or(e_match("result", r"(? i)ok|pass"), e_search("status== 200")), KEEP, DROP)
    ```

    Result: Only the first two log entries are retained.

    ```
    result: Ok
    status: 400
    ```

    ```
    result: Pass
    status: 200
    ```


## e\_switch

-   Syntax

    ```
    e_switch(Condition 1, Operation 1, ..., default=None)
    ```

    **Note:** The `Condition` and `Operation` parameters must be used in pairs.

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Condition|Arbitrary|Yes|One or more expressions. If the result is not a Boolean value, the system evaluates whether the condition is true or false.|
    |Operation|Global processing function|Yes|One or more global processing functions.|
    |default|Global processing function|No|One or more default global processing functions. If no specified conditions are met and the default parameter is specified, the default operations specified in the default parameter are performed.|

-   Response

    A log entry on which the specified operations are performed is returned.

-   Examples
    -   If the value of the `content` field is 123, the `__topic__` field is set to Number. If the value of the `data` field is 123, the `__topic__` field is set to PRO.

        Raw log entries:

        ```
        __topic__:  
        age:  18
        content:  123
        name:  maki
        data: 342
        ```

        ```
        __topic__:  
        age:  18
        content:  23
        name:  maki
        data: 123
        ```

        Transformation rule:

        ```
        e_switch(e_search("content==123"), e_set("__topic__", "Number", mode="overwrite"), e_search("data==123"), e_set("__topic__", "PRO", mode="overwrite"))
        ```

        Result:

        ```
        __topic__:  Number
        age:  18
        content:  123
        name:  maki
        data: 342
        ```

        ```
        __topic__:  PRO
        age:  18
        content:  23
        name:  maki
        data: 123
        ```

    -   You can use the e\_switch and e\_output functions to deliver log entries that meet specified conditions to different Logstores. default=e\_drop\(\) indicates that log entries that do not meet specified conditions are deleted, and no other operations are performed on these log entries. If you do not set the default parameter, log entries that meet the specified conditions are delivered to the first specified Logstore.

        Raw log entries:

        ```
        __topic__:sas-log-dns
        test: aliyun
        
        __topic__: aegis-log-network
        test:ecs
        
        __topic__: local-dns
        test:sls
        
        __topic__:aegis-log-login
        test:sls
        ```

        Transformation rule:

        ```
        e_switch(e_match("__topic__","sas-log-dns"),e_output(name="target1"),
        e_match("__topic__","sas-log-process"),e_output(name="target2"),
        e_match("__topic__","local-dns"),e_output(name="target3"),
        e_match("__topic__","aegis-log-network"),e_output(name="target4"),
        e_match("__topic__","aegis-log-login"),e_output(name="target5"),
        default=e_drop())
        ```


