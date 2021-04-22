# String functions

This topic describes the syntax of string functions and provides parameter descriptions and function examples.

## Functions

|Type|Function|Description|
|----|--------|-----------|
|Multi-string operation|[str\_format](#section_z9w_lhq_1np)|Formats a string.|
|[str\_join](#section_m6w_z5p_dsz)|Concatenates the elements in a sequence with a specified string to generate a new string.|
|[str\_zip](#section_3k5_k2o_9nv)|Splits two value strings or expression strings concurrently and then merges them into a single string.|
|Sorting, reversing, and replacement|[str\_sort](#section_25v_pnh_lvu)|Sorts specified objects.|
|[str\_reverse](#section_tj4_8br_687)|Reverses a string.|
|[str\_replace](#section_vjs_f3r_7dm)|Replaces an old string with a new string based on an applicable rule.|
|[str\_logstash\_config\_normalize](#section_4r2_umx_8z8)|Converts the data format of the Logstash configuration language into the JSON format.|
|[str\_translate](#section_r2e_pue_p9l)|Replaces the specified characters of a string with the corresponding characters.|
|Regular munging|[str\_strip](#section_zbr_fn2_d5q)|Deletes specified characters from a string.|
|[str\_lstrip](#section_m5v_v05_d6m)|Deletes specified characters from the start of a string.|
|[str\_rstrip](#section_jv3_cjm_uli)|Deletes specified characters from the end of a string.|
|[str\_lower](#section_d7o_z79_1ok)|Converts all uppercase characters in a string to lowercase characters.|
|[str\_upper](#section_9iy_ias_kkw)|Converts all lowercase characters in a string to uppercase characters.|
|[str\_title](#section_d6d_haf_4f5)|Capitalizes only the first letter of all words.|
|[str\_capitalize](#section_hel_e1z_8yx)|Capitalizes only the first letter of a string.|
|[str\_swapcase](#section_s56_fi4_znz)|Interchanges uppercase letters and lowercase letters in a string.|
|Search and check|[str\_count](#section_4pc_0pt_x5r)|Counts the number of occurrences of a character in a string.|
|[str\_find](#section_v2o_zo8_qwp)|Checks whether an original string contains a specified substring.|
|[str\_rfind](#section_xcw_opp_d0v)|Locates the position of the last occurrence of a character or a substring in a string.|
|[str\_endswith](#section_1av_zcw_a43)|Checks whether a string ends with a specified suffix.|
|[str\_startswith](#section_p71_nsr_1vq)|Checks whether a string starts with a specified string.|
|Splitting|[str\_split](#section_whb_ze9_1zp)|Splits a string with a specified delimiter.|
|[str\_splitlines](#section_1uo_y5n_b81)|Splits a string with a line feed.|
|[str\_partition](#section_brb_4tj_43x)|Splits a string with a specified delimiter into three parts from left to right.|
|[str\_rpartition](#section_sut_jtp_tas)|Splits a string with a specified delimiter into three parts from right to left.|
|Formatting|[str\_center](#section_w4h_xu5_6dp)|Pads a string to a specified length with a specified character.|
|[str\_ljust](#section_ku6_3qi_jdm)|Pads a string to a specified length from the end with a specified character.|
|[str\_rjust](#section_emy_k2l_9ha)|Pads a string to a specified length from the start with a specified character.|
|[str\_zfill](#section_vah_zsq_2h3)|Pads a string to a specified length from the start with 0.|
|[str\_expandtabs](#section_gkt_nd5_o46)|Converts `\t` in a string into a space character.|
|Character set check|[str\_isalnum](#section_x55_wog_z0t)|Checks whether a string consists of only letters and digits.|
|[str\_isalpha](#section_8zu_n32_lpq)|Checks whether a string consists of only letters.|
|[str\_isascii](#section_8vq_m57_0b7)|Checks whether a string is in the ASCII table.|
|[str\_isdecimal](#section_tn6_eey_vgw)|Checks whether a string contains only decimal characters.|
|[str\_isdigit](#section_kdk_w45_leh)|Checks whether a string consists of only digits.|
|[str\_isidentifier](#section_0wn_tah_jo8)|Checks whether a string is a valid Python identifier. This function can also check whether a variable name is valid.|
|[str\_islower](#section_0q1_rks_gvv)|Checks whether a string consists of lowercase letters.|
|[str\_isnumeric](#section_b92_h5s_t4w)|Checks whether a string consists of digits.|
|[str\_isprintable](#section_x2j_ptg_7dc)|Checks whether all characters in a string are printable characters.|
|[str\_isspace](#section_4hy_915_ljz)|Checks whether a string consists of only space characters.|
|[str\_istitle](#section_ttk_bx5_bd7)|Checks whether the first letter of all words in a string are uppercase and whether other letters in the string are lowercase.|
|[str\_isupper](#section_vns_pxu_9q3)|Checks whether all letters in a string are uppercase letters.|

The following table describes the functions that can be used together with string functions.

|Type|Function|Description|
|----|--------|-----------|
|Multi-string operation|[op\_add](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Operator functions.md)|Performs a logic AND operation of multiple numeric values. This function can be used for numeric values and strings.|
|[op\_max](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Operator functions.md)|Calculates the maximum value among multiple values. This function can be used for numeric values and strings.|
|[op\_min](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Operator functions.md)|Calculates the minimum value among multiple values. This function can be used for numeric values and strings.|
|String truncation|[op\_slice](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Operator functions.md)|Truncates a string.|
|Length calculation|[op\_len](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Operator functions.md)|Calculates the length of a string.|

## str\_format

-   Syntax

    ```
    str_format(formatted string, value 1, value 2, ...)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |formatted string|Arbitrary \(automatically converted to a string\)|Yes|The format of the converted string, for example, `{}={}`.|
    |value 1|Arbitrary|Yes|Value 1 to be formatted.|
    |value 2|Arbitrary|Yes|Value 2 to be formatted.|

-   Response

    A formatted string is returned.

-   Example

    Raw log entry:

    ```
    class: Format
    escape_name: Traditional
    ```

    Transformation rule:

    ```
    e_set("str_format", str_format("{}={}",v("class"),v("escape_name")))
    ```

    Result:

    ```
    class: Format
    escape_name: Traditional
    str_format: Format=Traditional
    ```


## str\_join

-   Syntax

    ```
    Str_join(string delimiter, value 1, value 2, ...)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |string delimiter|Arbitrary \(automatically converted to a string\)|Yes|The identifier of a string delimiter, for example, `!`, `@`, `#`, `$`, and `%`.|
    |value 1|Arbitrary \(automatically converted to a string\)|Yes|The value to be concatenated.|
    |value 2|Arbitrary \(automatically converted to a string\)|Yes|The value to be concatenated.|

-   Response

    A concatenated string is returned.

-   Example

    Raw log entry:

    ```
    name: ETL
    company: aliyun.com
    ```

    Transformation rule:

    ```
    e_set("email", str_join("@",v("name"),v("company")))
    ```

    Result:

    ```
    name: ETL
    company: aliyun.com
    email:ETL@aliyun.com
    ```


## str\_replace

-   Syntax

    ```
    str_replace(value, old,new,count)
    ```

    **Note:** This function is a basic method to call variable parameters. For more information, see [Function invoking](/intl.en-US/Data Transformation/Data processing syntax/Basic syntax.md).

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The value to be replaced.|
    |old|Arbitrary \(automatically converted to a string\)|Yes|The string to be replaced.|
    |new|Arbitrary \(automatically converted to a string\)|Yes|The new string after the replacement.|
    |count|Number|No|The number of replacements. If you do not specify this parameter, all old strings are replaced.|

-   Response

    A new string is returned.

-   Example: The following example converts a dictionary to a JSON string.

    Raw log entry:

    ```
    content:  {'referer': '-', 'request': 'GET /phpMyAdmin', 'status': 404, 'data-1': {'aaa': 'Mozilla', 'bbb': 'asde'}, 'data-2': {'up_adde': '-', 'up_host': '-'}}
    ```

    Transformation rule:

    ```
    e_set("content_json", str_replace(ct_str(v("content")),"'",'"'))
    ```

    Result:

    ```
    content:  {'referer': '-', 'request': 'GET /phpMyAdmin', 'status': 404, 'data-1': {'aaa': 'Mozilla', 'bbb': 'asde'}, 'data-2': {'up_adde': '-', 'up_host': '-'}}
    content_json:  {"referer": "-", "request": "GET /phpMyAdmin", "status": 404, "data-1": {"aaa": "Mozilla", "bbb": "asde"}, "data-2": {"up_adde": "-", "up_host": "-"}}
    ```


## str\_sort

-   Syntax

    ```
    str_sort(value, reverse)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The original string to be sorted.|
    |reverse|Bool|No|Default value: False. This value indicates that the string is sorted in ascending order.|

-   Response

    A sorted string is returned.

-   Examples
    -   Example 1: This example sorts the str string in alphabetical order.

        Raw log entry:

        ```
        str: twish
        ```

        Transformation rule:

        ```
        e_set("str_sort", str_sort(v("str")))
        ```

        Result:

        ```
        str: twish
        str_sort: histw
        ```

    -   Example 2: This example sorts the str string in two-letter pairs in reverse alphabetical order.

        Raw log entry:

        ```
        str: twish
        ```

        Transformation rule:

        ```
        e_set("str_sort", str_sort(v("str"),True))
        ```

        Result:

        ```
        str: twish
        str_sort: wtsih
        ```


## str\_reverse

-   Syntax

    ```
    str_reverse(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The value to be reversed.|

-   Response

    A converted string is returned.

-   Example: The following function reverses the data string.

    Raw log entry:

    ```
    data:twish
    ```

    Transformation rule:

    ```
    e_set("reverse_data", str_reverse(v("data")))
    ```

    Result:

    ```
    data:twish
    reverse_data:hsiwt
    ```


## str\_logstash\_config\_normalize

-   Syntax

    ```
    str_logstash_config_normalize (value)
    ```

    **Note:** For more information about the Logstash configuration language, see [Logstash](http://logstash-docs.elasticsearch.org.s3.amazonaws.com/_logstash_config_language.html).

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The value to be converted.|

-   Response

    A converted string is returned.

-   Example: The following example converts the logstash string.

    Raw log entry:

    ```
    logstash: {"name"=>"tw5"}
    ```

    Transformation rule:

    ```
    e_set("normalize_data", str_logstash_config_normalize(v("logstash")))
    ```

    Result:

    ```
    logstash: {"name"=>"tw5"}
    normalize_data:{"name":"tw5"}
    ```


## str\_strip

-   Syntax

    ```
    str_strip(value, chars )
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The original string to be modified.|
    |chars|Arbitrary \(automatically converted to a string\)|No|The character set to be deleted from the start and end of a string. Default value: `\t\r\n`.|

-   Response

    A modified string is returned.

-   Examples
    -   Example 1: This example deletes the "\*" character from the start of the strip string.

        Raw log entry:

        ```
        strip: ***I love Etl
        ```

        Transformation rule:

        ```
        e_set("str_strip", str_strip(v("strip"),"*"))
        ```

        Result:

        ```
        strip: ***I love Etl
        str_strip:I love Etl
        ```

    -   Example 2: This example deletes the space character from the start of the strip string.

        Raw log entry:

        ```
        strip:    I love Etl
        ```

        Transformation rule:

        ```
        e_set("str_strip", str_strip(v("strip")))
        ```

        Result:

        ```
        strip:    I love Etl
        str_strip: I love Etl
        ```

    -   Example 3: This example deletes the `xy` character set.

        Raw log entry:

        ```
        strip:xy123yx
        ```

        Transformation rule:

        ```
        e_set("str_strip", str_strip(v("strip"),"xy"))
        ```

        Result:

        ```
        strip:xy123yx
        str_strip:123
        ```


## str\_lower

-   Syntax

    ```
    str_lower(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The string to be converted.|

-   Response

    A converted string is returned.

-   Example: The following example converts the name string into lowercase letters.

    Raw log entry:

    ```
    name: Etl
    ```

    Transformation rule:

    ```
    e_set("str_lower", str_lower(v("name")))
    ```

    Result:

    ```
    name: Etl
    str_lower: etl
    ```


## str\_upper

-   Syntax

    ```
    str_lower(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The string to be converted.|

-   Response

    A converted string is returned.

-   Example: The following example converts the name string into uppercase letters.

    Raw log entry:

    ```
    name: etl
    ```

    Transformation rule:

    ```
    e_set("str_upper", str_upper(v("name")))
    ```

    Result:

    ```
    name: etl
    str_upper: ETL
    ```


## str\_title

-   Syntax

    ```
    str_title(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The string to be converted.|

-   Response

    A converted string is returned.

-   Example: The following example converts the first letters of all words in the word string into uppercase letters.

    Raw log entry:

    ```
    word: this is etl
    ```

    Transformation rule:

    ```
    e_set("str_title", str_title(v("word")))
    ```

    Result:

    ```
    word: this is etl
    str_title: This Is Etl
    ```


## str\_capitalize

-   Syntax

    ```
    str_capitalize(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The string to be converted.|

-   Response

    A converted string is returned.

-   Example: The following example converts the letters of all words in the word string into lowercase letters.

    Raw log entry:

    ```
    word: this Is MY EAL
    ```

    Transformation rule:

    ```
    e_set("str_capitalize", str_capitalize(v("word")))
    ```

    Result:

    ```
    word: this Is MY EAL
    str_capitalize: This is my eal
    ```


## str\_lstrip

-   Syntax

    ```
    str_lstrip(value,chars)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The original string to be modified|
    |chars|Arbitrary \(automatically converted to a string\)|No|The character set to be deleted from the start of a string. The default value is the space character.|

-   Response

    A modified string is returned.

-   Examples
    -   Example 1: This example deletes the "\*" character from the start of the word string.

        Raw log entry:

        ```
        word: ***this is string
        ```

        Transformation rule:

        ```
        e_set("str_lstrip", str_lstrip(v("word"),"*"))
        ```

        Result:

        ```
        word: ***this is string
        str_lstrip: this is string
        ```

    -   Example 2: This example deletes the space character from the start of the word string.

        Raw log entry:

        ```
        word:     this is string
        ```

        Transformation rule:

        ```
        e_set("str_lstrip", str_lstrip(v("word")))
        ```

        Result:

        ```
        word:     this is string
        str_lstrip: this is string
        ```

    -   Example 3: This example deletes the `xy` character set.

        Raw log entry:

        ```
        lstrip:xy123yx
        ```

        Transformation rule:

        ```
        e_set("str_lstrip", str_lstrip(v("lstrip"),"xy"))
        ```

        Result:

        ```
        lstrip:xy123yx
        str_lstrip:123yx
        ```


## str\_rstrip

-   Syntax

    ```
    str_rstrip(value, chars)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The original string to be modified.|
    |chars|Arbitrary \(automatically converted to a string\)|No|The character set to be deleted from the end of a string. The default value is the space character.|

-   Response

    A modified string is returned.

-   Examples
    -   Example 1: This example deletes the "\*" character from the end of the word string.

        Raw log entry:

        ```
        word: this is string*****
        ```

        Transformation rule:

        ```
        e_set("str_rstrip", str_rstrip(v("word"),"*"))
        ```

        Result:

        ```
        word: this is string*****
        str_rstrip: this is string
        ```

    -   Example 2: This example deletes the `xy` character set.

        Raw log entry:

        ```
        word:xy123yx
        ```

        Transformation rule:

        ```
        e_set("str_rstrip", str_rstrip(v("word"),"xy"))
        ```

        Result:

        ```
        word:xy123yx
        str_rstrip:xy123
        ```


## str\_swapcase

-   Syntax

    ```
    str_swapcase(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The string to be converted.|

-   Response

    A converted string is returned.

-   Example

    Raw log entry:

    ```
    name: this is string
    ```

    Transformation rule:

    ```
    e_set("str_swapcase", str_swapcase(v("name")))
    ```

    Result:

    ```
    name: this is string
    str_swapcase: THIS IS STRING
    ```


## str\_translate

-   Syntax

    ```
    str_translate(value, original character set, mapping character set)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The original string to be replaced.|
    |original character set|Arbitrary \(automatically converted to a string\)|Yes|The original character set.|
    |mapping character set|Arbitrary \(automatically converted to a string\)|Yes|The new character set after the replacement.|

-   Response

    A replaced string is returned.

-   Example

    Raw log entry:

    ```
    name: I love ETL!!!
    ```

    Transformation rule:

    ```
    e_set("str_translate", str_translate(v("name"),"aeiou","12345"))
    ```

    Result:

    ```
    name: I love ETL!!!
    str_translate: I l4v2 ETL!!!
    ```


## str\_endswith

-   Syntax

    ```
    tr_endwith(value, suffix, start, end )
    ```

    **Note:** This function is a basic method to call variable parameters. For more information, see [Function invoking](/intl.en-US/Data Transformation/Data processing syntax/Basic syntax.md).

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The original string to be checked.|
    |suffix|Arbitrary \(automatically converted to a string\)|Yes|The suffix rule. This parameter can be a string or an element.|
    |start|Number|No|The position where the string suffix check starts. The value 0 indicates the first character and the value -1 indicates the last character. |
    |end|Number|No|The position where the string suffix check ends. The value 0 indicates the first character and the value -1 indicates the last character. |

-   Response

    The value True is returned if the string ends with the specified suffix. Otherwise, the value False is returned.

-   Example

    Raw log entry:

    ```
    name: this is endswith!!!
    ```

    Transformation rule:

    ```
    e_set("str_endswith",str_endswith(v("name"),"!"))
    ```

    Result:

    ```
    name: this is endswith!!!
    str_endswith: True
    ```


## str\_startswith

-   Syntax

    ```
    str_startswith(value, prefix, start, end)
    ```

    **Note:** This function is a basic method to call variable parameters. For more information, see [Function invoking](/intl.en-US/Data Transformation/Data processing syntax/Basic syntax.md).

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The original string to be checked.|
    |prefix|Arbitrary\(automatically converted to a string\)|Yes|The prefix rule. This parameter can be a string or an element.|
    |start|Number|No|The position where the string prefix check starts. The value 0 indicates the first character and the value -1 indicates the last character. |
    |end|Number|No|The position where the string prefix check ends. The value 0 indicates the first character and the value -1 indicates the last character. |

-   Response

    The value True is returned if the string starts with the specified prefix. Otherwise, the value False is returned.

-   Example

    Raw log entry:

    ```
    name: !! this is startwith
    ```

    Transformation rule:

    ```
    e_set("str_startswith",str_startswith(v("name"),"!!"))
    ```

    Result:

    ```
    name: !! this is startwith
    str_startswith: True
    ```


## str\_find

-   Syntax

    ```
    str_find(value,str, begin, end)
    ```

    **Note:** This function is a basic method to call variable parameters. For more information, see [Function invoking](/intl.en-US/Data Transformation/Data processing syntax/Basic syntax.md).

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The original string in which you search for a substring.|
    |str|Arbitrary \(automatically converted to a string\)|Yes|The substring you enter for search.|
    |begin|Number|No|The position where the substring search starts. The value 0 indicates the first character and the value -1 indicates the last character. |
    |end|Number|No|The position where the substring search ends. The default value is the length of the string. The value 0 indicates the first character and the value -1 indicates the last character. |

-   Response

    The position of the specified substring in the original string is returned.

-   Example

    Raw log entry:

    ```
    name: hello world
    ```

    Transformation rule:

    ```
    e_set("str_find",str_find(v("name"),"h"))
    ```

    Result:

    ```
    name: hello world
    str_find: 0
    ```


## str\_count

-   Syntax

    ```
    str_count(value,sub,start,end)
    ```

    **Note:** This function is a basic method to call variable parameters. For more information, see [Function invoking](/intl.en-US/Data Transformation/Data processing syntax/Basic syntax.md).

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The original string in which the number of occurrences of a specified character is counted.|
    |sub|Arbitrary \(automatically converted to a string\)|Yes|The character whose number of occurrences is counted.|
    |start|Number|No|The position where the search for the specified character starts in the string. The value 0 indicates the first character and the value -1 indicates the last character. |
    |end|Number|No|The position where the search for the specified character ends in the string. The default value is the last character of the string. The value 0 indicates the first character and the value -1 indicates the last character. |

-   Response

    The number of occurrences of the specified character is returned.

-   Example

    Raw log entry:

    ```
    name: this is really a string
    ```

    Transformation rule:

    ```
    e_set("str_count",str_count(v("name"),"i"))
    ```

    Result:

    ```
    name: this is really a string
    str_count: 3
    ```


## str\_rfind

-   Syntax

    ```
    str_rfind(value,substr,beg,end)
    ```

    **Note:** This function is a basic method to call variable parameters. For more information, see [Function invoking](/intl.en-US/Data Transformation/Data processing syntax/Basic syntax.md).

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The original string in which you search for a specified character.|
    |substr|Arbitrary \(automatically converted to a string\)|Yes|The character you enter for search.|
    |beg|Number|No|The position where the search starts. Default value: 0.|
    |end|Number|No|The position where the search ends. The default value is the length of the string.|

-   Response

    The position of the last occurrence of the specified character is returned.

-   Example

    Raw log entry:

    ```
    name: this is really a string
    ```

    Transformation rule:

    ```
    e_set("str_rfind",str_rfind(v("name"),"i"))
    ```

    Result:

    ```
    name: this is really a string
    str_rfind: 20
    ```


## str\_split

-   Syntax

    ```
    str_split(value, sep=None, maxsplit=-1)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The original string to be split.|
    |sep|Number|No|The delimiter. The value None indicates the space character.|
    |maxsplit|Number|No|The maximum number of splits. The value -1 indicates that no limit is set.|

-   Response

    A split string is returned.

-   Example: The following example splits the content string with a space character.

    Raw log entry:

    ```
    content: hello world
    ```

    Transformation rule:

    ```
    e_set("str_split",str_split(v("content")," "))
    ```

    Result:

    ```
    content: hello world
    str_split: ["hello", "world"]
    ```


## str\_splitlines

-   Syntax

    ```
    str_splitlines(value,keepends)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The original string to be split.|
    |keepends|Bool|No|Indicates whether to delete line feeds \(`\r`, `\r\n`, and `\n`\) from the result. The default value is False, which indicates that line feeds are deleted. The value True indicates that line feeds are retained.|

-   Response

    A list of split string is returned.

-   Examples
    -   Example 1:

        Raw log entry:

        ```
        content: ab c\n\nde fg\rkl\r\n
        ```

        Transformation rule:

        ```
        e_set("str_splitlines",str_splitlines(v("content"),False))
        ```

        Result:

        ```
        content: ab c\n\nde fg\rkl\r\n
        str_splitlines: ['ab c', '', 'de fg', 'kl']
        ```

    -   Example 2:

        Raw log entry:

        ```
        content: ab c\n\nde fg\rkl\r\n
        ```

        Transformation rule:

        ```
        e_set("str_splitlines",str_splitlines(v("content"),True))
        ```

        Result:

        ```
        content: ab c\n\nde fg\rkl\r\n  
        str_splitlines: ['ab c\n', '\n', 'de fg\r', 'kl\r\n']
        ```


## str\_partition

-   Syntax

    ```
    str_partition(value,substr)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The string to be split.|
    |substr|Arbitrary \(automatically converted to a string\)|No|The specified delimiter.|

-   Response

    A split string is returned.

-   Example: The following example splits the website string with periods \(`.`\) into three parts from left to right.

    Raw log entry:

    ```
    website: www.aliyun.com
    ```

    Transformation rule:

    ```
    e_set("str_partition",str_partition(v("website"),"."))
    ```

    Result:

    ```
    website: www.aliyun.com
    str_partition:  ["www", ".", "aliyun.com"]
    ```


## str\_rpartition

-   Syntax

    ```
    str_rpartition(value,substr)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The original string to be split.|
    |substr|Arbitrary \(automatically converted to a string\)|No|The specified delimiter.|

-   Response

    A list of split string is returned.

-   Example: The following example splits the website string with periods \(`.`\) into three parts from right to left.

    Raw log entry:

    ```
    website: www.aliyun.com
    ```

    Transformation rule:

    ```
    e_set("str_partition",str_rpartition(v("website"),"."))
    ```

    Result:

    ```
      website: www.aliyun.com
      str_partition: ["www.aliyun", ".", "com"]
    ```


## str\_center

-   Syntax

    ```
    str_center(value,width,fillchar)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The original string to be modified.|
    |width|Number|Yes|The length of the padded string.|
    |fillchar|Arbitrary \(automatically converted to a string\)|No|The pad character. The default pad character is the space character.|

-   Response

    A padded string is returned.

    **Note:** If the length of the padded string is shorter than that of the original string, the original string is returned.

-   Examples
    -   Example 1: This example pads a string with asterisks \(`*`\).

        Raw log entry:

        ```
        center: this is center
        ```

        Transformation rule:

        ```
        e_set("str_center",str_center(v("center"),width=40,fillchar="*"))
        ```

        Result:

        ```
        center: this is center
        str_center: *************this is center*************
        ```

    -   Example 2: This example pads a string with space characters.

        Raw log entry:

        ```
        center: this is center
        ```

        Transformation rule:

        ```
        e_set("str_center",str_center(v("center"),width=40))
        ```

        Result:

        ```
        center: this is center
        str_center:              this is center 
        ```


## str\_zfill

-   Syntax

    ```
    str_zfill(value,width)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The original string to be modified.|
    |width|Number|Yes|The length of the padded string.|

-   Response

    A padded string is returned.

-   Example

    Raw log entry:

    ```
    center: this is zfill
    ```

    Transformation rule:

    ```
    e_set("str_zfill",str_zfill(v("center"),width=40))
    ```

    Result:

    ```
    center: this is zfill
    str_zfill: 00this is zfill
    ```


## str\_expandtabs

-   Syntax

    ```
    str_expandtabs(value,tabsize)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The original string to be modified.|
    |tabsize|Number|Required|Specifies the number of space characters after the conversion.|

-   Response

    A converted string is returned.

-   Examples
    -   Example 1: This example converts the `\t` character in the logstash string into a space character.

        Raw log entry:

        ```
        logstash: this is\tstring
        ```

        Transformation rule:

        ```
        e_set("str_expandtabs",str_expandtabs(v("center")))
        ```

        Result:

        ```
        logstash: this is\tstring
        str_expandtabs: this is string
        ```

    -   Example 2: This example converts the `\t` character in the center string into a space character.

        Raw log entry:

        ```
        center: this is\tstring
        ```

        Transformation rule:

        ```
        e_set("str_expandtabs",str_expandtabs(v("center"),16))
        ```

        Result:

        ```
        center: this is\tstring
        str_expandtabs: this is         string
        ```


## str\_ljust

-   Syntax

    ```
    str_ljust(value,width,fillchar)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The original string to be modified.|
    |width|Number|Yes|The length of the padded string.|
    |fillchar|Arbitrary \(automatically converted to a string\)|No|The pad character. The default pad character is the space character.|

-   Response

    A padded string is returned.

    **Note:** If the length of the padded string is shorter than that of the original string, the original string is returned.

-   Examples
    -   Example 1:

        Raw log entry:

        ```
        content: this is ljust
        ```

        Transformation rule:

        ```
        e_set("str_ljust",str_ljust(v("content"),20,"*"))
        ```

        Result:

        ```
        content: this is ljust
        str_ljust: this is ljust*******
        ```

    -   Example 2:

        Raw log entry:

        ```
        center: this is ljust
        ```

        Transformation rule:

        ```
        e_set("str_ljust",str_ljust(v("center"),20,))
        ```

        Result:

        ```
        center: this is ljust
        str_ljust: this is ljust   
        ```

    -   Example 3: In this example, the original string is returned because the length of the padded string is shorter than the length of the original string.

        Raw log entry:

        ```
        center: this is ljust
        ```

        Transformation rule:

        ```
        e_set("str_ljust",str_ljust(v("center"),10,"*"))
        ```

        Result:

        ```
        center: this is ljust
        str_ljust: this is ljust
        ```


## str\_rjust

-   Syntax

    ```
    str_ljust(value,width,fillchar)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The original string to be modified.|
    |width|Number|Yes|The length of the padded string.|
    |fillchar|Arbitrary \(automatically converted to a string\)|No|The pad character. The default pad character is the space character.|

-   Response

    A padded string is returned.

    **Note:** If the length of the padded string is shorter than that of the original string, the original string is returned.

-   Example

    Raw log entry:

    ```
    center: this is rjust
    ```

    Transformation rule:

    ```
    e_set("str_rjust",str_rjust(v("center"),20,"*"))
    ```

    Result:

    ```
    center: this is rjust
    str_rjust: *******this is rjust
    ```


## str\_zip

-   Syntax

    ```
    str_zip(value1, value2, combine_sep=None, sep=None, quote=None, lparse=None, rparse=None)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value 1|Arbitrary \(automatically converted to a string\)|Yes|A value to be merged.|
    |value 2|Arbitrary \(automatically converted to a string\)|Yes|A value to be merged.|
    |combine\_sep|Arbitrary \(automatically converted to a string\)|No|The identifier used to merge the elements. Default value: `#`.|
    |sep|Arbitrary \(automatically converted to a string\)|No|The delimiter between merged elements. The value must be a single character. Default value: `,`.|
    |quote|Arbitrary \(automatically converted to a string\)|No|The character that encloses the merged elements. This parameter is required when the merged elements contain a delimiter. Default value: `"`.|
    |lparse|Arbitrary \(automatically converted to a string\)|No|The delimiter and quote used among the elements of `value 1`. The default delimiter is `,` and the default quote is `"`. The format is `lparse=(',', '"')`. **Note:** The quote takes precedence over the delimiter. |
    |rparse|Arbitrary \(automatically converted to a string\)|No|The delimiter and quote used among the elements of `value 2`. The default delimiter is `,` and the default quote is `"`. The format is `rparse=(',', '"')`. **Note:** The quote takes precedence over the delimiter. |

-   Response

    A merged string is returned.

-   Examples
    -   Example 1:

        Raw log entry:

        ```
        website: wwww.aliyun.com
        escape_name: o
        ```

        Transformation rule:

        ```
        e_set("combine",str_zip(v("website"),v("escape_name"), combine_sep="@"))
        ```

        Result:

        ```
        website: wwww.aliyun.com
        escape_name: o
        combine: wwww.aliyun.com@o
        ```

    -   Example 2:

        Raw log entry:

        ```
        website: wwww.aliyun.com
        escape_name: o
        ```

        Transformation rule:

        ```
        e_set("combine",str_zip(v("website"),v("escape_name")))
        ```

        Result:

        ```
        website: wwww.aliyun.com
        escape_name: o
        wwww.aliyun.com#o  
        ```

    -   Example 3: In this example, the sep parameter is specified.

        Raw log entry:

        ```
        f1: a,b,c
        f2: x,y,z
        ```

        Transformation rule:

        ```
        e_set("combine",str_zip(v("f1"), v("f2"), sep="|"))
        ```

        Result:

        ```
        f1: a,b,c
        f2: x,y,z
        combine: a#x|b#y|c#z
        ```

    -   Example 4: In this example, the quote parameter is specified.

        Raw log entry:

        ```
        f1: "a,a", b, "c,c"
        f2: x, "y,y", z
        ```

        Transformation rule:

        ```
        e_set("combine",str_zip(v("f1"), v("f2"), quote='|'))
        ```

        Result:

        ```
        f1: "a,a", b, "c,c"
        f2: x, "y,y", z
        combine: |a,a#x|,|b#y,y|,|c,c#z|
        ```

    -   Example 5: In this example, elements of different lengths are used.

        Raw log entry:

        ```
        f1: a,b
        f2: x,y,z
        ```

        Transformation rule:

        ```
        e_set("combine",str_zip(v("f1"), v("f2")))
        ```

        Result:

        ```
        f1: a,b
        f2: x,y,z
        combine: a#x,b#y
        ```

    -   Example 6: In this example, the lparse and rparse parameters are specified.

        Raw log entry:

        ```
        f1: a#b#c
        f2: x|y|z
        ```

        Transformation rule:

        ```
        e_set("combine",str_zip(v("f1"), v("f2"), lparse=("#", '"'), rparse=("|", '"')))
        ```

        Result:

        ```
        f1: a#b#c
        f2: x|y|z
        combine: a#x,b#y,c#z
        ```

    -   Example 7: In this example, the lparse and rparse parameter are specified.

        Raw log entry:

        ```
        f1: |a,a|, b, |c,c|
        f2: x, #y,y#, z
        ```

        Transformation rule:

        ```
        e_set("combine",str_zip(v("f1"), v("f2"), lparse=(",", '|'), rparse=(",", '#')))
        ```

        Result:

        ```
        f1: |a,a|, b, |c,c|
        f2: x, #y,y#, z
        combine: "a,a#x","b#y,y","c,c#z"
        ```


## str\_isalnum

-   Syntax

    ```
    str_isalnum(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The string to be checked.|

-   Response

    The value True or False is returned.

-   Example

    Raw log entry:

    ```
    content: 13
    ```

    Transformation rule:

    ```
    e_set("str_isalnum",str_isalnum(v("content")))
    ```

    Result:

    ```
    content: 13
    str_isalnum: True
    ```


## str\_isalpha

-   Syntax

    ```
    str_isalpha(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The string to be checked.|

-   Response

    The value True or False is returned.

-   Example

    Raw log entry:

    ```
    content: 13
    ```

    Transformation rule:

    ```
    e_set("str_isalpha",str_isalpha(v("content")))
    ```

    Result:

    ```
    content: 13
    str_isalpha: False
    ```


## str\_isascii

-   Syntax

    ```
    str_isascii(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The string to be checked.|

-   Response

    The value True or False is returned.

-   Example

    Raw log entry:

    ```
    content: asw123
    ```

    Transformation rule:

    ```
    e_set("str_isascii",str_isascii(v("content")))
    ```

    Result:

    ```
    content: asw123
    str_isascii: True
    ```


## str\_isdecimal

-   Syntax

    ```
    str_isdecimal(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The string to be checked.|

-   Response

    The value True or False is returned.

-   Examples
    -   Example 1:

        Raw log entry:

        ```
        welcome: hello
        ```

        Transformation rule:

        ```
        e_set("str_isdecimal",str_isdecimal(v("welcome")))
        ```

        Result:

        ```
        welcome: hello
        str_isdecimal: False
        ```

    -   Example 2:

        Raw log entry:

        ```
        num: 123
        ```

        Transformation rule:

        ```
        e_set("str_isdecimal",str_isdecimal(v("num")))
        ```

        Result:

        ```
        num: 123
        str_isdecimal: True
        ```


## str\_isdigit

-   Syntax

    ```
    str_isdigit(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The string to be checked.|

-   Response

    The value True or False is returned.

-   Example

    Raw log entry:

    ```
    content: 13
    ```

    Transformation rule:

    ```
    e_set("str_isdigit",str_isdigit(v("content")))
    ```

    Result:

    ```
    content: 13
    str_isdigit: True
    ```


## str\_isidentifier

-   Syntax

    ```
    str_isidentifier(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The string to be checked.|

-   Response

    The value True or False is returned.

-   Example

    Raw log entry:

    ```
    class: class
    ```

    Transformation rule:

    ```
    e_set("str_isidentifier",str_isidentifier(v("class")))
    ```

    Result:

    ```
    class: class
    str_isidentifier: True
    ```


## str\_islower

-   Syntax

    ```
    str_islower(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The string to be checked.|

-   Response

    The value True or False is returned.

-   Example

    Raw log entry:

    ```
    lower: asds
    ```

    Transformation rule:

    ```
    e_set("str_islower",str_islower(v("lower")))
    ```

    Result:

    ```
    lower: asds
    str_islower: True
    ```


## str\_isnumeric

-   Syntax

    ```
    str_isnumeric(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The string to be checked.|

-   Response

    The value True or False is returned.

-   Example

    Raw log entry:

    ```
    num: 123
    ```

    Transformation rule:

    ```
    e_set("str_isnumeric",str_isnumeric(v("num")))
    ```

    Result:

    ```
    num: 123
    str_isnumeric: True
    ```


## str\_isprintable

-   Syntax

    ```
    str_isprintable(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The string to be checked.|

-   Response

    The value True or False is returned.

-   Example

    Raw log entry:

    ```
    content: vs;,.as
    ```

    Transformation rule:

    ```
    e_set("str_isprintable",str_isprintable(v("content")))
    ```

    Result:

    ```
    content: vs;,.as
    str_isprintable: True
    ```


## str\_isspace

-   Syntax

    ```
    str_isspace(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The string to be checked.|

-   Response

    The value True or False is returned.

-   Example

    Raw log entry:

    ```
    space:  as afsd
    ```

    Transformation rule:

    ```
    e_set("str_isspace",str_isspace(v("space")))
    ```

    Result:

    ```
    space:  as afsd
    str_isspace: False
    ```


## str\_istitle

-   Syntax

    ```
    str_istitle(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The string to be checked.|

-   Response

    The value True or False is returned.

-   Example

    Raw log entry:

    ```
    title: Alex
    ```

    Transformation rule:

    ```
    e_set("str_istitle",str_istitle(v("title")))
    ```

    Result:

    ```
    title: Alex
    str_istitle: True
    ```


## str\_isupper

-   Syntax

    ```
    str_istitle(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary \(automatically converted to a string\)|Yes|The string to be checked.|

-   Response

    The value True or False is returned.

-   Example

    Raw log entry:

    ```
    content: ASSD
    ```

    Transformation rule:

    ```
    e_set("str_isupper",str_isupper(v("content")))
    ```

    Result:

    ```
    content: ASSD
    str_isupper: True
    ```


## str\_uuid

-   Syntax

    ```
    str_uuid(lower=True)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |lower|Bool|No|Indicates whether the letters in the generated UUID are in lowercase. Default value: True. This value indicates that the letters are lowercase.|

-   Response

    A UUID is returned.

-   Example
    -   Example 1:

        Raw log entry:

        ```
        content: I am Iran man
        ```

        Transformation rule:

        ```
        e_set("UUID",str_uuid())
        ```

        Result:

        ```
        content: I am Iran man
        UUID: dcc54d7e-c95f-11e9-9791-ffd6754a4f32
        ```

    -   Example 2

        Raw log entry:

        ```
        content: I am Iran man
        ```

        Transformation rule:

        ```
        e_set("UUID",str_uuid(lower=False))
        ```

        Result:

        ```
        content: I am Iran man
        UUID: DCC72D74-C95F-11E9-9921-F395B8647FFA
        ```


