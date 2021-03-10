# Table functions

This topic describes the syntax of table functions and provides parameter descriptions and function examples.

## Functions

|Type|Function|Description|
|----|--------|-----------|
|Text-to-table conversion|[tab\_parse\_csv](#section_tsx_vav_cte)|Constructs a table from a delimited text file.|
|Table-to-dictionary conversion|[tab\_to\_dict](#section_bdj_ad6_hrl)|Constructs a dictionary from a table.|

## tab\_parse\_csv

-   Syntax

    ```
    tab_parse_csv(data, sep=',', quote='"', lstrip=True, headers=None, case_insensitive=True)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |data|String|Yes|The text data in a delimited format. Generally, the data is in comma-separated values \(CSV\) format.|
    |sep|String|No|The delimiter used to separate values. By default, the delimiter is a comma \(,\).|
    |quote|String|No|The character used to enclose a value when the value contains the delimiter. Double quotation marks \(""\) are used by default.|
    |lstrip|Bool|No|Specifies whether to trim the leading space characters from each value. Default value: True.|
    |headers|String\\String List|No|The column headers used to parse data. By default, the system retrieves the headers from the first row of the data. If the first row of the data is not used to store headers, you can pass the headers to the function through this parameter. This parameter can be set to a string or a string list.|
    |case\_insensitive|Bool|No|Specifies whether field names are matched in a case-insensitive manner. Default value: True.|

-   Response

    The constructed table is returned.

-   Examples
    -   Example 1: Construct a table and map a field in a raw log entry to the table data. The source Logstore must contain fields in the table so that the mapping between the log data and the table data can be established.

        Raw log entry:

        ```
        city:  nanjing
        ```

        Transformation rule:

        ```
        e_table_map(tab_parse_csv("province,city,pop,gdp\nshanghai,shanghai,2000,1000\njiangsu,nanjing,800,500"), "city", "province")
        ```

        Result:

        ```
        city:  nanjing
        province:  jiangsu
        ```

    -   Example 2: Construct a table and map multiple fields in a raw log entry to the table data.

        Raw log entry:

        ```
        city:  nanjing
        province:  jiangsu
        ```

        Transformation rule:

        ```
        e_table_map(tab_parse_csv("province,city,pop,gdp\nshanghai,shanghai,2000,1000\njiangsu,nanjing,800,500"), ["province", "city"], ["pop", "gdp"])
        ```

        Result:

        ```
        city:  nanjing
        gdp:  500
        pop:  800
        province:  jiangsu
        ```

    -   Example 3: Construct a table and map multiple fields in a raw log entry to the table data. In this example, multiple fields in the raw log entry is not the same as the fields in the table. In the parentheses that include the source fields, the first field is a field of the raw log entry and the second field is a field of the table. In the parentheses that include the destination fields, the first field is the field of the table and the second field is a new field.

        Raw log entry:

        ```
        city:  nanjing
        province:  jiangsu
        ```

        Transformation rule:

        ```
        e_table_map(tab_parse_csv("prov,city,pop,gdp\nshanghai,shanghai,2000,1000\njiangsu,nanjing,800,500"), [("province","prov"), "city"], [("pop", "population"), ("gdp", "GDP")])
        ```

        Result:

        ```
        GDP:  500
        city:  nanjing
        population:  800
        province:  jiangsu
        ```


## tab\_to\_dict

-   Syntax

    ```
    tab_to_dict(table, key_field, value_field, key_join=",", value_join=",")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |table|Table|Yes|The table data from which the dictionary is constructed.|
    |key\_field|String\\String List|Yes|The columns used to construct keys in the dictionary. Separate multiple columns with the character specified by `key_join`.|
    |value\_field|String\\String List|Yes|The columns used to construct values in the dictionary. Separate multiple columns with the character specified by `value_join`.|
    |key\_join|String|No|The string used to separate multiple columns that are used as keys in the dictionary. By default, the string is a comma \(,\).|
    |value\_join|String|No|The string used to separate multiple columns that are used as values in the dictionary. By default, the string is a comma \(,\).|

-   Response

    The constructed dictionary is returned.

-   Examples
    -   Example 1:

        Raw log entry:

        ```
        k1: v1
        city: nj
        ```

        Transformation rule:

        ```
        e_dict_map(tab_to_dict(tab_parse_csv("city,pop\nsh,2000\nnj,800"), "city", "pop"), "city", "popu")
        ```

        Result:

        ```
        k1: v1
        city: nj
        popu: 800
        ```

    -   Example 2:

        Raw log entry:

        ```
        k1: v1
        city: js,nj
        ```

        Transformation rule:

        ```
        e_dict_map(tab_to_dict(tab_parse_csv("province,city,pop\nsh,sh,2000\njs,nj,800"), ["province", "city"], "pop"), "city", "popu")
        ```

        Result:

        ```
        k1: v1
        city: js,nj
        popu: 800
        ```


