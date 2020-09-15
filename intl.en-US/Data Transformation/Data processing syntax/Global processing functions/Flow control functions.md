# Flow control functions

This topic describes the syntax of the flow control functions and provides parameter descriptions and function examples.

## Functions

|Function|Description|
|--------|-----------|
|[e\_compose](#section_zr0_ghx_vie)|Combines operations. -   This function is used to combine operations in the `e_if`, `e_switch`, or `e_if_else` function.
-   The system invokes the operations specified in this function in sequence, and then dispatches, converts, and returns the resulting event.
-   If the system has invoked an operation to delete an event, no subsequent operations will be performed on the event. |
|[e\_if](#section_dhk_ius_2q8)|Combines conditions and operations. -   If a condition evaluates to true, the system invokes the operation associated with the condition. If a condition evaluates to false, the system skips the operation associated with the condition and continues onto the next condition.
-   If the system has invoked an operation to delete an event, no subsequent operations will be performed on the event. |
|[e\_if\_else](#section_6dy_m0v_hig)|Specifies a condition to invoke an associated operation.|
|[e\_switch](#section_f1t_ukb_ilk)|Combines conditions and operations. -   If a condition evaluates to true, the system invokes the operation associated with the condition and returns the corresponding result. If a condition evaluates to false, the system skips the associated operation and continues onto the next condition.
-   If none of the specified conditions evaluates to true but the default parameter is set, the system invokes the operations specified in the default parameter and returns the corresponding results.
-   If the system has invoked an operation to delete an event, no subsequent operations will be performed on the event. |

## e\_compose

-   Syntax

    ```
    e_compose(Operation 1, Operation 2, ...)     
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Operation 1|Global operation function|Yes|One or more other global operation functions.|
    |Operation 2|Global operation function|No|One or more other global operation functions.|

-   Response

    An event on which the specified operations have been invoked is returned.

-   Example: If the value of the `content` field is 123, delete the `age` and `name` fields and then set the `content` field to ctx.

    Raw log:

    ```
    content: 123
    age: 23
    name: twiss
    ```

    Processing rule:

    ```
    e_if(e_search("content==123"), e_compose(e_drop_fields("age|name"), e_rename("content", "ctx")))
    ```

    Processing result:

    ```
    ctx:  123
    ```


## e\_if

-   Syntax

    ```
    e_if(Condition, Operation)
    e_if(Condition 1, Operation 1, Condition 2, Operation 2, ....)
    ```

    **Note:** The `Condition` and `Operation` parameters must appear in pairs.

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Condition|All|Yes|One or more other expressions. If the result is not a Boolean value, the system evaluates whether the condition is true or false.|
    |Operation|Global operation function|No|One or more other global operation functions.|

-   Response

    An event on which the specified operations have been invoked is returned.

-   Examples
    -   Example 1: When a specified field is a certain value, invoke an associated operation.

        If the value of the `result` field is `failed` or `failure`, the system sets the topic field to `login_failed_event`:

        ```
        e_if(e_match("result", r"failed|failure"), e_set("__topic__", "login_failed_event"))
        ```

    -   Example 2: When the specified field exists and its value is not null, extract data.

        If the `request_body` field exists and its value is not null, the system invokes a JSON function to expand the `request_body` field to show more than one value:

        ```
        e_if(v("request_body"), e_json("request_body"))
        ```

    -   Example 3: When a specified nested field is a certain value, invoke an associated operation.

        If the value of the `valid` field is failed in lowercase, the system discards the event:

        ```
        e_if(op_eq(str_lower(v("valid")), "failed"), DROP)
        ```

    -   Example 4: Invoke operations associated with multiple conditions in sequence.

        ```
        e_if(True, e_set("__topic__", "default_login"), 
             e_match("valid", "failed"), e_set("__topic__", "login_failed_event")
          )
        ```


## e\_if\_else

-   Syntax

    ```
    e_if_else(Condition, Operation invoked with Condition evaluated to true, Operation invoked with Condition evaluated to false)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Condition|All|Yes|One or more other expressions. If the result is not a Boolean value, the system evaluates whether the condition is true or false.|
    |Operation invoked with Condition evaluated to true|Global operation function|Yes|One or more other global operation functions.|
    |Operation invoked with Condition evaluated to false|Global operation function|Yes|One or more other global operation functions.|

-   Response

    The results of operations associated with specified conditions are returned.

-   Example: If the value of the `result` field is ok or pass or the value of the `status` field is 200, retain the log. Otherwise, delete the log.

    Raw log:

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

    Processing rule:

    ```
    e_if_else(op_or(e_match("result", r"(? i)ok|pass"), e_search("status== 200"), KEEP, DROP)
    ```

    Processing result: The system retains only the first two log entries.

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

    **Note:** The `Condition` and `Operation` parameters must appear in pairs.

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Condition|All|Yes|One or more other expressions. If the result is not a Boolean value, the system evaluates whether the condition is true or false.|
    |Operation|Global operation function|Yes|One or more other global operation functions.|
    |default|Global operation function|No|One or more other default global operation functions. If none of the specified conditions evaluates to true, the system invokes the operations specified by this parameter.|

-   Response

    An event on which the specified operations have been invoked is returned.

-   Example: If the value of the `content` field is 123, set the `__topic__` field to Number. If the value of the `data` field is 123, set the `__topic__` field to PRO.

    Raw log:

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

    Processing rule:

    ```
    e_switch(e_search("content==123"), e_set("__topic__", "Number", mode="overwrite"), e_search("data==123"), e_set("__topic__", "PRO", mode="overwrite"))
    ```

    Processing result:

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


