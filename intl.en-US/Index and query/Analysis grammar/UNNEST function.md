# UNNEST function

## Scenarios

Columns of log data are usually of a primitive data type, such as string or number. In certain scenarios with more complex data structures, the columns of log data may involve complex data types, such as arrays, maps, and JSON objects. The UNNEST function can be used to enumerate an array of complex data into rows for easier querying and analysis.

Example:

```
__source__:  1.1.1.1
__tag__:__hostname__:  vm-req-170103232316569850-tianchi111932.tc
__topic__:  TestTopic_4
array_column:  [1,2,3]
double_column:  1.23
map_column:  {"a":1,"b":2}
text_column:  Product
```

The `array_column` field is of array type. To obtain an aggregate of all values in an `array_column` array, you must enumerate all elements of the array for each row.

## UNNEST function

|Syntax|Description|
|:-----|:----------|
|`unnest( array) as table_alias(column_name)`|Splits the specified array into multiple rows. Each row has a name specified in the `column_name` column.|
|`unnest(map) as table(key_name, value_name)`|Splits the specified map into multiple rows. Each key has a key name specified in the `key_name` column, and a value specified in the `value_name` column.|

**Note:** The UNNEST function takes only arrays or maps. If you specify a string, the string must represent a JSON object. Then you can parse the string into an array or map by using the `cast(json_parse(array_column) as array(bigint))` syntax.

## Enumerate the elements of an array

You can use the following SQL statement to split an array into multiple rows:

```
* | select  array_column, a   from log, unnest( cast( json_parse(array_column)   as array(bigint) ) ) as  t(a)
```

In the preceding statement, the UNNEST function `unnest( cast( json_parse(array_column) as array(bigint) ) ) as t(a)` splits the array into multiple rows and stores the rows in a new table referenced as t, with each column referenced as a.

-   Calculate the sum of the elements in the array:

    ```
    * | select   sum(a)    from log, unnest( cast( json_parse(array_column)   as array(bigint) ) ) as  t(a)
    ```

-   Perform a GROUP BY operation on the array by a \(each element of the array\):

    ```
    * | select   a, count(1)    from log, unnest( cast( json_parse(array_column)   as array(bigint) ) ) as  t(a)     group by a
    ```


## Enumerate the elements of a map

-   Enumerate the elements of a map:

    ```
    * | select  map_column , a,b    from log, unnest( cast( json_parse(map_column)   as map(varchar, bigint) ) ) as  t(a,b)
    ```

-   Perform a GROUP BY operation on the map by key:

    ```
    * | select   key,  sum(value)    from log, unnest( cast( json_parse(map_column)   as map(varchar, bigint) ) ) as  t(key,value)    GROUP  BY  key
    ```


## Visualize histogram, numeric\_histogram query results

-   histogram

    The histogram function is similar to the COUNT GROUP BY syntax. For more information about the syntax, see [Map functions](/intl.en-US/Index and query/Analysis grammar/Map functions.md).

    In general, the histogram function returns a JSON string that cannot be directly visualized. Example:

    ```
    * | select histogram(method)
    ```

    You can use the UNNEST function to split JSON data into multiple rows. Example:

    ```
    * | select  key , value  from( select histogram(method) as his from log) , unnest(his ) as t(key,value)
    ```

-   Numeric\_histogram

    The numeric\_histogram syntax sorts values in a numeric column into multiple bins. This operation is similar to performing a GROUP BY operation on the numeric column. For more information about the syntax, see [Approximate functions](/intl.en-US/Index and query/Analysis grammar/Approximate functions.md).

    ```
    * | select numeric_histogram(10,Latency)
    ```


