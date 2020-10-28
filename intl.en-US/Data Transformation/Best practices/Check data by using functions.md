# Check data by using functions

You can check and process data based on specific conditions by using the functions. This topic describes how to use functions to check data in various scenarios.

## Scenario 1: Check whether a field exists

-   Raw log entry

    ```
    a: a_value
    b:       // Empty string
    ```

-   Domain-specific language \(DSL\) orchestration
    -   Solution 1: Use the `e_has` and `e_not_has` functions.

        ```
        e_if(e_has("a"),e_set("has_a", true))
        e_if(e_has("b"),e_set("has_b", true))
        e_if(e_has("c"),e_set("has_c", true))
        e_if(e_not_has("a"),e_set("not_has_a", true))
        e_if(e_not_has("b"),e_set("not_has_b", true))
        e_if(e_not_has("c"),e_set("not_has_c", true))
        ```

    -   Solution 2: Use the `e_search` function.

        ```
        e_if(e_search('a: *'),e_set("has_a", true))
        e_if(e_search('b: *'), e_set("has_b", true))
        e_if(e_search('c: *'), e_set("has_c", true))
        e_if(e_search('not a: *'), e_set("not_has_a", true))
        e_if(e_search('not b: *'), e_set("not_has_b", true))
        e_if(e_search('not c: *'), e_set("not_has_c", true))
        ```

        **Note:** In the preceding example, an `e_if` function is written for each condition to better illustrate the solution. You can simplify the function by including all conditions and the corresponding operations as `e_if(condition 1, operation 1, condition 2, operation 2)`.

-   Result

    ```
    a:a_value
    b:    // Empty string
    has_a: true
    has_b: true
    has_c: false
    not_has_a: false
    not_has_b: false
    not_has_c: true
    ```


## Scenario 2: Check whether a field value exists and is not empty

-   Raw log entry

    ```
    a: a_value
    b:     // Empty string
    ```

-   DSL orchestration
    -   Solution 1 \(recommended\): Use the `v` function that returns a field value.

        ```
        e_if(v("a"), e_set("not_empty_a", true))
        e_if(v("b"), e_set("not_empty_b", true))
        e_if(v("c"), e_set("not_empty_c", true))
        ```

        **Note:** If the field value extracted by the `v` function exists and is not empty, the `Bool` value true is returned. Otherwise, false is returned.

    -   Solution 2: Use the `e_search` function.

        ```
        # The field value contains at least one character.
        e_if(e_search('a: "?"'), e_set("not_empty_a", true))
        e_if(e_search('b: "?"'), e_set("not_empty_b", true))
        e_if(e_search('c: "?"'), e_set("not_empty_c", true))
        
        # Regular expression
        e_if(e_search('a~=".+"'), e_set("not_empty_a", true))
        e_if(e_search('b~=".+"'), e_set("not_empty_b", true))
        e_if(e_search('c~=".+"'), e_set("not_empty_c", true))
        
        # The field value exists and is not empty.
        e_if(e_search('a: * and not a==""'), e_set("not_empty_a", true))
        e_if(e_search('b: * and not b==""'), e_set("not_empty_b", true))
        e_if(e_search('c: * and not c==""'), e_set("not_empty_b", true))
        ```

-   Result

    ```
    a: a_value
    b:     // Empty string
    not_empty_a: true
    not_empty_b: false
    not_empty_c: false
    ```


## Scenario 3: Check whether a field value exists and is empty

-   Raw log entry

    ```
    a: a_value
    b:     // Empty string
    ```

-   DSL orchestration
    -   Solution 1 \(recommended\): Use the `v` function that returns a field value.

        ```
        e_if(op_and(e_has("a"), op_not(v("a"))), e_set("empty_a", true))
        e_if(op_and(e_has("b"), op_not(v("b"))), e_set("empty_b", true))
        e_if(op_and(e_has("c"), op_not(v("c"))), e_set("empty_c", true))
        
        # Invalid syntax
        e_if(op_not(v("a")), e_set("empty_a", true))
        e_if(op_not(v("b")), e_set("empty_b", true))
        e_if(op_not(v("c")), e_set("empty_c", true))
        ```

        **Note:** If the field value extracted by the `v` function exists and is not empty, the `Bool` value true is returned. Otherwise, false is returned. The true value is returned if the field value does not exist or if the field value is `None`.

    -   Solution 2: Use the `e_search` function.

        ```
        e_if(e_search('a==""'), e_set("empty_a", true))
        e_if(e_search('b==""'), e_set("empty_b", true))
        e_if(e_search('c==""'), e_set("empty_c", true))
        
        # Invalid syntax
        e_if(e_search('a:""'), e_set("empty_a", true))
        e_if(e_search('b:""'), e_set("empty_b", true))
        ```

        **Note:** In the preceding example of the invalid syntax, the `e_search` function is used for partial query. In this case, true is returned if the value of the `a: ""` field exists, regardless of whether the value is empty.

-   Result

    ```
    a: a_value
    b:       // Empty string
    empty_a: false
    empty_b: true
    empty_b: false
    ```


## Scenario 4: Perform actions based on the logical relationships between field values

-   Raw log entries

    ```
    Log entry 1
    http_host: m1.abcd.com
    status: 200
    request_method: GET
    scheme: https
    header_length: 700
    body_length: 1200
    
    Log entry 2
    http_host: m2.abcd.com
    status: 200
    request_method: POST
    scheme: https
    header_length: 100
    body_length: 800
    
    Log entry 3
    http_host: m3.abcd.com
    status: 200
    request_method: GET
    scheme:  http
    header_length: 700
    body_length: 800
    
    Log entry 4
    http_host: m4.abcd.com
    status: 404
    request_method: GET
    scheme: https
    header_length: 100
    body_length: 300
    ```

-   Requirement 1

    Add the `type` field to all log entries in which the value of the `status` field is 200. The value of the type field is normal.

    -   DSL orchestration

        ```
        e_if(e_match("status", "200"), e_set("type", "normal"))
        Or
        e_if(e_search('status==200'), e_set("type", "normal"))
        ```

        **Note:**

        -   You can use one of these solutions in scenarios where the requirements are simple.
        -   In this case, `status:200` can be used to check whether the value of the status field contains 200. To be more precise, we recommend that you use `status==200`.
    -   Result

        ```
        Log entry 1
        type: normal
        http_host: m1.abcd.com
        status: 200
        request_method: GET
        scheme: https
        header_length: 700
        body_length: 1200
        
        Log entry 2
        type: normal
        http_host: m2.abcd.com
        status: 200
        request_method: POST
        scheme: https
        header_length: 100
        body_length: 800
        
        Log entry 3
        type: normal
        http_host: m3.abcd.com
        status: 200
        request_method: GET
        scheme: http
        header_length: 700
        body_length: 800
        
        Log entry 4
        http_host: m4.abcd.com
        status: 404
        request_method: GET
        scheme: https
        header_length: 100
        body_length: 300
        ```

-   Requirement 2

    Add the `type` field to all log entries that meet the following conditions: the value of the `status` field is 200, the value of the `request_method` field is GET, and the value of the `scheme` field is https. The value of the type field is normal.

    -   DSL orchestration

        ```
        e_if(e_search('status==200 and request_method==GET and scheme==https'), e_set("type", "normal"))
        Or
        e_if(e_match_all("status", "200", "request_method", "GET", "scheme", "https"), e_set("type", "normal"))
        ```

        **Note:**

        -   You can use the `e_search` or `e_match_all` function to match multiple fields. The `e_search` function is simpler.
        -   In this case, `status:200` can be used to check whether the value of the status field contains 200. To be more precise, we recommend that you use `status==200`.
    -   Result

        ```
        Log entry 1
        type: normal
        http_host: m1.abcd.com
        status: 200
        request_method: GET
        scheme: https
        header_length: 700
        body_length: 1200
        
        Log entry 2
        http_host: m2.abcd.com
        status: 200
        request_method: POST
        scheme: https
        header_length: 100
        body_length: 800
        
        Log entry 3
        http_host: m3.abcd.com
        status: 200
        request_method: GET
        scheme: http
        header_length: 700
        body_length: 800
        
        Log entry 4
        http_host: m4.abcd.com
        status: 404
        request_method: GET
        scheme: https
        header_length: 100
        body_length: 300
        ```

-   Requirement 3

    Add the `type` field to all log entries that meet one or more of the following conditions: the value of the `status` field is 200, the value of the `request_method` field is GET, or the value of the `scheme` field is https. The value of the type field is normal.

    -   DSL orchestration

        ```
        e_if(e_search('status==200 or request_method==GET or scheme==https'), e_set("type", "normal"))
        Or
        e_if(e_match_any("status", "200", "request_method", "GET", "scheme", "https"), e_set("type", "normal"))
        ```

    -   Result

        ```
        Log entry 1
        type: normal
        http_host: m1.abcd.com
        status: 200
        request_method: GET
        scheme: https
        header_length: 700
        body_length: 100
        
        Log entry 2
        type: normal
        http_host: m2.abcd.com
        status: 200
        request_method: POST
        scheme: https
        header_length: 100
        body_length: 800
        
        Log entry 3
        type: normal
        http_host: m3.abcd.com
        status: 200
        request_method: GET
        scheme: http
        header_length: 700
        body_length: 800
        
        Log entry 4
        type: normal
        http_host: m4.abcd.com
        status: 404
        request_method: GET
        scheme: https
        header_length: 100
        body_length: 1300
        ```

-   Requirement 4

    Add the `type` field to all log entries that meet the following conditions: the value of the `status` field is 200, the value of the `request_method` field is GET, and the sum of the values of the `header_length` and `body_length` fields is less than or equal to 1000. The value of the type field is normal.

    -   DSL orchestration

        ```
        e_if(op_and(e_search('status: 200 and request_method: GET'), op_le(op_sum(v("header_length"), v("body_length")), 1000)), e_set("type", "normal"))
        ```

        **Note:** You can use the `e_search` function and other expression functions for multiple logical operations.

    -   Result

        ```
        Log entry 1
        type: normal
        http_host: m1.abcd.com
        status: 200
        request_method: GET
        scheme: https
        header_length: 700
        body_length: 100
        
        Log entry 2
        http_host: m2.abcd.com
        status: 200
        request_method: POST
        scheme: https
        header_length: 100
        body_length: 800
        
        Log entry 3
        http_host: m3.abcd.com
        status: 200
        request_method: GET
        scheme: http
        header_length: 700
        body_length: 800
        
        Log entry 4
        http_host: m4.abcd.com
        status: 404
        request_method: GET
        scheme: https
        header_length: 100
        body_length: 1300
        ```


