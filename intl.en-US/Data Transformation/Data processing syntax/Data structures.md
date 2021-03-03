# Data structures

This topic describes the data structures in data transformation syntax.

## Basic data structures

The following table describes the basic data structures.

|Type|Description|
|----|-----------|
|Integer|You can use integers as field values. You can also use integers to pass the values of function parameters.For example, the `e_set("f1", 100)` function is used to set the value of the `f1` field to 100. |
|Float|You can use floats as field values. You can also use floats to pass the values of function parameters.For example, the `e_set("f1", 1.5)` function is used to set the value of the `f1` field to 1.5. |
|String|Strings can be used in multiple formats. For example:-   `"abc"` is equivalent to `'abc'`. If a string contains a double quotation mark \("\), you can write the string in the `'abc"xyz'` format. You can also use a backslash \(\\\) to escape the double quotation mark in the following format: `"abc\"xyz"`.

Backslashes \(\\\) are used to escape special characters. For example, `"\\abc\\xyz"` refers to the `\abc\xyz` string.

-   Both `r"\\10.64.1.1\share\folder"` and `"\\\\10.64.1.1\\share\\folder"` refer to the `\\10.64.1.1\share\folder` string.
-   Multibyte character strings are encoded in Unicode. For example, the length of `a string consisting of two Chinese characters` is 2.
-   Regular expressions are represented as strings.

**Note:** The strings used in the e\_search function must be enclosed by double quotation marks \(""\) instead of single quotation marks \(''\). For example, `e_search("domain: '/url/test.jsp'")` is invalid whereas `e_search('domain: "/url/test.jsp"')` is valid. |
|Bytes|Example: `b'abc'`. Bytes are encoded in memory by using a format that is different from the format of strings. Bytes are received and returned by special functions.|
|None|Both `None` and `null` indicate a NULL value. Some named parameters of functions use `None` as the default value to indicate a specific default behavior. **Note:** None or null does not indicate an empty string. |
|List|An array, for example, \[1, 2, 3, 4\]. -   Some functions receive lists as parameters, for example, `e_dict_map("dict data", ["f1", "f2", "f3"], ...)`.
-   Some functions return lists as a result. For example, if you call the `json_select` function to extract an array, a list is returned. |
|Tuple|Tuples are similar to lists, for example, \(1,2,3,4\).|
|Dictionary|A collection of key-value pairs in the format of `{"key": "value", "k2": "v2", ...}`. A key is usually a unique string and a value can be of any data type. The key-value pairs are stored in a hash table in an unordered manner. -   An event is a special dictionary.
-   Some functions can receive dictionaries in specific formats, for example, `{"key": [1,2, 3], "ke": {"k3": "va3"} }`.
-   The dictionary structure is also used as the input data for mapping fields to a dictionary. |
|Boolean|For example, `True`, `False`, `true`, and `false`.|
|Table|Each table consists of multiple rows and columns. You can load data in the comma-separated values \(CSV\) file that consists of multiple rows from an external resource to construct a table. You can also load multiple columns of data from an ApsaraDB RDS database or a Logstore to construct a table. Tables are used in advanced operations such as data mapping and enrichment.|
|Datetime object|A memory object that indicates date and time. A datetime object can be converted to a UNIX timestamp or a formatted time string. A datetime object can be passed to `dt_` functions for further conversion.|

## Event structure and fields

Events have the following structures and fields:

-   Basic structure

    An event is processed in the dictionary structure during the data transformation process, for example, `{"__topic__": "access_log", "content": "....."}`.

    The keys and values of the dictionary correspond to the fields and values in the log.

    **Note:** The keys and values of an event are strings, and the keys must be unique.

-   Meta-fields

    The following meta-fields are supported:

    -   `__time__`: the timestamp when the log is received by Log Service. The value is an integer string. The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970.
    -   `__topic__`: the topic of the log. Topics are used to group logs in a Logstore. You can specify a topic for logs that are written to Log Service. You can also specify a topic when you query logs.
    -   `__source__`: the source of the log. For example, the value of this field can be the IP address of the server that is used to generate the log.
-   Time field modification

    You can change the value of the \_\_time\_\_ field to modify the event time of a log. You can use date and time functions to perform more operations on the \_\_time\_\_ field.

    **Note:** If the \_\_time\_\_ field is deleted, the system time when the log is processed is used as the event time when the log is written to the destination Logstore.

-   Tags

    Tags are used to differentiate fields in logs. Tags are in the format of `__tag__:name`.

    -   If the source Logstore enables the feature of recording the public IP address, logs contain the `__tag__:__receive_time__` tag.
    -   Container logs contain many container-related tags, such as `__tag__:__container_name__`.
    -   You can add and modify tags. For example, you can add a tag named type by calling the `e_set("__tag__:type", "access_log")` function.
-   Automatic type conversion during assignment

    The keys and values of an event are represented as strings. Therefore, when you assign a value to a field, the value is automatically converted to a string. For example:

    ```
    e_set("v1", 12.3)
    e_set("v2", True)
    ```

    After you call the preceding functions, the `v1` field is set to the string 12.3, and the `v2` field is set to the string true.

    The following table provides conversion examples of different data types.

    |Type|Value|Conversion type|Conversion value|
    |----|-----|---------------|----------------|
    |Integer|`1`|String|`"1"`|
    |Float|`1.2`|String|`"1.2"`|
    |Boolean|`True`|String|`"true"`|
    |Byte|`b"123"`|String encoded in UTF-8|`"123"`|
    |Tuple|    -   Example 1: `(1, 2, 3)`
    -   Example 2: `("a", 1)`
|String that represents a list|    -   Example 1: `"[1, 2, 3]"`
    -   Example 2: `"[\"a\", 1]"` |
    |List|    -   Example 1: `[1,2,3]`
    -   Example 2: `["a", 1]`
|String|    -   Example 1: `"[1, 2, 3]"`
    -   Example 2: `"[\"a\", 1]"` |
    |Dictionary|`{"1":2, "3":4}`|String|`"{\"1\": 2, \"3\": 4}"`|
    |Datetime|`datetime(2018, 10, 10, 10, 10, 10)`|String that represents time in the ISO format|`2018-10-10 10:10:10`|


## Identifiers

The data transformation feature provides identifiers to represent field names or values. You can use the identifiers to simplify your code or make your code easy to understand.

|Identifier|Type|Equivalent|
|----------|----|----------|
|true|Bool|`True`|
|false|Bool|`False`|
|null|None|`None`|
|F\_TAGS|String|Regular expression that represents `tag` fields: `"__tag__:. +"`|
|F\_META|String|Regular expression that represents `tag`, `__topic__`, and `__source__` fields: `__tag__:.+|__topic__|__source__`|
|F\_TIME|String|`__time__```|
|F\_PACK\_META|String|Regular expression that represents `pack meta` fields: `"__pack_meta__|__tag__:__pack_id__"`|
|F\_RECEIVE\_TIME|String|`"__tag__:__receive_time__"```|

## JSON objects

JSON objects are returned by JSON expression functions, such as `json_select` and `json_parse`. The objects are in basic data structures. For more information, see [Basic data structures](#section_u9c_cbl_hjq). The following table describes how strings are converted to JSON objects.

|String|JSON object|Type|
|------|-----------|----|
|`1`|`1`|Integer|
|`1.2`|`1.2`|Float|
|`true`|`True`|Boolean|
|`false`|`False`|Boolean|
|`"abc"`|`"abc"`|String|
|`null`|`None`|None|
|`["v1", "v2", "v3"]`|`["v1", "v2", "v3"]`|List|
|`["v1", 3, 4.0]`|`["v1", 3, 4.0]`|List|
|`{"v1": 100, "v2": "good"}`|`{"v1": 100, "v2": "good"}`|Dictionary|
|`{"v1": {"v11": 100, "v2": 200}, "v3": "good"}`|`{"v1": {"v11": 100, "v2": 200}, "v3": "good"}`|Dictionary|

