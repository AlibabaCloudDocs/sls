# Field processing functions

This topic describes the syntax of field processing functions and provides parameter descriptions and function examples.

## Functions

|Function|Description|
|--------|-----------|
|[e\_drop\_fields](#section_q8m_zn8_uvj)|Deletes the log fields that meet the specified conditions.|
|[e\_keep\_fields](#section_e3g_856_vs6)|Retains the log fields that meet the specified conditions.|
|[e\_rename](#section_dg9_67q_cjh)|Renames the log fields that meet the specified conditions.|

## e\_drop\_fields

You can use the e\_drop\_fields function to delete a log field that meets the specified conditions.

-   Syntax

    ```
    e_drop_fields (field 1, field 2, ....,regex=False)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Log field|String|Yes|The name of a log field. The name can be a regular expression. If the field name fully matches the specified conditions, the field is deleted. Otherwise, the field is retained. For more information about regular expressions, see [Regular expressions](/intl.en-US/Data Transformation/Data processing syntax/General reference/Regular expressions.md).You must configure at least one log field. |
    |regex|Boolean|No|Default value: False. This value indicates that regular expressions are not used to match log fields.|

-   Example: If the value of the content field is 123, the content and age fields are deleted.
    -   Raw log entry:

        ```
        age:  18
        content:  123
        name: twiss
        ```

    -   Transformation rule:

        ```
        e_if(e_search("content==123"), e_drop_fields("content", "age",regex=True)
        ```

    -   Result:

        ```
        name: twiss
        ```


## e\_keep\_fields

You can use the e\_keep\_fields function to retain a log field that meets the specified conditions.

**Note:** Log Service provides built-in meta fields, such as the \_\_time\_\_ and \_\_topic\_\_ fields. If you do not retain the \_\_time\_\_ field when you call the e\_keep\_fields function, the time of the event is reset to the current time. If you do not want to reset the value of a meta field, add the meta field to the list of fields to retain in the format of F\_TIME, F\_META, F\_TAGS, "f1", "f2". For more information, see [Identifiers](/intl.en-US/Data Transformation/Data processing syntax/Data structures.md).

-   Syntax

    ```
    e_keep_fields (field 1, field 2, ....,regex=False)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |String|String|Yes|The name of a log field. The name can be a regular expression. If the field name fully matches the specified conditions, the field is retained. Otherwise, the field is deleted.You must configure at least one log field. |
    |regex|Boolean|No|Default value: False. This value indicates that regular expressions are not used to match log fields.|

-   Example: If the value of the content field is 123, the content and age fields are retained.
    -   Raw log entry:

        ```
        age:  18
        content:  123
        name: twiss
        ```

    -   Transformation rule:

        ```
        e_if(e_search("content==123"), e_keep_fields("content", "age"))
        ```

    -   Result:

        ```
        age:  18
        content:  123
        ```


## e\_rename

You can use the e\_rename function to rename a log field that matches the specified conditions.

-   Syntax

    ```
    e_rename("field 1", "renamed field 1", "field 2", "renamed field 2", ..., regex=False)
    ```

    **Note:** The field and renamed field fields must be used in pairs.

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |String|String|Yes|The name of a log field. The name can be a regular expression. If the field name fully matches the specified conditions, the field is renamed. For more information about regular expressions, see [Regular expressions](/intl.en-US/Data Transformation/Data processing syntax/General reference/Regular expressions.md).You must configure at least one log field. |
    |Renamed field|String|Yes|The renamed field.|
    |regex|Boolean|No|Default value: False. This value indicates that regular expressions are not used to match log fields.|

-   Result:

    The renamed string is returned.

-   Examples
    -   Example 1
        -   Raw log entry:

            ```
            host:  1006
            ```

        -   Transformation rule:

            ```
            e_rename("host","client_host")
            ```

        -   Result:

            ```
            client_host:  1006
            ```

    -   Example 2
        -   Raw log entry:

            ```
            host:  1006
            ```

        -   Transformation rule:

            ```
            e_rename("url","rename_url")
            ```

        -   Result:

            ```
            host:  1006
            ```


