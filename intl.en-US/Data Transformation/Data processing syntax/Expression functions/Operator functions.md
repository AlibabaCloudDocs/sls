# Operator functions

This topic describes the syntax of operator functions and provides parameter descriptions and function examples.

## Functions

|Type|Function|Description|
|----|--------|-----------|
|Conditional functions and logical functions|[op\_if](#section_mig_jki_hlv)|Returns the expression based on a condition that you specify.|
|[op\_ifnull](#section_ckd_wda_tbl) and [op\_coalesce](#section_meh_y8z_mvp)|Returns the value of the first expression whose value is not None.|
|[op\_nullif](#section_4x5_inl_e6f)|Returns the value None if Expression 1 is equal to Expression 2, or returns Expression 1 if Expression 1 is not equal to Expression 2.|
|[op\_and](#section_6dn_e26_731)|Invokes the AND operation.|
|[op\_not](#section_3s2_73o_7q8)|Invokes the NOT operation.|
|[op\_or](#section_43p_je3_xy4)|Invokes the OR operation.|
|Comparison functions|[op\_eq](#section_25s_uy4_zym)|Returns the result calculated based on the `a==b` condition. The data type of a and b must the same. For example, a and b are both strings, numbers, or lists.|
|[op\_ge](#section_2yu_xtl_if2)|Returns the result calculated based on the `a>=b` condition. The data type of a and b must the same. For example, a and b are both strings, numbers, or lists.|
|[op\_gt](#section_8ze_80f_4zk)|Returns the result calculated based on the `a>b` condition. The values of the a and b parameters must be of the same data type. For example, their values are both strings, numbers, or lists.|
|[op\_le](#section_z60_nkx_zqz)|Returns the result calculated based on the `a<=b` condition. The values of the a and b parameters must be of the same data type. For example, their values are both strings, numbers, or lists.|
|[op\_lt](#section_nz6_t7d_c11)|Returns the result calculated based on the `a<b` condition. The data type of a and b must the same. For example, a and b are both strings, numbers, or lists.|
|[op\_ne](#section_azh_rkp_sgy)|Returns the result calculated based on the `a! =b` condition. The data type of a and b must the same. For example, a and b are both strings, numbers, or lists.|
|Container functions|[op\_len](#section_51y_aby_i6k)|Calculates the number of characters in a text string. You can use this function in expressions that return strings, tuples, lists, or dictionaries.|
|[op\_in](#section_upx_pp8_s8r)|Determines whether a string, tuple, list, or dictionary contains a specified element.|
|[op\_not\_in](#section_i5c_dlk_9d9)|Determines whether a string, tuple, list, or dictionary does not contain a specified element.|
|[op\_slice](#section_19o_mxu_cpq)|Truncates a specified string, array, or tuple.|
|[op\_index](#section_rxw_999_uop)|Returns the element that corresponds to the index of the string, array, or tuple.|
|General-purpose multivalued functions|[op\_add](#section_9wc_fea_59b)|Calculates the sum of multiple values, which can be strings or numbers.|
|[op\_max](#section_muw_ccu_4mt)|Determines the largest of the values of multiple fields or expressions.|
|[op\_min](#section_6ft_fco_n1w)|Determines the smallest of the values of multiple fields or expressions.|

## op\_if

-   Syntax

    ```
    op_if(Condition, Value 1, Value 2)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Condition|Arbitrary|Yes|A condition. If the condition is not a Boolean value, the system evaluates whether the condition is true or false.|
    |Value 1|Arbitrary|Yes|Expression 1 that is returned when the specified condition evaluates to True.|
    |Value 2|Arbitrary|Yes|Expression 2 that is returned when the specified condition evaluates to False.|

-   Response

    The expression that corresponds to the evaluation result of the specified condition is returned.

-   Examples
    -   Example 1: If the value of the `content` field is True, the value of Expression 1 is assigned to the `test_if` field.

        Raw log entry:

        ```
        content: hello
        ```

        Transformation rule:

        ```
        e_set("test_if", op_if(v("content"),"still origion content","replace this"))
        ```

        Result:

        ```
        content: hello
        test_if: still origion content
        ```

    -   Example 2: If the value of the `content` field is False, the value of Expression 2 is assigned to the `test_if` field.

        Raw log entry:

        ```
        content: 0
        ```

        Transformation rule:

        ```
        e_set("test_if", op_if(ct_int(v("content", default=0)),"still origion content","replace this"))
        ```

        Result:

        ```
        content: 0
        test_if: replace this
        ```


## op\_ifnull

-   Syntax

    ```
    op_ifnull(Value 1, Value 2)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Value 1|Arbitrary|Yes|The value of an expression.|
    |Value 2|Arbitrary|Yes|The value of an expression.|

-   Response

    The value of the first expression whose value is not None is returned.

-   Examples
    -   Example 1

        Raw log entry:

        ```
        test_if: hello
        escape_name: Etl
        ```

        Transformation rule:

        ```
        e_set("test_ifnull", op_ifnull(v("escape_name"),v("test_if")))
        ```

        Result:

        ```
        test_if: hello
        escape_name: Etl
        test_ifnull: Etl
        ```

    -   Example 2

        Raw log entry:

        ```
        test_if: hello
        escape_name: Etl
        ```

        Transformation rule:

        ```
        e_set("test_ifnull", op_ifnull(v("test_if"),v("escape_name")))
        ```

        Result:

        ```
        test_if: hello
        escape_name: Etl
        test_ifnull: hello
        ```


## op\_coalesce

This function is similar to the `op_ifnull` function. For more information, see [op\_ifnull](#section_ckd_wda_tbl).

## op\_nullif

-   Syntax

    ```
    op_nullif(Value 1, Value 2)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Value 1|Arbitrary|Yes|The value of a field.|
    |Value 2|Arbitrary|Yes|The value of a field.|

-   Response

    If Value 1 is equal to Value 2, the value None is returned. Otherwise, Value 1 is returned.

-   Examples
    -   Example 1

        Raw log entry:

        ```
        content: hello
        escape_name: Etl
        ```

        Transformation rule:

        ```
        e_set("test_ifnull", op_nullif(v("content"),v("escape_name")))
        ```

        Result:

        ```
        content: hello
        escape_name: Etl
        test_nullif: hello
        ```

    -   Example 2

        Raw log entry:

        ```
        content: hello
        escape_name: hello
        ```

        Transformation rule:

        ```
        e_set("test_ifnull", op_nullif(v("content"),v("escape_name")))
        ```

        Result:

        ```
        content: hello
        escape_name: hello
        # In this example, the value of the content field is the same as that of the escape_name field. Therefore, the value None is returned, which means that no content is returned to the test_isnull field.
        ```


## op\_and

-   Syntax

    ```
    op_and(Value 1, Value 2, ...)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Value 1|Arbitrary|Yes|A field value for which the AND operation is invoked.|
    |Value 2|Any type|Yes|A field value for which the AND operation is invoked.|

-   Response
    -   If all specified fields evaluate to true, the value True is returned.
    -   The system evaluates all types of fields to true or false. For more information, see [True or false evaluation](/intl.en-US/Data Transformation/Data processing syntax/Basic syntax.md).
-   Examples
    -   Example 1

        Raw log entry:

        ```
        number1: 123
        number2: 234
        ```

        Transformation rule:

        ```
        e_set("op_and", op_and(v("number1"),v("number2")))
        ```

        Result:

        ```
        number1: 123
        number2: 234
        op_and: True
        ```

    -   Example 2

        Raw log entry:

        ```
        number1: 0
        number2: 234
        ```

        Transformation rule:

        ```
        e_set("op_and", op_and(v("number1"),v("number2")))
        ```

        Result:

        ```
        number1: 0
        number2: 234
        op_and: True
        ```

    -   Example 3

        Raw log entry:

        ```
        ctx1: False
        ctx2: 234
        ```

        Transformation rule:

        ```
        e_set("op_and", op_and(v("ctx1"),v("ctx2")))
        ```

        Result:

        ```
        ctx1: False
        ctx2: 234
        op_and: False
        ```

    -   Example 4

        Raw log entry:

        ```
        ctx1: True
        ctx2: 234
        ```

        Transformation rule:

        ```
        
        e_set("op_and", op_and(v("ctx1"),v("ctx2")))
        ```

        Result:

        ```
        ctx1: True
        ctx2: 234
        op_and: True
        ```


## op\_not

-   Syntax

    ```
    op_not(Value)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Value|Arbitrary|Yes|The field value for which the NOT operation is invoked.|

-   Response
    -   The value True or False is returned.
    -   The system evaluates all types of fields to true or false. For more information, see [True or false evaluation](/intl.en-US/Data Transformation/Data processing syntax/Basic syntax.md).
-   Examples
    -   Example 1

        Raw log entry:

        ```
        ctx1: True
        ```

        Transformation rule:

        ```
        e_set("op_not", op_not(v("ctx1")))
        ```

        Result:

        ```
        ctx1: True
        op_not: False
        ```

    -   Example 2

        Raw log entry:

        ```
        ctx1: 345
        ```

        Transformation rule:

        ```
        e_set("op_not", op_not(v("ctx1")))
        ```

        Result:

        ```
        ctx1: 345
        op_not: False
        ```

    -   Example 3

        Raw log entry:

        ```
        ctx1: 0
        ```

        Transformation rule:

        ```
        e_set("op_not", op_not(v("ctx1")))
        ```

        Result:

        ```
        ctx1: 0
        op_not: True
        ```

    -   Example 4

        Raw log entry:

        ```
        ctx1: ETL
        ```

        Transformation rule:

        ```
        e_set("op_not", op_not(v("ctx1")))
        ```

        Result:

        ```
        ctx1: ETL
        op_not: False
        ```

    -   Example 5

        Raw log entry:

        ```
        ctx1: None
        ```

        Transformation rule:

        ```
        e_set("op_not", op_not(v("ctx1")))
        ```

        Result:

        ```
        ctx1: None
        op_not: True
        ```


## op\_or

-   Syntax

    ```
    op_or(Value 1, Value 2, ... Variable...)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Value 1|Arbitrary|Yes|A field value for which the OR operation is invoked.|
    |Value 2|Arbitrary|Yes|A field value for which the OR operation is invoked.|

-   Response
    -   If any of the specified fields evaluates to true, the value True is returned. Otherwise, the value False is returned.
    -   The system evaluates all types of fields to true or false. For more information, see [True or false evaluation](/intl.en-US/Data Transformation/Data processing syntax/Basic syntax.md).
-   Examples
    -   Example 1

        Raw log entry:

        ```
        ctx1: 123
        ctx2: 234
        ```

        Transformation rule:

        ```
        e_set("op_or", op_or(v("ctx1"),v("ctx2")))
        ```

        Result:

        ```
        ctx1: 123
        ctx2: 234
        op_or: True
        ```

    -   Example 2

        Raw log entry:

        ```
        ctx1: 0
        ctx2: 234
        ```

        Transformation rule:

        ```
        e_set("op_or", op_or(v("ctx1"),v("ctx2")))
        ```

        Result:

        ```
        ctx1: 0
        ctx2: 234
        op_or: True
        ```

    -   Example 3

        Raw log entry:

        ```
        ctx1: ETL
        ctx2: ALIYUN
        ```

        Transformation rule:

        ```
        e_set("op_or", op_or(v("ctx1"),v("ctx2")))
        ```

        Result:

        ```
        ctx1: ETL
        ctx2: ALIYUN
        op_or: True
        ```

    -   Example 4

        Raw log entry:

        ```
        ctx1: True
        ctx2: False
        ```

        Transformation rule:

        ```
        e_set("op_or", op_or(v("ctx1"),v("ctx2")))
        ```

        Result:

        ```
        ctx1: True
        ctx2: False
        op_or: True
        ```

    -   Example 5

        Raw log entry:

        ```
        ctx1: 0
        ctx2: False
        ```

        Transformation rule:

        ```
        e_set("op_or", op_or(v("ctx1"),v("ctx2")))
        ```

        Result:

        ```
        ctx1: 0
        ctx2: False
        op_or: False
        ```

    -   Example 6

        Raw log entry:

        ```
        ctx1: 124
        ctx2: True
        ```

        Transformation rule:

        ```
        e_set("op_or", op_or(v("ctx1"),v("ctx2")))
        ```

        Result:

        ```
        ctx1: 124
        ctx2: True
        op_or: True
        ```


## op\_eq

-   Syntax

    ```
    op_eq(Value 1, Value 2)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Value 1|Arbitrary|Yes|The value of a field.|
    |Value 2|Same as the data type of Value 1|Yes|The value of a field.|

-   Response

    If Value 1 is equal to Value 2, the value True is returned. Otherwise, the value False is returned.

-   Examples
    -   Example 1

        Raw log entry:

        ```
        content: hello
        ctx: hello
        ```

        Transformation rule:

        ```
        e_set("test_eq", op_eq(v("content"),v("ctx")))
        ```

        Result:

        ```
        content: hello
        ctx: hello
        test_eq: True
        ```

    -   Example 2

        Raw log entry:

        ```
        content: hello
        ctx: ctx
        ```

        Transformation rule:

        ```
        e_set("test_eq", op_eq(v("content"),v("ctx")))
        ```

        Result:

        ```
        content: hello
        ctx: ctx
        test_eq: False
        ```


## op\_ge

-   Syntax

    ```
    op_ge(Value 1, Value 2)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Value 1|Arbitrary|Yes|The value of a field.|
    |Value 2|Same as the data type of Value 1|Yes|The value of a field.|

-   Response

    If Value 1 is greater than or equal to Value 2, the value True is returned. Otherwise, the value False is returned.

-   Examples
    -   Example 1: If the value of the apple\_price field is greater than or equal to the value of the orange\_price field, the value True is returned.

        Raw log entry:

        ```
        apple_price: 16
        orange_price: 14
        ```

        Transformation rule:

        ```
        e_set("test_ge", op_ge(ct_int(v("apple_price")),ct_int(v("orange_price"))))
        ```

        Result:

        ```
        apple_price: 16
        orange_price: 14
        test_ge: true
        ```

    -   Example 2: If the value of the apple\_price field is less than the value of the orange\_price field, the value False is returned.

        Raw log entry:

        ```
        apple_price: 12
        orange_price: 14
        ```

        Transformation rule:

        ```
        e_set("test_ge", op_ge(ct_int(v("apple_price")),ct_int(v("orange_price"))))
        ```

        Result:

        ```
        apple_price: 12
        orange_price: 14
        test_ge: false
        ```


## op\_gt

-   Syntax

    ```
    op_gt(Value 1, Value 2)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Value 1|Arbitrary|Yes|The value of a field.|
    |Value 2|Same as the data type of Value 1|Yes|The value of a field.|

-   Response

    If Value 1 is greater than Value 2, the value True is returned. Otherwise, the value False is returned.

-   Examples
    -   Example 1: If the value of the old\_number field is greater than the value of the young\_number field, the value True is returned. Otherwise, the value False is returned.

        Raw log entry:

        ```
        old_number: 16
        young_number: 14
        ```

        Transformation rule:

        ```
        e_set("op_gt",op_gt(ct_int(v("old_number")),ct_int(v("young_number"))))
        ```

        Result:

        ```
        old_number: 16
        young_number: 14
        test_ge: True
        ```

    -   Example 2: If the value of the priority field is greater than the value of the price field, the value True is returned. Otherwise, the value False is returned.

        Raw log entry:

        ```
        priority: 14
        price: 16
        ```

        Transformation rule:

        ```
        e_set("op_gt",op_gt(ct_int(v("priority")),ct_int(v("price"))))
        ```

        Result:

        ```
        priority: 14
        price: 16
        test_ge: False
        ```


## op\_le

-   Syntax

    ```
    op_le(Value 1, Value 2)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Value 1|Arbitrary|Yes|The value of a field.|
    |Value 2|Same as the data type of Value 1|Yes|The value of a field.|

-   Response

    If Value 1 is less than or equal to Value 2, the value True is returned. Otherwise, the value False is returned.

-   Examples
    -   Example 1: If the value of the priority field is less than or equal to the value of the price field, the value True is returned. Otherwise, the value False is returned.

        Raw log entry:

        ```
        priority: 16
        price: 14
        ```

        Transformation rule:

        ```
        e_set("op_le",op_le(ct_int(v("priority")),ct_int(v("price"))))
        ```

        Result:

        ```
        priority: 16
        price: 14
        test_ge: False
        ```

    -   Example 2: If the value of the priority field is less than or equal to the value of the price field, the value True is returned. Otherwise, the value False is returned.

        Raw log entry:

        ```
        priority: 14
        price: 16
        ```

        Transformation rule:

        ```
        e_set("op_le",op_le(ct_int(v("priority")),ct_int(v("price"))))
        ```

        Result:

        ```
        priority: 14
        price: 16
        test_ge: True
        ```


## op\_lt

-   Syntax

    ```
    op_lt(Value 1, Value 2)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Value 1|Arbitrary|Yes|The value of a field.|
    |Value 2|Same as the data type of Value 1|Yes|The value of a field.|

-   Response

    If Value 1 is less than Value 2, the value True is returned. Otherwise, the value False is returned.

-   Examples
    -   Example 1: If the value of the priority field is less than or equal to the value of the price field, the value True is returned. Otherwise, the value False is returned.

        Raw log entry:

        ```
        priority: 16
        price: 14
        ```

        Transformation rule:

        ```
        e_set("op_lt",op_lt(ct_int(v("priority")),ct_int(v("price"))))
        ```

        Result:

        ```
        priority: 16
        price: 14
        op_lt: False
        ```

    -   Example 2: If the value of the priority field is less than or equal to the value of the price field, the value True is returned. Otherwise, the value False is returned.

        Raw log entry:

        ```
        priority: 14
        price: 15
        ```

        Transformation rule:

        ```
        e_set("op_lt",op_lt(ct_int(v("priority")),ct_int(v("price"))))
        ```

        Result:

        ```
        priority: 14
        price: 15
        op_lt: True
        ```


## op\_ne

-   Syntax

    ```
    op_ne(Value 1, Value 2)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Value|Arbitrary|Yes|The value of a field.|
    |Value 2|Same as the data type of Value 1|Yes|The value of a field.|

-   Response

    If Value 1 is not equal to Value 2, the value True is returned. Otherwise, the value False is returned.

-   Examples
    -   Example 1

        Raw log entry:

        ```
        priority: 16
        price: 14
        ```

        Transformation rule:

        ```
        e_set("op_ne",op_ne(ct_int(v("priority")),ct_int(v("price"))))
        ```

        Result:

        ```
        priority: 16
        price: 14
        op_ne: True
        ```

    -   Example 2

        Raw log entry:

        ```
        priority: 14
        price: 14
        ```

        Transformation rule:

        ```
        e_set("op_ne",op_ne(ct_int(v("priority")),ct_int(v("price"))))
        ```

        Result:

        ```
        priority: 14
        price: 14
        op_ne: False
        ```


## op\_len

-   Syntax

    ```
    op_len(Value)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Value|String, tuple, list, or dictionary|Yes|The field value whose length is to be calculated.|

-   Response

    The value length of the specified field is returned.

-   Examples

    Raw log entry:

    ```
    content: I,love,this,world
    ```

    Transformation rule:

    ```
    e_set("op_len",op_len(v("content")))
    ```

    Result:

    ```
    content: I,love,this,world
    op_len: 17
    ```


## op\_in

-   Syntax

    ```
    op_in(a, b)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |a|String, tuple, list, or dictionary|Yes|The name of a container.|
    |b|Arbitrary|Yes|The name of an element.|

    **Note:** The a parameter precedes the b parameter in this function.

-   Response

    If Container A contains Element B, the value True is returned. Otherwise, the value False is returned.

-   Examples

    Raw log entry:

    ```
    list: [1, 3, 2, 7, 4, 6]
    num2: 2
    ```

    Transformation rule:

    ```
    e_set("op_in",op_in(v("list"),v("num2")))
    ```

    Result:

    ```
    list: [1, 3, 2, 7, 4, 6]
    num2: 2
    op_in: True
    ```


## op\_not\_in

-   Syntax

    ```
    op_not_in(Container, Element)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Container|String, tuple, list, or dictionary|Yes|The name of a container. The value of this parameter can be a string, tuple, list, or dictionary.|
    |Element|Arbitrary|Yes|The name of an element.|

    **Note:** The Container parameter precedes the Element parameter in this function.

-   Response

    If the specified container does not contain the specified element, the value True is returned. Otherwise, the value False is returned.

-   Examples

    Raw log entry:

    ```
    list: [1, 3, 2, 7, 4, 6]
    num2: 12
    ```

    Transformation rule:

    ```
    e_set("op_not_in",op_not_in(v("list"),v("num2")))
    ```

    Result:

    ```
    list: [1, 3, 2, 7, 4, 6]
    num2: 12
    op_in: True
    ```


## op\_slice

-   Syntax

    ```
    op_slice(Value, start=None, end=None, step)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Value|String|Yes|The name of the field whose value is to be truncated.|
    |start|Number|No|The position from which the value of the specified field is truncated. Default value: 0.|
    |end|Number|No|The position to which the value of the specified field is truncated. The character in this position is not truncated. The position ends at the end of the specified string by default.|
    |step|Number|No|The length of each truncation.|

-   Response

    The substrings truncated from the value of the specified field are returned.

-   Examples
    -   Example 1: The value of the word field is truncated from the beginning to the end at a step size of 2.

        Raw log entry:

        ```
        word: I,love,this,world
        ```

        Transformation rule:

        ```
        e_set("op_slice",op_slice(v("word"),2))
        ```

        Result:

        ```
        word: I,love,this,world
        op_slice: I,
        ```

    -   Example 2: The value of the word field is truncated from position 2 to position 9 at a step size of 1.

        Raw log entry:

        ```
        word: I,love,this,world
        ```

        Transformation rule:

        ```
        e_set("op_slice",op_slice(v("word"),2,9,1))
        ```

        Result:

        ```
        word: I,love,this,world
        op_slice: love,th
        ```


## op\_index

-   Syntax

    ```
    op_index (Value, index)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Value|String|Yes|The name of the field whose value is to be truncated.|
    |index|Number|No|The index of the specified string, array, or tuple.|

-   Response

    The element that corresponds to the index is returned.

-   Examples
    -   Example 1: The element whose index is 0 in the value of the word field is returned.

        Raw log entry:

        ```
        word: I,love,this,world
        ```

        Transformation rule:

        ```
        e_set("op_index",op_index(v("word"),0))
        ```

        Result:

        ```
        word: I,love,this,world
        op_slice: I,
        ```

    -   Example 2: The element whose index is 3 in the value of the word field is returned.

        Raw log entry:

        ```
        word: I,love,this,world
        ```

        Transformation rule:

        ```
        e_set("op_index",op_index(v("word"),3))
        ```

        Result:

        ```
        word: I,love,this,world
        op_index: o
        ```


## op\_add

-   Syntax

    ```
    op_add(Value 1, Value 2, ...)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Value 1|String, tuple, list, or dictionary|Yes|A field value for which the ADD operation is invoked.|
    |Value 2|Same as the data type of Value 1|Yes|A field value for which the ADD operation is invoked.|

-   Response

    The result of adding the specified values is returned.

-   Examples
    -   Example 1: The values of the price\_orange and price\_apple fields are added to obtain the total price.

        Raw log entry:

        ```
        price_orange: 2
        price_apple: 13
        ```

        Transformation rule:

        ```
        e_set("account",op_add(ct_int(v("price_orange")),ct_int(v("price_apple"))))
        ```

        Result:

        ```
        price_orange: 2
        price_apple: 13
        account: 15
        ```

    -   Example 2: The values of the bytes\_in and bytes\_out fields are added to obtain the total number of bytes.

        Raw log entry:

        ```
        bytes_in: 214
        bytes_out: 123
        ```

        Transformation rule:

        ```
        e_set("total_bytes", op_add(ct_int(v("bytes_in"), ct_int(v("bytes_out")))))
        ```

        Result:

        ```
        bytes_in: 214
        bytes_out: 123
        total_bytes: 337
        ```

    -   Example 3: The string https:// is added to a URL.

        Raw log entry:

        ```
        host: aliyun.com
        ```

        Transformation rule:

        ```
        e_set("website", op_add("https://", v("host")))
        ```

        Result:

        ```
        host: aliyun.com
        website: https://aliyun.com
        ```


## op\_max

-   Syntax

    ```
    op_max(Value 1, Value 2, ...)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Value 1|Arbitrary|Yes|The value of a field.|
    |Value 2|Same as the data type of Value 1|Yes|The value of a field.|

-   Response

    The largest of the specified field values is returned.

-   Examples

    Raw log entry:

    ```
    price_orange: 2
    priority_apple: 13
    ```

    Transformation rule:

    ```
    e_set("max_price", op_max(ct_int(v("price_orange")),ct_int(v("priority_apple"))))
    ```

    Result:

    ```
    price_orange: 2
    priority_apple: 13
    max_price: 13
    ```


## op\_min

-   Syntax

    ```
    op_min(Value 1, Value 2, ...)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Value 1|Arbitrary|Yes|The value of a field.|
    |Value 2|Same as the data type of Value 1|Yes|The value of a field.|

-   Response

    The smallest of the specified field values is returned.

-   Examples

    Raw log entry:

    ```
    price_orange: 2
    price_apple: 13
    ```

    Transformation rule:

    ```
    e_set("op_min", op_min(ct_int(v("price_orange")),ct_int(v("price_apple"))))
    ```

    Result:

    ```
    price_orange: 2
    price_apple: 13
    op_min: 2
    ```


