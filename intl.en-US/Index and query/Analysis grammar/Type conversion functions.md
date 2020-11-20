# Type conversion functions

You can use type conversion functions to convert data types in a query statement.

## Syntax

**Note:** We recommend that you use the try\_cast\(\) function during a log query if the logs may contain dirty data. Otherwise, the query may fail.

-   If you use the cast\(\) function to convert data types and a value fails to be converted, the entire query is ended. The following examples show the syntax of the cast\(\) function:

    ```
    cast(key AS type)
    ```

    ```
    cast(value AS type)
    ```

-   If you use the try\_cast\(\) function to convert data types and a value fails to be converted, NULL is returned and the query continues. The following examples show the syntax of the try\_cast\(\) function:

    ```
    try_cast(key AS type)
    ```

    ```
    try_cast(value AS type)
    ```


|Parameter|Description|
|:--------|:----------|
|key|The name of a log field. If you set this parameter, all values of this parameter are converted to a specified type.|
|value|The value of a log field. If you set this parameter, the value of this parameter is converted to a specified type.|
|type|SQL data type. Valid values: bigint, varchar, double, timestamp, decimal, array, and map.For information about the mappings between index data types and SQL data types, see [Data type mappings](#section_8kr_3uo_o0b). |

## Examples

-   To convert the number 123 to a string in the varchar format, run the following query statement:

    ```
    cast(123 AS varchar)
    ```

-   To convert the values of the uid field to the varchar format, run the following query statement:

    ```
    cast(uid AS varchar)
    ```


## Data type mappings

The following table lists the mappings between index data types and SQL data types.

|Index data type|SQL data type|
|---------------|-------------|
|long|bigint|
|text|varchar|
|double|double|
|json|varchar|

