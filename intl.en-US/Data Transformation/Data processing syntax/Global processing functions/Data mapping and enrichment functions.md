# Data mapping and enrichment functions

This topic describes the syntax of functions that enrich data through mapping. This topic also provides parameter descriptions and function examples.

## Functions

|Type|Function|Description|
|----|--------|-----------|
|Field-based mapping|[e\_dict\_map](#section_66o_75y_psg)|Maps a specified field value to a field in a dictionary and returns the value of the matched field.|
|[e\_table\_map](#section_s80_usp_myx)|Maps a specified field value to a row in a table and returns the value of the field in this row.|
|Search-based mapping|[e\_search\_dict\_map](#section_pbk_lwq_04u)|Maps a search string to a key in a dictionary and returns the value of the matched key.|
|[e\_search\_table\_map](#section_mp3_goc_rxa)|Maps a search string to a column in a table and returns the value of the field in this column.|

## e\_dict\_map

-   Syntax

    ```
    e_dict_map(data, field, output_field, case_insensitive=True, missing=None, mode="overwrite")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |data|Dict|Yes|The dictionary that is used for mapping. A dictionary consists of a collection of key-value pairs. Each key must be a string.|
    |field|String or string list|Yes|One or more field names. When the value of this parameter contains multiple field names:     -   The system maps the field names in sequence.
    -   If multiple logs match your search condition and the value of the `mode` parameter is overwrite, the last matched log overwrites the previous logs.
    -   If no logs match your search condition, the system returns the value of the `missing` parameter. |
    |output\_field|String|Yes|The field name to return.|
    |case\_insensitive|Bool|No|Specifies whether data is case-sensitive when the system maps the data. Default value: True, indicating that data is case-insensitive. **Note:** If multiple fields in the dictionary matches the search condition and the value of the `case_insensitive` parameter is True, the system selects the field that uses the same case as the key. If no such key is found, the system randomly selects a field. |
    |missing|String|No|The value that is returned when no matched fields are found. Default value: None, indicating that no actions are performed. **Note:** If the specified dictionary contains a mapping rule that returns the `*` wildcard when no matched fields are found, the `missing` parameter becomes invalid because the `*` wildcard has a higher priority than the `missing` parameter. |
    |mode|String|No|The overwrite mode for the field. Default value: overwrite. For more information, see [Field check and overwrite modes](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).|

-   Response

    This function returns an event that contains new field values.

-   Examples
    -   Example 1

        Raw log

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

        Raw log \(three logs\):

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
        message: Other
        ```

        ```
        status:  400
        message: Error
        ```

        ```
        status:  200
        message: Success
        ```


## e\_table\_map

-   Syntax

    ```
    e_table_map(data, field, output_fields, missing=None, mode="fill-auto")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |data|Table|Yes|The table to map values in multiple columns.|
    |field|String, string list, or tuple list|Yes|The source fields that are mapped to the specified table in an event. If no such field is found in the event, no actions are performed on the source field.|
    |output\_fields|String, string list, or tuple list|Yes|The matched fields that are found in the specified table.|
    |missing|String|No|The value that is returned when no matched fields are found. Default value: None, indicating that no actions are performed. If the source field is mapped to multiple columns, the value of the `missing` parameter can be a list of default values. The list length is the same as the length of the source field. **Note:** If the specified dictionary contains a mapping rule that returns the `*` wildcard when no matched fields are found, the `missing` parameter becomes invalid because the `*` wildcard has a higher priority than the `missing` parameter. |
    |mode|String|No|The overwrite mode for a field. Default value: fill-auto. For more information, see [Field check and overwrite modes](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).|

-   Response

    This function returns an event that contains new field values.

-   Default mapping and the `missing` parameter

    In a dictionary, the `*` wildcard has a higher priority than the `missing` parameter. The `missing` parameter is invalid if the `*` wildcard exists.

    If you set the value of a column to the `*` wildcard, the value can match an arbitrary search condition. Example:

    ```
    c1,c2,d1,d2
    c1,*,1,1
    c2,*,2,2,
    *,*,0,0
    ```

-   Examples
    -   Example 1: One field is returned.

        Raw log:

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

        Raw log:

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

    -   Example 3: The sep parameter is used in the tab\_parse\_csv function.

        Raw log:

        ```
        data: 123
        city: nj
        ```

        Transformation rule:

        ```
        e_table_map(tab_parse_csv("city#pop#province\nnj#800#js\nsh#2000#sh", sep='#'), "city", ["province", "pop"])
        ```

        Result

        ```
        data: 123
        city: nj
        province: js
        pop: 800
        ```

    -   Example 4: The quote parameter is used in the tab\_parse\_csv function.

        Raw log:

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

    -   Example 5: The source fields are different from the fields in the specified table.

        Raw log:

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

    -   Example 6: The source fields are different from the fields in the specified table, and the output fields are renamed.

        Raw log:

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


## e\_search\_dict\_map

-   Description

    This function maps a search string to a key in a dictionary and returns the value of the matched key.

-   Syntax

    ```
    e_search_dict_map(data, output_field, multi_match=False, multi_join=" ", missing=None, mode="overwrite")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |data|Dict|Yes|The dictionary that is used for mapping. A dictionary consists of a collection of key-value pairs. Each key must be a string. For more information, see [dct\_get](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Dictionary functions.md).|
    |output\_field|String|Yes|The field name to return.|
    |multi\_match|Bool|No|Specifies whether to return multiple matched fields. Default value: False, indicating that the system returns only the value of the first matched field. If the value of this parameter is True, the system splices the values of multiple matched fields by using the character specified by the `multi_join` parameter.|
    |multi\_join|String|No|The character that is used to splice the values of multiple matched fields. The default value is a space. This parameter is valid when the value of the `multi_match` parameter is True.|
    |missing|String|No|The value that is returned when no matched fields are found. Default value: None, indicating that no actions are performed. **Note:** If the specified dictionary contains a mapping rule that returns the `*` wildcard when no matched fields are found, the `missing` parameter is invalid because the `*` wildcard has a higher priority than the `missing` parameter. |
    |mode|String|No|The overwrite mode for the field. Default value: overwrite. For more information, see [Field check and overwrite modes](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).|

-   Response

    This function returns the matched results.

-   Examples
    -   Example 1: Data mapping in matching mode

        Raw log:

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

    -   Example 2: Data mapping based on the first character of each field

        Raw log:

        ```
        status: 200,300
        ```

        Transformation rule

        ```
        e_search_dict_map ({"status:2??": "ok", "status:3??": "redirect", "status:4??": "auth", "status:5??": "server_error"}, "status_desc", multi_match=True, multi_join="test")
        ```

        Result:

        ```
        status: 200,300
        status_desc: ok test redirect
        ```


## e\_search\_table\_map

-   Description

    This function maps search strings to the fields of a column in a table and returns the matched values in other columns.

-   Syntax

    ```
    e_search_table_map(data, inpt, output_fields, multi_match=False, multi_join=" ", missing=None, mode="fill-auto")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |data|Table|Yes|The table from which data is obtained. The name of one table column must match the search string.|
    |inpt|String|Yes|The field names that match the search string in the specified table.|
    |output\_fields|String, string list, or tuple list|Yes|The matched fields that are found in the specified table. The value of this parameter is a string, string list, or tuple list.|
    |multi\_match|Bool|No|Specifies whether to return multiple matched fields. The default value is False. This indicates that the system returns only the value of the first matched field. If the value of this parameter is True, the system can combine the values of multiple matched fields by using the character that is specified by the `multi_join` parameter.|
    |multi\_join|String|No|The character to combine the values of multiple matched fields. The default value is a space. This parameter is valid when the value of the `multi_match` parameter is True.|
    |missing|String|No|The value that is returned when no matched fields are found. The default value is None. This indicates that no actions are performed. **Note:** If the specified dictionary contains a mapping rule that returns the `*` wildcard when no matched fields are found, the `missing` parameter is invalid because the `*` wildcard has a higher priority than the `missing` parameter. |
    |mode|String|No|The overwrite mode for a field. Default value: fill-auto.|

-   Source table

    |Column name|Type 1|Type 2|
    |-----------|------|------|
    |`content: guide and title:~"\w guide"'`|guide|user|
    |`content: city and title:~"\w shanghai"`|food|home|

-   Response

    This function returns the matched results.

-   Examples
    -   Example 1: Data mapping in simple mode

        Raw log:

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

        Raw log:

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

    -   Example 3: Data mapping in missing mode where the table does not contain the province field

        Raw log:

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

        Raw log:

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


