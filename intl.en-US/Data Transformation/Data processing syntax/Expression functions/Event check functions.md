# Event check functions

This topic describes the syntax of event check functions and provides parameter descriptions and function examples.

## Functions

|Type|Function|Description|
|----|--------|-----------|
|Basic functions|[v](#section_u2u_et7_tvj)|Extracts the value of a field from an event. If multiple field names are passed to the function, the value of the first field that exists is returned.|
|[e\_has](#section_a1l_c7d_olu)|Checks whether a field exists.|
|[e\_not\_has](#section_4wc_uyi_81q)|Checks whether a field does not exist.|
|Expression functions|[e\_search](#section_syl_ku4_k84)|Searches for an event by using Lucene-like query syntax.|
|[e\_match\*](#section_8ng_n08_z4i)|Functions of this type include `e_match`, `e_match_all`, and `e_match_any`. Checks whether the value of a field in an event meets the conditions specified in an expression.|

The following table lists the expression functions that can be used together with event check functions.

|Type|Function|Description|
|----|--------|-----------|
|Logical functions|[op\_and](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Operator functions.md)|Invokes the AND operation among expressions.|
|[op\_or](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Operator functions.md)|Invokes the OR operation among expressions.|
|[op\_not](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Operator functions.md)|Invokes the NOT operation among expressions.|
|[op\_nullif](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Operator functions.md)|Checks whether the values of two expressions are equal.|
|[op\_ifnull](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Operator functions.md)|Returns the value of the first expression whose value is not None.|
|[op\_coalesce](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Operator functions.md)|Returns the value of the first expression whose value is not None.|

## v

-   Syntax

    ```
    v(Field name, ..., default=None)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Field name|String|Yes|The name of the field in an event from which you want to extract a value.|
    |default|Arbitrary|No|The value of this parameter is returned if none of the specified fields exist. Default value: None.|

-   Response

    The value of the first field that exists in the specified event is returned. If none of the specified fields exist, the value of the `default` parameter is returned.

-   Example

    Assigns the value of the content field to the test\_content field.

    -   Raw log entry:

        ```
        content: hello
        ```

    -   Transformation rule:

        ```
        e_set("test_content", v("content"))
        ```

    -   Result:

        ```
        content: hello
        test_content: hello
        ```


## e\_has

-   Syntax

    ```
    e_has("Field name")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Field name|String|Yes|The name of a field in an event.|

-   Response

    If the specified field exists, the value True is returned. Otherwise, the value False is returned.

-   Example

    Checks whether an event contains the content field. If the event contains the content field, the event is retained. Otherwise, the event is dropped.

    -   Raw log entry:

        ```
        content: 123
        ```

    -   Transformation rule:

        ```
        e_keep(e_has("content"))
        ```

    -   Result:

        ```
        content: 123
        ```


## e\_not\_has

-   Syntax

    ```
    e_not_has("Field name")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Field name|String|Yes|The name of a field in an event.|

-   Response

    If the specified field does not exist, the value True is returned. Otherwise, the value False is returned.

-   Example

    Checks whether an event contains the content field. If the event does not contain the content field, the event is retained. Otherwise, the event is dropped.

    -   Raw log entry:

        ```
        content: 123
        ```

    -   Transformation rule:

        ```
        e_if(e_not_has("content"),KEEP,DROP)
        ```

    -   Result:

        ```
        # The event is dropped.
        ```


## e\_search

-   Syntax

    ```
    e_search(Query string)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Query string|String|Yes|The string that is used to filter log data and simplify data transformation. For more information, see [Query string syntax](/intl.en-US/Data Transformation/Data processing syntax/General reference/Query string syntax.md).|

-   Response

    If the specified conditions are met, the value True is returned. Otherwise, the value False is returned.

-   Examples``

    ```
    # Full-text search
    e_search("active error")   # Searches for multiple substrings in full text. The substrings are associated with each other by using the logical operator OR.
    e_search('"active error"')   # Searches for a substring in full text.
    
    # Field search
    e_search("status: active")   # Searches for a substring in a specified field.
    e_search('author: "john smith"')   # Searches for a substring that contains a space character in a specified field.
    e_search('field: active error')   # Searches the specified field for the substring "active" or searches all logs for the substring "error". The query string in this example is equivalent to field:active OR "error".
    
    # Exact match
    e_search('author== "john smith"')  
    
    # Search for field values by using wildcard characters. Each asterisk (*) is used to match zero or more characters. Each question mark (?) is used to match one character.
    e_search("status: active * test")   # `active*test` contains one asterisk (*). The value does not need to be enclosed in double quotation marks ("").
    e_search("status: active? good")    # `active? good` contains one question mark (?). The value does not need to be enclosed in double quotation marks ("").
    e_search("status== ac*tive? good")   # The query string is used for exact match.
    
    # Escape special characters in a field value. If no asterisks (*) or question marks (?) are used as wildcards, they must be escaped in a field value.
    e_search('status: "\*\?()[]:="')   # `\*\?()[]:=` contains multiple special characters. The value must be enclosed in double quotation marks (""). The asterisks(*), question marks (?), and backslashes (\) in the value are escaped.
    e_search("status: active\* test")   # `active\*test` contains one asterisk (*). The value does not need to be enclosed in double quotation marks ("").
    e_search("status: active\? test")  # `active\? test`contains one question mark (?). The value does not need to be enclosed in double quotation marks ("").
    
    # Escape special characters in a field name
    e_search("\*\(1+1\)\?: abc")   # The field name cannot be enclosed in double quotation marks (""). Special characters must be escaped by using backslashes (\).
    e_search("__tag__\:__container_name__: abc")   # Special characters must be escaped by using backslashes (\).
     
    
    # Search for strings by using regular expressions.
    e_search('content~="regular expression"')   # Searches for substrings that match the regular expression.
    
    # Numeric value comparison
    e_search('count: [100, 200]')   # >=100 and <=200
    e_search('count: [*, 200]')     # <=200
    e_search('count: [200, *]')     # >=200
    e_search('age >= 18')           # >= 18
    e_search('age > 18')            # > 18
    
    # Relational operators
    e_search("abc OR xyz")   # The relational operator is case-insensitive.
    e_search("abc and (xyz or zzz)")
    e_search("abc and not (xyz and not zzz)")
    e_search("abc && xyz")    # and
    e_search("abc || xyz")    # or
    e_search("abc || ! xyz")   # or not
    ```


## e\_match\*

-   Syntax

    ```
    e_match(Field name, regular expression, full=True)
    e_match_all(Field name 1, regular expression 1, field name 2, regular expression 2, ..., full=True)
    e_match_any(Field name 1, regular expression 1, field name 2, regular expression 2, ..., full=True)
    ```

    **Note:**

    -   The `field name` and `regular expression` parameters in the function must be used in pairs.
    -   The e\_match function is often used together with the `op_not`, `op_and`, or `op_or` function.
-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Field name|String|Yes|The name of a field. If the specified field does not exist, the condition that is specified for the field is not met. For example, if the `f1` field does not exist, the value returned by the `e_match("f1", ...)` function is False. |
    |Regular expression|String|Yes|The regular expression used to match strings. To match strings by using exact strings, you can use the `str_regex_escape` function to convert regular expressions.|
    |full|Bool|No|Specifies whether to implement an exact match. By default, the parameter is set to True, which specifies an exact match. For more information, see [Regular expressions](/intl.en-US/Data Transformation/Data processing syntax/General reference/Regular expressions.md).|

-   Response

    The value True is returned if the specified field is matched. Otherwise, the value False is returned.

    **Note:**

    -   `e_match_any`: If one or more specified fields are matched, the value True is returned. Otherwise, the value False is returned.
    -   `e_match_all`: If all specified fields are matched, the value True is returned. Otherwise, the value False is returned.
-   Examples
    -   Example 1:

        Implements an exact match by using the e\_match function.

        -   Raw log entry:

            ```
            k1: 123
            ```

        -   Transformation rule:

            ```
            e_set("match",e_match("k1",r'\d+'))
            ```

        -   Result:

            ```
            k1: 123
            match: True
            ```

    -   Example 2:

        Implements an exact match by using the e\_match\_all function.

        -   Raw log entry:

            ```
            k1: 123
            k2: abc
            k3: abc123
            ```

        -   Transformation rule:

            ```
            e_set("match",e_match_all('k1', r'\d+', 'k4', '. +'))
            ```

        -   Result:

            ```
            k1: 123
            k2: abc
            k3: abc123
            match: False
            ```

        **Note:** The e\_match\_any and e\_match\_all functions are used in a similar way.


