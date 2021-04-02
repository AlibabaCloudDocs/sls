# JSON functions

This topic describes how to use JSON functions. This topic also provides related examples.

**Note:**

-   If a string fails to be parsed into JSON data, null is returned.
-   In analytic statements of Log Service, a JSON array that is enclosed in single quotation marks \(''\) indicates a string.
-   If the value of a log field is of the JSON type and needs to be expanded to multiple rows, we recommend that you use the UNNEST function. For more information, see [UNNEST function](/intl.en-US/Index and query/Analysis grammar/UNNEST function.md).

## json\_parse\(\) function

The json\_parse\(\) function is used to convert a string to JSON data. The returned result is of the JSON type.

-   Syntax

    ```
    json_parse(string)
    ```

-   Example

    Convert the \[1, 2, 3\] string to the \[1,2,3\] JSON array.

    ```
     * | SELECT json_parse('[1, 2, 3]')
    ```

    The returned result is \[1,2,3\].


## json\_format\(\)

The json\_format\(\) function is used to convert JSON data to a string. The returned result is a string.

-   Syntax

    ```
    json_format(json)
    ```

-   Example

    Convert the \[1,2,3\] JSON array to the \[1, 2, 3\] string.

    ```
    * | SELECT json_format(json_parse('[1, 2, 3]'))
    ```

    The returned result is \[1,2,3\].


## json\_array\_contains\(\)

The json\_array\_contains\(\) function is used to check whether a JSON array or a JSON string contains a specified value. The returned result is true or false.

-   Syntax

    ```
    json_array_contains(json , value)
    ```

-   Example
    -   Check whether the \[1, 2, 3\] JSON array contains 2.

        ```
        * | SELECT json_array_contains(json_parse('[1, 2, 3]'), 2)
        ```

        The returned result is true.

    -   Check whether the \[1, 2, 3\] JSON string contains 2.

        ```
        * | SELECT json_array_contains('[1, 2, 3]', 2)
        ```

        The returned result is true.


## json\_array\_get\(\)

The json\_array\_get\(\) function is used to extract the element that corresponds to the subscript of a JSON array.

-   Syntax

    ```
    json_array_get(json_array, index)
    ```

-   Example

    Extract the element that corresponds to the subscript 0 of the \["status", "request\_time", "request\_method"\] JSON array.

    ```
    * | SELECT json_array_get('["status", "request_time", "request_method"]', 0)
    ```

    The returned result is status.


## json\_array\_length\(\)

The json\_array\_length\(\) function is used to calculate the number of elements in a JSON array.

-   Syntax

    ```
    json_array_length(json array)
    ```

-   Example

    Calculate the number of the elements in the \["status", "request\_time", "request\_method"\] JSON array.

    ```
    * | SELECT json_array_length('["status", "request_time", "request_method"]')
    ```

    The returned result is 3.


## json\_extract\(\)

The json\_extract\(\) function is used to extract the value of a specified field from a JSON object. The returned result is of the JSON type.

**Note:** If the JSON data is invalid when you use the json\_extract\(\) function, an error message appears. We recommend that you use the json\_extract\_scalar\(\) function.

-   Syntax

    ```
    json_extract(json, json_path)
    ```

    The format of json\_path is `$.store.book[0].title`.

-   Example
    -   Extract the value of the status field from the content field. The content field is a JSON object.

        ```
        * | SELECT json_extract(content, '$.status')
        ```

        The returned result is the value of the status field, for example, **"200"**.

    -   Expand the value of the request\_time field and use row to represent the expanded rows. The value of the request\_time field is a JSON array. Then, extract and sum up the values of the status field from the rows.

        ```
        * | select sum(cast (json_extract_scalar(row, '$.status') as bigint) ) from  log, unnest(cast(json_parse(request_time)  as array(json) ) as  t(row)
        ```

        The returned result is the sum result.


## json\_extract\_scalar\(\)

The json\_extract\_scalar\(\) function is used to extract the value of a specified field from a JSON object. The returned result is a string.

-   Syntax

    ```
    json_extract_scalar(json, json_path)
    ```

    The format of json\_path is `$.store.book[0].title`.

-   Example
    -   Extract the value of the status field from the content field. The content field is a JSON object.

        ```
        * | SELECT json_extract_scalar(content, '$.status')
        ```

        The returned result is the value of the status field, for example, **"200"**.

    -   Extract the value of the status field from the content field. The content field is a JSON object. Then, convert the value to the bigint type for summation.

        ```
        * | select sum( cast (json_extract_scalar(content, '$.status') as bigint) )
        ```

        The returned result is the sum result.


## json\_size\(\)

The json\_size\(\) function is used to calculate the number of elements in a JSON object or JSON array.

-   Syntax

    ```
    json_size(json,json_path)
    ```

-   Example

    Calculate the number of elements in the status field.

    ```
    * | SELECT json_size('{"status":[1, 2, 3]}','$.status') 
    ```

    The returned result is 3.


