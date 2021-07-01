# Value assignment function

This topic describes the syntax and parameters of the value assignment function. This topic also provides several examples of the value assignment function.

## e\_set

You can use the e\_set function to add a field or specify a new value for an existing field.

-   Syntax

    ```
    e_set(key1, value1, key2, value2, mode="overwrite")
    ```

    **Note:**

    -   The key1 and value1 parameters must be specified in pairs.
    -   When you use the e\_set function to set the value of a time field, such as F\_TIME or \_\_time\_\_, the value must be a numeric string.

        ```
        e_set(F_TIME, "abc")   # Invalid syntax.
        e_set(F_TIME, "12345678")   # Valid syntax.
        ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |key|String|Yes|The name of a log field. You can set this parameter to an expression that is used to return a string. For more information about how to set special field names, see [Event structure and fields](/intl.en-US/Data Transformation/Data processing syntax/Data structures.md).|
    |value|Arbitrary|Yes|The new value of a specified field. If the value of this parameter is not a string, the value is automatically converted to a string. For example, a value of the tuple, list, or dictionary type is automatically converted to a JSON string. For more information about the conversion rule of strings, see [Automatic type conversion during assignment](/intl.en-US/Data Transformation/Data processing syntax/Data structures.md). **Note:** If you set the value to None, the original value of the specified field is not updated. |
    |mode|String|No|The overwrite mode of fields. Default value: overwrite. For more information, see [Field check and overwrite modes](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).|

-   Response

    The updated log entry is returned.

-   Examples
    -   Example 1: Assign a fixed value to a field.

        Add a new field named city and set the value to Shanghai.

        ```
        e_set("city", "Shanghai")
        ```

    -   Example 2: Extract the value of an existing field and assign the value to another field.

        Invoke an expression function to extract the value of an existing field named ret, and then assign the value to a new field named result.

        ```
        e_set("result", v("ret"))
        ```

    -   Example 3: Assign a dynamic value to a field.

        Combine multiple expression functions to obtain the value of the first field from specified existing fields, and specify the value in lowercase for the result field.

        ```
        e_set("result", str_lower(v("ret", "return")))
        ```

    -   Example 4: Specify a value for a field multiple times.
        1.  Specify a fixed value for the event\_type field.

            ```
            e_set("event_type", "login event", "event_info", "login host")
            ```

        2.  If the value of the ret field is fail, set the event\_type field to login failed event.

            ```
            e_if(e_search('ret==fail'), e_set("event_type", "login failed event" ))
            ```


