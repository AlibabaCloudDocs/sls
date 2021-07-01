# Mapping and enrichment functions

This topic describes the syntax and parameters of mapping and enrichment functions. This topic also provides usage examples.

## Functions

|Type|Function|Description|
|----|--------|-----------|
|Field-based mapping|[e\_dict\_map](#section_66o_75y_psg)|Maps the value of a specified field to a field in a dictionary, and then returns the value of the matched field.|
|[e\_table\_map](#section_s80_usp_myx)|Maps the value of a specified field to a row in a table, and then returns the value of the field in this row.|
|Search-based mapping|[e\_search\_dict\_map](#section_pbk_lwq_04u)|Maps a search string to a key in a dictionary, and then returns the value of the matched key.|
|[e\_search\_table\_map](#section_mp3_goc_rxa)|Maps a search string to a column in a table, and then returns the value of the field in this column.|

## e\_dict\_map

-   Syntax

    ```
    e_dict_map(data, field, output_field, case_insensitive=True, missing=None, mode="overwrite")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |data|Dict|Yes|The dictionary that is used for mapping. A dictionary includes a collection of key-value pairs. Each key must be a string.|
    |field|String or string list|Yes|One or more field names. If the value of this parameter contains multiple field names, one of the following results is returned:    -   The system maps the field names in sequence.
    -   If multiple log entries match the specified search condition and the mode parameter is set to overwrite, the system returns the last matched log entry.
    -   If no field matches the specified search condition, the system returns the value of the missing parameter. |
    |output\_field|String|Yes|The field name to be returned.|
    |case\_insensitive|Bool|No|Specifies whether data is case-sensitive when the system maps the data. Default value: True. This value indicates that data is not case-sensitive. **Note:** If multiple fields in the dictionary match the specified search condition and the case\_insensitive parameter is set to True, the system selects the field that uses the same case as the key. If no key is found, the system randomly selects a field. |
    |missing|String|No|The value that is returned if no fields are matched. Default value: None. This value indicates that no operations are performed. **Note:** If the specified dictionary contains a mapping rule that returns the asterisk \(\*\) wildcard and no fields are matched, the `missing` parameter is disabled. This is because the asterisk \(\*\) wildcard has a higher priority than the `missing` parameter. |
    |mode|String|No|The overwrite mode of fields. Default value: overwrite. For more information, see [Field check and overwrite modes](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).|

-   Response

    An event in which new fields and values are added is returned.

-   Examples
    -   Example 1

        Raw log entry:

        ```
        data:  123
        pro:  1
        ```

        Transformation rule:

        ```
        e_dict_map({"1": "TCP", "2": "UDP", "3": "HTTP", "*": "Unknown"}, "pro", "protocol")
        ```

        Result:

        ```
        data:  123
        pro:  1
        protocol:  TCP
        ```

    -   Example 2

        Raw log entries \(three log entries\):

        ```
        status:  500
        ```

        ```
        status:  400
        ```

        ```
        status:  200
        ```

        Transformation rule:

        ```
        e_dict_map({"400": "Error", "200": "Success", "*": "Other"}, "status", "message")
        ```

        Result:

        ```
        status:  500
        message:  Other
        ```

        ```
        status:  400
        message:  Error
        ```

        ```
        status:  200
        message:  Success
        ```


## e\_table\_map

-   Syntax

    ```
    e_table_map(data, field, output_fields, missing=None, mode="fill-auto")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |data|Table|Yes|The table that is used to map values in multiple columns. **Note:** If you use a resource function such as res\_rds\_mysql or res\_log\_logstore\_pull to pull data, we recommend that you set the primary\_keys parameter to improve the performance of data transformation. For more information, see [Resource functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.md). |
    |field|String, string list, or tuple list|Yes|The source fields that are mapped to the specified table in an event. If no field is matched in the event, no operations are performed on the source fields.|
    |output\_fields|String, string list, or tuple list|Yes|The fields that are matched in the specified table.|
    |missing|String|No|The value that is returned if no fields are matched. Default value: None. This value indicates that no operations are performed. If the source field is mapped to multiple columns, the value of the `missing` parameter can be a list of default values. The length of the list is the same as the length of the source field. **Note:** If the specified table contains a mapping rule that returns the asterisk \(\*\) wildcard and no fields are matched, the `missing` parameter is disabled. This is because the asterisk \(\*\) wildcard has a higher priority than the `missing` parameter. |
    |mode|String|No|The overwrite mode of fields. Default value: fill-auto. For more information, see [Field check and overwrite modes](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).|

-   Response

    An event in which new fields and values are added is returned.

-   Examples
    -   Example 1: One field is returned.

        Raw log entry:

        ```
        data: 123
        city: nj
        ```

        Transformation rule:

        ```
        e_table_map(tab_parse_csv("city,pop,province\nnj,800,js\nsh,2000,sh"), "city", "province")
        ```

        Result:

        ```
        data: 123
        city: nj
        province: js
        ```

    -   Example 2: Two fields are returned.

        Raw log entry:

        ```
        data: 123
        city: nj
        ```

        Transformation rule:

        ```
        e_table_map(tab_parse_csv("city,pop,province\nnj,800,js\nsh,2000,sh"), "city", ["province", "pop"])
        ```

        Result:

        ```
        data: 123
        city: nj
        province: js
        pop: 800
        ```

    -   Example 3: The sep parameter in the tab\_parse\_csv function is used.

        Raw log entry:

        ```
        data: 123
        city: nj
        ```

        Transformation rule:

        ```
        e_table_map(tab_parse_csv("city#pop#province\nnj#800#js\nsh#2000#sh", sep='#'), "city", ["province", "pop"])
        ```

        Result:

        ```
        data: 123
        city: nj
        province: js
        pop: 800
        ```

    -   Example 4: The quote parameter in the tab\_parse\_csv function is used.

        Raw log entry:

        ```
        data: 123
        city: nj
        ```

        Transformation rule:

        ```
        e_table_map(tab_parse_csv('city,pop,province\n|nj|,|800|,|js|\n|shang hai|,2000,|SHANG,HAI|', quote='|'), "city", ["province", "pop"])
        ```

        Result:

        ```
        data: 123
        city: nj
        province: js
        pop: 800
        ```

    -   Example 5: The value of the specified source field is different from the values of the fields in the specified table.

        Raw log entry:

        ```
        data: 123
        cty: nj
        ```

        Transformation rule:

        ```
        e_table_map(tab_parse_csv("city,pop,province\nnj,800,js\nsh,2000,sh"), [("cty","city")], "province")
        ```

        Result:

        ```
        data: 123
        cty: nj
        province: js
        ```

    -   Example 6: The value of the specified source field is different from the values of the fields in the specified table, and the specified output field is renamed.

        Raw log entry:

        ```
        data: 123
        cty: nj
        ```

        Transformation rule:

        ```
        e_table_map(tab_parse_csv("city,pop,province\nnj,800,js\nsh,2000,sh"), [("cty","city")], [("province","pro")])
        ```

        Result:

        ```
        data: 123
        cty: nj
        pro: js
        ```

    -   Example 7: Multiple source fields are specified.

        Raw log entry:

        ```
        data: 123
        city: nj
        pop: 800
        ```

        Transformation rule:

        ```
        e_table_map(tab_parse_csv("city,pop,province\nnj,800,js\nsh,2000,sh"), ["city", "pop"], "province")
        ```

        Result:

        ```
        data: 123
        city: nj
        pop: 800
        province: js
        ```

    -   Example 8: Multiple source fields are specified, and the values of the specified source fields are different from the values of the fields in the specified table.

        Raw log entry:

        ```
        data: 123
        cty: nj
        pp: 800
        ```

        Transformation rule:

        ```
        e_table_map(tab_parse_csv("city,pop,province\nnj,800,js\nsh,2000,sh"), [("cty", "city"), ("pp", "pop")], "province")
        ```

        Result:

        ```
        data: 123
        cty: nj
        pop: 800
        province: js
        ```


## e\_search\_dict\_map

-   Syntax

    ```
    e_search_dict_map(data, output_field, multi_match=False, multi_join=" ", missing=None, mode="overwrite")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |data|Dict|Yes|The dictionary that is used for mapping. A dictionary includes a collection of key-value pairs. Each key must be a search string. For more information about search strings, see the [dct\_get](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Dictionary functions.md) function.|
    |output\_field|String|Yes|The field name to be returned.|
    |multi\_match|Bool|No|Specifies whether multiple matched fields can be returned. Default value: False. This value indicates that the system returns only the value of the first matched field. If you set the parameter to True, the system concatenates the values of multiple matched fields by using the character that is specified in the `multi_join` parameter.|
    |multi\_join|String|No|The character that is used to concatenate the values of multiple matched fields. The default value is a space character. This parameter is valid only when the multi\_match parameter is set to True.|
    |missing|String|No|The value that is returned if no fields are matched. Default value: None. This value indicates that no operations are performed. **Note:** If the specified dictionary contains a mapping rule that returns the asterisk \(\*\) wildcard and no fields are matched, the `missing` parameter is disabled. This is because the asterisk \(\*\) wildcard has a higher priority than the `missing` parameter. |
    |mode|String|No|The overwrite mode of fields. Default value: overwrite. For more information, see [Field check and overwrite modes](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).|

-   Response

    The matched values are returned.

-   Examples
    -   Example 1: Data mapping in matching mode

        Raw log entry:

        ```
        data: 123
        pro: 1
        ```

        Transformation rule:

        ```
        e_search_dict_map ({"pro==1": "TCP", "pro==2": "UDP", "pro==3": "HTTP"}, "protocol")
        ```

        Result:

        ```
        data: 123
        pro: 1
        protocol: TCP
        ```

    -   Example 2: Data mapping based on the first character in the value of each field

        Raw log entry:

        ```
        status: 200,300
        ```

        Transformation rule:

        ```
        e_search_dict_map ({"status:2??": "ok", "status:3??": "redirect", "status:4??": "auth", "status:5??": "server_error"}, "status_desc", multi_match=True, multi_join="test")
        ```

        Result:

        ```
        status: 200,300
        status_desc: ok test redirect
        ```


## e\_search\_table\_map

-   Syntax

    ```
    e_search_table_map(data, inpt, output_fields, multi_match=False, multi_join=" ", missing=None, mode="fill-auto")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |data|Table|Yes|The table from which data is obtained. The name of a column in the table must be a search string. **Note:** If you use a resource function, such as res\_rds\_mysql or res\_log\_logstore\_pull, to pull data, we recommend that you set the primary\_keys parameter to improve the performance of data transformation. For more information, see [Resource functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.md). |
    |inpt|String|Yes|The field names that match the search string in the specified table.|
    |output\_fields|String, string list, or tuple list|Yes|The fields that are matched in the specified table. The value of this parameter is a string, a string list, or a tuple list.|
    |multi\_match|Bool|No|Specifies whether multiple matched fields can be returned. Default value: False. This value indicates that the system returns only the value of the first matched field. If you set the parameter to True, the system concatenates the values of multiple matched fields by using the character that is specified in the `multi_join` parameter.|
    |multi\_join|String|No|The character that is used to concatenate the values of multiple matched fields. The default value is a space character. This parameter is valid only when the `multi_match` parameter is set to True.|
    |missing|String|No|The value that is returned if no fields are matched. Default value: None. This value indicates that no operations are performed. **Note:** If the specified table contains a mapping rule that returns the asterisk \(`*`\) wildcard and no fields are matched, the `missing` parameter is disabled. This is because the asterisk \(`*`\) wildcard has a higher priority than the `missing` parameter. |
    |mode|String|No|The overwrite mode of fields. Default value: fill-auto.|

-   Source table

    |Column name|Type 1|Type 2|
    |-----------|------|------|
    |`content: guide and title:~"\w guide"'`|guide|user|
    |`content: city and title:~"\w shanghai"`|food|home|

-   Response

    The matched values are returned.

-   Examples
    -   Example 1: Data mapping in simple mode

        Raw log entry:

        ```
          data: 123
          city: sh
        ```

        Transformation rule:

        ```
        e_search_table_map(tab_parse_csv("search,pop,province\ncity==nj,800,js\ncity==sh,2000,sh"), "search", ["pop", "province"])
        ```

        Result:

        ```
        data: 123
        city: sh
        province: sh
        pop: 2000
        ```

    -   Example 2: Data mapping in overwrite mode

        Raw log entry:

        ```
        data: 123
        city: nj
        province:
        ```

        Transformation rule:

        ```
        e_search_table_map(tab_parse_csv("search,pop,province\ncity==nj,800,js\ncity==sh,2000,sh"), "search", "province",mode="overwrite")
        ```

        Result:

        ```
        data: 123
        city: nj
        province: js
        ```

    -   Example 3: Data mapping in missing mode when the table does not contain the province field

        Raw log entry:

        ```
        data: 123
        city: wh
        province: 
        ```

        Transformation rule:

        ```
        e_search_table_map(tab_parse_csv("search,pop,province\ncity==nj,800,\ncity==sh,2000,sh"), "search", "province",missing="Unknown")
        ```

        Result:

        ```
        data: 123
        city: wh
        province: Unknown
        ```

    -   Example 4: Data mapping in multi\_match mode

        Raw log entry:

        ```
        data: 123
        city: nj,sh
        province: 
        ```

        Transformation rule:

        ```
        e_search_table_map(tab_parse_csv("search,pop,province\ncity:nj,800,js\ncity:sh,2000,sh"), "search", "province",multi_match=True, multi_join=",")
        ```

        Result:

        ```
        data: 123
        city: nj,sh
        province: js,sh
        ```


