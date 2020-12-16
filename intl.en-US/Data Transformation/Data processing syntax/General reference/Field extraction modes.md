# Field extraction modes

This topic describes the parameters of field extraction modes in different functions.

## Related functions

The following table lists the functions that use the mode parameter.

|Category|Function|Default value of mode|
|--------|--------|---------------------|
|Value assignment function|[e\_set](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value assignment function.md)|overwrite|
|Value extraction functions|[e\_regex](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value extraction functions.md)|fill-auto|
|[e\_json](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value extraction functions.md)|fill-auto|
|[e\_kv](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value extraction functions.md)|fill-auto|
|[e\_csv, e\_psv, and e\_tsv](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value extraction functions.md)|fill-auto|
|[e\_kv\_delimit](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value extraction functions.md)|fill-auto|
|[t947544.md\#section\_ud8\_v7y\_3rt](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value extraction functions.md)|overwrite|
|[e\_syslogrfc](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value extraction functions.md)|overwrite|
|Mapping and enrichment functions|[e\_dict\_map](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md)|fill-auto|
|[e\_table\_map](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md)|fill-auto|
|[e\_search\_dict\_map](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md)|overwrite|
|[e\_search\_table\_map](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md)|fill-auto|

## Field check and overwrite modes

The following table describes the values of the mode parameter.

|Value|Description|
|-----|-----------|
|fill|Sets the destination field if the field does not exist or the field value is an empty string.|
|fill-auto|Sets the destination field if the new value is not an empty string and the destination field does not exist or its value is an empty string.|
|add|Sets the destination field if the field does not exist.|
|add-auto|Sets the destination field if the new value is not an empty string and the destination field does not exist.|
|overwrite|Always sets the destination field.|
|overwrite-auto|Sets the destination field if the new value is not an empty string.|

The following examples show how these modes work.

```
Raw log entry:
  a:         # An empty string
  b: 100
```

|Mode|Example|Result|
|----|-------|------|
|add|`e_set("c", "123", mode='add')`|The c field is added as `c: 123` to the raw log entry.|
|`e_set("c", "", mode='add')`|The c field is added as `c:` to the raw log entry.|
|`e_set("a", "123", mode='add')`|The a field remains `a:`.|
|add-auto|`e_set("c", "", mode='add-auto')`|The `c` field is not added to the raw log entry.|
|fill|`e_set("c", "123", mode='fill')`|The c field is added as `c: 123` to the raw log entry.|
|`e_set("c", "", mode='fill')`|The c field is added as `c:` to the raw log entry.|
|`e_set("a", "123", mode='fill')`|The a field is modified to `a: 123`.|
|`e_set("b", "123", mode='fill')`|The b field remains `b: 100`.|
|fill-auto|`e_set("c", "", mode='fill-auto')`|The `c` field is not added to the raw log entry.|
|overwrite|`e_set("c", "123", mode='overwrite')`|The c field is added as `c: 123` to the raw log entry.|
|`e_set("c", "", mode='overwrite')`|The c field is added as `c:` to the raw log entry.|
|`e_set("b", "200", mode='overwrite')`|The b field is modified to `b: 200`.|
|`e_set("b", "", mode='overwrite')`|The b field is modified to `b:`.|
|overwrite-auto|`e_set("b", "", mode='overwrite-auto')`|The b field remains `b: 100`.|

## Constraints on field name extraction

The constraints apply to functions such as e\_json, e\_kv, e\_kv\_delimit, and e\_regex.

Only the fields whose names meet the constraints can be extracted. The fields that do not meet the constraints are discarded. The regular expression `u'_*[\u4e00-\u9fa5\u0800-\u4e00a-zA-Z][\u4e00-\u9fa5\u0800-\u4e00\\w\\.\\-]*'` is not supported. For example, the fields `123=abc __1__:100 1k=200 {"123": "456"}` will be discarded.

The default constraints are used in the following example.

-   Raw log entry:

    ```
    data: {"k1": 100, "k2": {"k3": 200, "k4": {"k5": 300} } }
    ```

-   Transformation rule:

    ```
    e_json("data", fmt='parent', sep="@", prefix="__", suffix="__",include_node=r"[\u4e00-\u9fa5\u0800-\u4e00a-zA-Z][\w\-\.]*", mode='fill-auto' )
    ```

-   Result:

    ```
    data: {"k1": 100, "k2": {"k3": 200, "k4": {"k5": 300} } }
    data@__k1__: 100
    k2@__k3__: 200
    k4@__k5__: 300
    ```


