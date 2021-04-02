# Value assignment function

This topic describes the syntax and parameters of the value assignment function. This topic also provides several examples of the value assignment function.

## e\_set

You can use the value assignment function to set the value of a field in an event.

-   Syntax

    ```
    e_set(Field 1, Value 1, Field 2, Value 2, ..., mode="overwrite")
    ```

    **Note:**

    -   The `Field` and `Value` parameters must be specified in pairs.
    -   When you use the `e_set` function to modify the value of a time field, such as `F_TIME` or `__time__`, you must set the value to a numeric string.

        ```
        e_set(F_TIME, "abc")   # Invalid syntax.
        e_set(F_TIME, "12345678")   # Valid syntax.
        ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Field|String|Yes|The name of a log field. The value of this parameter can be an expression that returns a string. For more information about how to set special field names, see [Event structure and fields](/intl.en-US/Data Transformation/Data processing syntax/Data structures.md).|
    |Value|All|Yes|The new value of a specified field. If the value of this parameter is not a string, the value is automatically converted to a string. For example, a value of the tuple, list, or dictionary type is automatically converted to a JSON string. For more information about the conversion rule of strings, see [Automatic type conversion during assignment](/intl.en-US/Data Transformation/Data processing syntax/Data structures.md). **Note:** If the passed value is None, the original value of the specified field is not updated. |
    |mode|String|No|The overwrite mode of fields. Default value: overwrite. For more information, see [Field check and overwrite modes](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).|

-   Response

    The updated event is returned.

-   Examples
    -   Example 1: Assign a fixed value to a field.

        Add a new field named `city` and set the value to `Shanghai`.

        ```
        e_set("city", "Shanghai")
        ```

    -   Example 2: Extract the value of an existing field and assign the value to another field.

        Invoke an expression function to extract the value of an existing field named `ret`, and then assign the value to a new field named `result`.

        ```
        e_set("result", v("ret"))
        ```

    -   Example 3: Assign a dynamic value to a field.

        Combine multiple expression functions to obtain the value of the first field from specified existing fields, and assign the value in lowercase to the `result` field.

        ```
        e_set("result", str_lower(v("ret", "return")))
        ```

    -   Example 4: Assign a value to a field multiple times.
        1.  Assign a fixed value to the `event_type` field.

            ```
            e_set("event_type", "login event", "event_info", "login host")
            ```

        2.  If the value of the `ret` field is fail, set the value of the `event_type` field to login failed event.

            ```
            e_if(e_search('ret==fail'), e_set("event_type", "login failed event" ))
            ```


