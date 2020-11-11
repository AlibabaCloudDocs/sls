# Query string syntax

Query strings are used in the domain-specific languages \(DSL\) for Log Service to filter log data and simplify data transformation. This topic describes the query string syntax used for different functions.

## Functions

The following table describes functions that use query strings.

|Type|Function|Scenario|
|----|--------|--------|
|Event check functions|[e\_search](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Event check functions.md)|Query strings are used to check whether the value of a field in an event meets specific conditions.|
|Resource functions|[res\_log\_logstore\_pull](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.md)|Query strings are used to configure a blacklist or whitelist to filter table data retrieved from a Logstore.|
|[res\_rds\_mysql](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.md)|Query strings are used to configure a blacklist or whitelist to filter table data retrieved from an ApsaraDB RDS for MySQL database.|
|Event mapping functions|[e\_search\_table\_map](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md) and [e\_search\_dict\_map](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md)|Query strings are used to match key-value pairs in a dictionary.|

## Features

The following table lists the search features that are available for field search and full-text search.

|Feature|Field search|Full-text search|
|-------|------------|----------------|
|Substring search|Supported|Supported|
|Search for strings by using wildcard characters `*?`|Supported|Supported|
|Exact match|Supported|Unsupported|
|Search for strings by using regular expressions|Supported|Unsupported|
|Numeric range comparison|Supported|Unsupported|
|Numeric value comparison|Supported|Unsupported|
|Logical operators such as AND, OR, NOT, and their combinations|Supported|Supported|

## Escape special characters

Some special characters such as asterisks \(\*\) and backslashes \(\\\) must be escaped in query strings.

-   Escape special characters in a field name

    Field names cannot be enclosed in double quotation marks \("\). Special characters in a field name must be escaped by using backslashes \(\\\). Examples:

    -   `\*\(1+1\)\?: abc`: Special characters are escaped by using backslashes \(\\\).
    -   `__tag__\:__container_name__: abc`: Special characters are escaped by using backslashes \(\\\).
    -       -   `"content": abc`: The format of the field name in this example is invalid. The field name cannot be enclosed in double quotation marks \("\).
-   Escape special characters in a field value
    -   To query a field value that contains quotation marks \("\) and backslashes \(\\\), you must escape these characters by using backslashes \(\\\), for example, `content:%abc\%xy\\z"`.

        **Note:** To enclose a field value, you must use double quotation marks \("\). You can use single quotation marks \('\) to enclose the whole string and double quotation marks \("\) to enclose the substring, for example, `e_search('field1:"string" ')`.

    -   To query a field value that contains asterisks \(\*\) and question marks \(?\), you must escape these characters by using backslashes \(\\\). Otherwise, these characters are considered wildcard characters.
    -   To query a field value that contains only letters, digits, underscores \(\_\), hyphens \(-\), asterisks \(\*\), and question marks \(?\), you do not need to enclose the value in double quotation marks \("\). In other cases, you must enclose the field value in double quotation marks \("\). Examples:
        -   `status: "\*\?()[]:="`: The field value is enclosed in double quotation marks \("\). The asterisk \(\*\) and question mark \(?\) are escaped by using the backslash \(\\\).
        -   `content: ()[]:=`: The format of the field value is invalid. The value must be enclosed in double quotation marks \("\).
        -   `status: active\*test` and `status: active\? test`: The field values contain only asterisks \(\*\), question marks \(?\), and letters. Therefore, the asterisk \(\*\) and question mark \(?\) are escaped. The field values are not enclosed in double quotation marks \("\).

## Substring search

-   Full-text search

    Searches for substrings in all fields.

    -   Syntax

        ```
        e_search('substring')
        ```

    -   Examples
        -   `e_search('"error"')`: searches for a substring.
        -   `e_search('"active error"')`: searches for a substring that contains a space character.
        -   `e_search('active error')`: searches for multiple substrings that are associated with each other by using the logical operator OR.
-   Field search

    Search for substrings in specific fields.

    -   Syntax

        ```
        e_search('...')
        ```

    -   Examples

        -   `e_search('status: active')`: searches for a substring.
        -   `e_search('author: "john smith"')`: searches for a substring that contains a space character.
        **Note:** `e_search('field: active error')`: searches for the substring named "active" in the field field or the substring named "error" in all fields. The query string in this example is equivalent to `field:active OR "error"`.


## Search with wildcard characters

Each asterisk \(\*\) is used to match zero or more characters. Each question mark \(?\) is used to match one character.

-   Full-text search

    Search for substrings in all fields.

    -   Syntax

        ```
        e_search('substring')
        ```

    -   Examples
        -   `e_search('active*test')`: The asterisk \(\*\) is used to match zero or more characters. The query string does not need to be enclosed in double quotation marks \("\) because it contains only letters and an asterisk \(\*\).
        -   `e_search('error*occurs')`: The asterisk \(\*\) is used to match zero or more characters. In this case, `error occurs` and `error in network connection occurs` are matched.
        -   `e_search('active?good')`: The question mark \(?\) is used to match one character. The query string does not need to be enclosed in double quotation marks \("\) because it contains only letters and a question mark \(?\).
        -   `e_search('ac*tive?good')`: The query string is used to perform an exact match by using an asterisk \(\*\) and a question mark \(?\).
        -   `e_search('ac*tive??go*od')`: The query string is used to perform an exact match by using multiple asterisks \(\*\) and question marks \(?\).
-   Field search

    Search for substrings in specific fields.

    -   Syntax

        ```
        e_search('field name: substring')
        ```

    -   Examples
        -   `e_search('status: active*test')`: The asterisk \(\*\) is used to match zero or more characters.
        -   `e_search('status: active?good')`: The question mark \(?\) is used to match one character.

## Exact match

Exact match requires that the entire field value be matched.

-   Syntax

    ```
    e_search (r'field name==string that must be exactly matched')
    ```

-   Examples
    -   `e_search('author== "john smith"')`: The value of the author field must be john smith.
    -   `e_search('status== ac*tive?good')`: The query string contains wildcard characters and is used for an exact match.

## Search for strings by using regular expressions

Regular expressions can better match the occurrences of strings than wildcard characters.

-   Syntax

    ```
    e_search('field name~="regular expression"')
    ```

    **Note:**

    -   We recommend that you use `r` instead of backslashes \(\\\) to escape special characters in regular expressions because backslashes are used in regular expressions.
    -   By default, a regular expression is used to match the occurrences of strings. For an exact match, you must start the regular expression with `^` and end the expression with `$`.
-   Examples
    -   `e_search('status~= "\d+"')`: The value of the status field contains numbers.
    -   `e_search('status~= "^\d+$"')`: The value of the status field is a numeric value.

## Field value comparison

You can search for field values by comparing field values with specified numeric values.

-   Numeric value comparison

    You can use `>`, `>=`, `=`, `<`, and `<=` to compare field values with specified numeric values.

    ```
    e_search('age >= 18')  #  >=18
    e_search('age > 18')   #  > 18
    e_search('age = 18')   #  = 18
    e_search('age <= 18')  #  <=18
    e_search('age < 18')   #  < 18
    ```

-   Numeric range comparison

    You can also search for field values that are within a closed interval. You can use asterisks \(\*\) to specify a range that has no lower limit or upper limit.

    ```
    e_search('count: [100, 200]') # >=100 and  <=200
    e_search('count: [*, 200]')   # <=200
    e_search('count: [200, *]')   # >=200
    ```


## Logical relationships

Logical operators can be used among multiple search conditions. Parentheses `()` are used to nest search conditions.

|Logical relationships|Keywords|
|---------------------|--------|
|AND|`and`, `AND`, and `&&`. The keywords are case-insensitive.|
|OR|`or` and `OR`. The keywords are case-insensitive.|
|NOT|`not`, `NOT`, and `!`. The keywords are case-insensitive.|

Examples:

```
e_search('abc OR xyz')    # The logical operator is case-insensitive.
e_search('abc and (xyz or zzz)')
e_search('abc and not (xyz and not zzz)')
e_search('abc && xyz')    # and
e_search('abc || xyz')    # or
e_search('abc || ! xyz')   # or not
```

Local operators can also be used in substrings.

```
e_search('field: (abc OR xyz)')      # The field value contains abc or xyz.
e_search('field: (abc OR not xyz)')  # The field value contains abc or does not contain xyz.
e_search('field: (abc && ! xyz)')     # The field value contains abc and does not contain xyz.
```

## Field check

You can use query strings to check fields.

-   `e_search('field: *')`: checks whether a field exists.
-   `e_search('not field:*')`: checks whether a field does not exist.
-   `e_search('not field:""')`: checks whether a field does not exist.
-   `e_search('field: "?"')`: checks whether a field exists and its value is not empty.
-   `e_search('field==""')`: checks whether a field exists and its value is empty.
-   `e_search('field~=". +"')`: checks whether a field exists and its value is not empty.
-   `e_search('not field~=". +"')`: checks whether a field does not exist or its value is empty.
-   `e_search('not field==""')`: checks whether a field does not exist or its value is not empty.

