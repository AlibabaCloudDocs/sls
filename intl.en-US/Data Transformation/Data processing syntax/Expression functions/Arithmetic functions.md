# Arithmetic functions

This topic describes the syntax of arithmetic functions and provides parameter descriptions and function examples.

**Note:** If the value is negative, use the op\_neg \(positive\) function. For example, if you want to represent `-1`, use op\_neg\(1\).

## Functions

|Type|Function|Description|
|----|--------|-----------|
|Sum calculation|[op\_sum](#section_2we_gk6_d76)|Returns the sum of passed values.|
|Basic calculation|[op\_abs](#section_vxg_j99_2gg)|Returns the absolute value of a passed value.|
|[op\_div\_floor](#section_4e2_wrl_f3b)|Returns the integer part of the quotient after a division of two passed values.|
|[op\_div\_true](#section_oqj_uz7_tc5)|Returns the quotient of two passed values.|
|[op\_pow](#section_cp2_qb6_138)|Returns a value raised to a specified power.|
|[op\_mul](#section_6ac_bnj_f0p)|Returns the product of two passed values.|
|[op\_neg](#section_kmg_mv5_oop)|Returns the opposite number of a passed value.|
|[op\_mod](#section_ks3_6sq_2kr)|Returns the remainder of a passed value divided by the other passed value.|
|[op\_sub](#section_plh_nhh_lnm)|Returns the difference of two passed values.|
|[op\_round](#section_spv_u4r_l7a)|Returns the result of rounding a passed value to a specified number of decimal places.|
|Mathematical calculation|[mat\_ceil](#section_gcc_ma8_z9u)|Returns a passed value rounded up to the nearest integer.|
|[mat\_exp](#section_yen_n86_pzo)|Returns Euler's number raised to the power of a passed value.|
|[mat\_fabs](#section_nj8_vlh_6cl)|Returns the absolute value of a passed value.|
|[mat\_floor](#section_k76_s8z_d0v)|Returns a passed value rounded down to the nearest integer.|
|[mat\_log](#section_tkg_p2t_59c)|Returns the logarithm of a passed value with the other passed value as the base.|
|[mat\_log10](#section_gsa_5pi_kbo)|Returns the base-10 logarithm of a passed value.|
|[mat\_sqrt](#section_kzw_wbw_b8y)|Returns the square root of a passed value.|
|[mat\_degrees](#section_3sz_ruq_ijn)|Converts a radian value to a degree.|
|[mat\_radians](#section_cq7_pgr_ffo)|Converts a degree to a radian value.|
|[mat\_sin](#section_pbh_dyc_6h0)|Returns the sine \(in radians\) of a passed value.|
|[mat\_cos](#section_sci_2uz_5wz)|Returns the cosine \(in radians\) of a passed value.|
|[mat\_tan](#section_9t5_m2i_l0h)|Returns the tangent \(in radians\) of a passed value.|
|[mat\_acos](#section_z6m_fxs_aca)|Returns the arc cosine \(in radians\) of a passed value.|
|[mat\_asin](#section_704_inv_5ud)|Returns the arc sine \(in radians\) of a passed value.|
|[mat\_atan](#section_dkb_vx2_0ql)|Returns the arc tangent \(in radians\) of a passed value.|
|[mat\_atan2](#section_zyu_xy7_7s9)|You can call this function to calculate the arc tangent of X- and Y-coordinates.|
|[mat\_atanh](#section_4cp_rgb_btw)|Returns the inverse hyperbolic tangent of a passed value.|
|[mat\_hypot](#section_9fy_wcn_zgy)|Returns the Euclidean norm of passed values.|
|MATH\_PI|The constant pi.|
|MATCH\_E|The constant e.|

## op\_sum

-   Syntax

    ```
    op_sum(Value 1, Value 2, ...)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value 1|Number or numeric string|Yes|The value used for the calculation.|
    |Value 2|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The sum of all passed values is returned.

-   Example: Calculate the sum of the values for the course\_price and goods\_price fields.

    Raw log entry:

    ```
    course_price: 12
    goods_price: 2
    ```

    Transformation rule:

    ```
    e_set("account", op_sum(v("course_price"),v("goods_price")))
    ```

    Result:

    ```
    course_price: 12
    goods_price: 2
    account: 14
    ```


## op\_abs

-   Syntax

    ```
    op_abs(Value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The absolute value of the passed value is returned.

-   Example: Calculate the absolute value of the value for the course\_price field.

    Raw log entry:

    ```
    course_price: -4
    ```

    Transformation rule:

    ```
    e_set("op_abs", op_abs(v("course_price")))
    ```

    Result:

    ```
    course_price: -4
    op_abs: 4
    ```


## op\_div\_floor

-   Syntax

    ```
    op_div_floor(Value 1, Value 2)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value 1|Number or numeric string|Yes|The value used for the calculation.|
    |Value 2|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The integer part of the quotient after Value 1 is divided by Value 2 is returned.

-   Example: Calculate the value of the course\_price field.

    Raw log entry:

    ```
    course_price: 4
    count: 2
    ```

    Transformation rule:

    ```
    e_set("op_div_floor", op_div_floor(v("course_price"),v("count")))
    ```

    Result:

    ```
    course_price: 4
    count: 2
    op_div_floor: 2
    ```


## op\_div\_true

Returns the quotient of two passed values.

**Note:** This function can be used to convert data types, such as String and Integer.

-   Syntax

    ```
    op_div_true(Value 1, Value 2)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value 1|Number or numeric string|Yes|The value used for the calculation.|
    |Value 2|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The quotient after Value 1 is divided by Value 2.

-   Examples
    -   Example 1: Calculate the value of the fruit\_price field.

        ```
        e_set("op_div_true", op_div_true(v("fruit_price"),v("count")))
        ```

        Raw log entry:

        ```
        fruit_price: 9
        count: 2
        ```

        Result:

        ```
        fruit_price: 9
        count: 2
        op_div_true: 4.5
        ```

    -   Example 2: Calculate the values of the one\_speed and two\_speed fields. The value is rounded. In this example, a is calculated by using the following formula: a = \(one\_speed-two\_speed\)/time.

        ```
        e_set("a", op_round(op_div_true(op_sub(v("one_speed"),v("two_speed")),v("time"))),2)
        ```

        Raw log entry:

        ```
        one_speed: 9
        two_speed: 2
        time: 3
        ```

        Result:

        ```
        one_speed: 9
        two_speed: 2
        time: 3
        a: 2.33
        ```


## op\_pow

-   Syntax

    ```
    op_pow(Value 1,Value 2)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value 1|Number or numeric string|Yes|The value used for the calculation.|
    |Value 2|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    Value 1 raised to the power of Value 2 is returned.

-   Example: Calculate the value of the course field raised to the power of the pow field.

    Raw log entry:

    ```
    course: 100
    pow: 2
    ```

    Transformation rule:

    ```
    e_set("pow_course", op_pow(v("course"),v("pow")))
    ```

    Result:

    ```
    course: 100
    pow: 2
    pow_course: 10000
    ```


## op\_mul

-   Syntax

    ```
    op_mul(Value 1,Value 2)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value 1|Number, string, tuple, or list|Yes|The value used for the calculation.|
    |Value 2|Number|Yes|The value used for the calculation.|

-   Response

    The result of multiplying Value 1 by Value 2 is returned.

    -   If Value 1 is a number, the product of multiplying Value 1 by Value 2 is returned.
    -   If Value 1 is a string, tuple, or list, the result is the original value repeated for the specified times.
-   Examples
    -   Example 1:

        Raw log entry:

        ```
        course: 10
        price: 23
        ```

        Transformation rule:

        ```
        e_set("account", op_mul(ct_int(v("course")),ct_int(v("price"))))
        ```

        Result:

        ```
        course: 10
        price: 23
        account: 230
        ```

    -   Example 2:

        Raw log entry:

        ```
        course: "abc"
        ```

        Transformation rule:

        ```
        e_set("course", op_mul(v("course"), 3))
        ```

        Result:

        ```
        course: "abcabcabc"
        ```


## op\_neg

-   Syntax

    ```
    op_neg(Value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The opposite number of the passed value is returned.

-   Example

    Raw log entry:

    ```
    course: -100
    ```

    Transformation rule:

    ```
    e_set("account", op_neg(v("course_price")))
    ```

    Result:

    ```
    course: -100
    account: 100
    ```


## op\_mod

-   Syntax

    ```
    op_mod(Value 1,Value 2)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value 1|Number or numeric string|Yes|The value used for the calculation.|
    |Value 2|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The remainder after Value 1 is divided by Value 2 is returned.

-   Example

    Raw log entry:

    ```
    course: 4
    count: 3
    ```

    Transformation rule:

    ```
    e_set("op_mod", op_mod(v("course"),v("count")))
    ```

    Result:

    ```
    course: 4
    count: 3
    op_mod: 1
    ```


## op\_sub

-   Syntax

    ```
    op_sub(Value 1,Value 2)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value 1|Number or numeric string|Yes|The value used for the calculation|
    |Value 2|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The result of subtracting Value 2 from Value 1 is returned.

-   Example: Calculate the result of subtracting the value of the count\_apple field from the value of the count field.

    Raw log entry:

    ```
    count: 6
    count_apple: 3
    ```

    Transformation rule:

    ```
    e_set("sub_number", op_sub(v("count"),v("count_apple")))
    ```

    Result:

    ```
    count: 6
    count_apple: 3
    sub_number:  3
    ```


## op\_round

-   Syntax

    ```
    op_round(Value, Decimal place)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value|Number or numeric string|Yes|The value used for the calculation.|
    |Decimal place|Number|Yes|The number of decimal places to which the value is rounded. Default value: 0.|

-   Response

    The result of rounding the passed value is returned.

-   Example: Round the value of the price field to one decimal place.

    Raw log entry:

    ```
    price: 4.56
    ```

    Transformation rule:

    ```
    e_set("round_price", op_round(v("price"),1))
    ```

    Result:

    ```
    price: 4.56
    round_price: 4.6
    ```


## mat\_ceil

-   Syntax

    ```
    mat_ceil(Value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The smallest integer that is not less than the passed value is returned.

-   Examples

    Raw log entry:

    ```
    price: 4.1
    ```

    Transformation rule:

    ```
    e_set("mat_ceil", mat_ceil(v("price")))
    ```

    Result:

    ```
    price: 4.1
    mat_ceil: 5
    ```


## mat\_exp

-   Description

    You can call this function to calculate the result of raising e to the power of a passed value.

-   Syntax

    ```
    mat_exp(Value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The result of raising e to the power of the passed value is returned.

-   Example

    Raw log entry:

    ```
    e: 1
    ```

    Transformation rule:

    ```
    e_set("e_x", mat_exp(v("e")))
    ```

    Result:

    ```
    e: 1
    e_x: 2.718281828459045
    ```


## mat\_fabs

-   Description

    You can call this function to calculate the absolute value of a passed value.

-   Syntax

    ```
    mat_fabs(Value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The absolute value of the passed value is returned.

-   Example

    Raw log entry:

    ```
    course_price: -10
    ```

    Transformation rule:

    ```
    e_set("mat_fabs", mat_fabs(v("course_price")))
    ```

    Result:

    ```
    course_price: -10
    mat_fabs: 10.0
    ```


## mat\_floor

-   Description

    You can call this function to obtain the largest integer not greater than a passed value.

-   Syntax

    ```
    mat_floor(Value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The largest integer that is not greater than the passed value is returned.

-   Example

    Raw log entry:

    ```
    course_price: 4.9
    ```

    Transformation rule:

    ```
    e_set("mat_floor", mat_floor(v("course_price")))
    ```

    Result:

    ```
    course_price: 4.9
    mat_floor: 4
    ```


## mat\_log

-   Description

    You can call this function to calculate the logarithm of a passed value.

-   Syntax

    ```
    mat_log(Value 1,Value 2)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value 1|Number or numeric string|Yes|The value used for the calculation.|
    |Value 2|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The logarithm of the passed value is returned.

-   Example

    Raw log entry:

    ```
    number1: 100
    number2: 10
    ```

    Transformation rule:

    ```
    e_set("mat_log", mat_log(v("number1"),v("number2")))
    ```

    Result:

    ```
    number1: 100
    number2: 10
    mat_log: 2.0
    ```


## mat\_log10

-   Description

    You can call this function to calculate the base-10 logarithm of a passed value.

-   Syntax

    ```
    mat_log10(Value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The base-10 logarithm of the passed value is returned.

-   Example

    Raw log entry:

    ```
    number: 100
    ```

    Transformation rule:

    ```
    e_set("number2", mat_log10(v("number")))
    ```

    Result:

    ```
    number: 100
    numbe2: 2.0
    ```


## mat\_sqrt

-   Description

    You can call this function to calculate the square root of a passed value.

-   Syntax

    ```
    mat_sqrt(Value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The square root of the passed value is returned.

-   Example

    Raw log entry:

    ```
    number1: 100
    ```

    Transformation rule:

    ```
    e_set("sqrt_account", mat_sqrt(v("number1")))
    ```

    Result:

    ```
    number1: 100
    sqrt_account: 10.0
    ```


## mat\_degrees

-   Description

    You can call this function to convert radians to degrees.

-   Syntax

    ```
    mat_degrees(Value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The degrees are returned.

-   Examples

    Raw log entry:

    ```
    num: 1
    ```

    Transformation rule:

    ```
    e_set("mat_degrees", mat_degrees(v("num")))
    ```

    Result:

    ```
    num: 1
    mat_degrees: 57.29577951308232
    ```


## mat\_radians

-   Description

    You can call this function to convert degrees to radians.

-   Syntax

    ```
    mat_radians(Value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The radians are returned.

-   Example

    Raw log entry:

    ```
    rad: 30
    ```

    Transformation rule:

    ```
    e_set("mat_radians", mat_radians(v("rad")))
    ```

    Result:

    ```
    rad: 30
    mat_radians: 0.5235987755982988
    ```


## mat\_sin

-   Description

    You can call this function to calculate the sine of a passed value \(in radians\).

-   Syntax

    ```
    mat_sin(Value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The sine of the passed value \(in radians\) is returned.

-   Example

    Raw log entry:

    ```
    sin: 90
    ```

    Transformation rule:

    ```
    e_set("mat_sin", mat_sin(v("sin")))
    ```

    Result:

    ```
    sin: 90
    mat_sin: 0.8939966636005579
    ```


## mat\_cos

-   Description

    You can call this function to calculate the cosine of a passed value \(in radians\).

-   Syntax

    ```
    mat_cos(Value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The cosine of the passed value \(in radians\) is returned.

-   Example

    Raw log entry:

    ```
    cos: 30
    ```

    Transformation rule:

    ```
    e_set("mat_cos", mat_cos(v("cos")))
    ```

    Result:

    ```
    cos: 30
    mat_cos: 0.15425144988758405
    ```


## mat\_tan

-   Description

    You can call this function to calculate the tangent of a passed value \(in radians\).

-   Syntax

    ```
    mat_tan(Value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The tangent of the passed value \(in radians\) is returned.

-   Example

    Raw log entry:

    ```
    tan: 30
    ```

    Transformation rule:

    ```
    e_set("mat_tan", mat_tan(v("tan")))
    ```

    Result:

    ```
    tan: 30
    mat_tan: 1.6197751905438615
    ```


## mat\_acos

-   Description

    You can call this function to calculate the arc cosine \(in radians\) of a passed value.

-   Syntax

    ```
    mat_acos(Value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The arc cosine \(in radians\) of the passed value is returned.

-   Example

    Raw log entry:

    ```
    acos: 1
    ```

    Transformation rule:

    ```
    e_set("mat_acos", mat_acos(v("acos")))
    ```

    Result:

    ```
    acos: 1
    mat_acos: 0.0
    ```


## mat\_asin

-   Description

    You can call this function to calculate the arc sine \(in radians\) of a passed value.

-   Syntax

    ```
    mat_asin(Value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The arc sine \(in radians\) of the passed value is returned.

-   Example

    Raw log entry:

    ```
    asin: 1
    ```

    Transformation rule:

    ```
    e_set("mat_asin", mat_asin(v("asin")))
    ```

    Result:

    ```
    asin: 1
    mat_asin: 1.5707963267948966
    ```


## mat\_atan

-   Description

    You can call this function to calculate the arc tangent \(in radians\) of a passed value.

-   Syntax

    ```
    mat_atan(Value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value|Number or numeric string|Yes|The value used for the calculation.|

-   Response

    The arc tangent \(in radians\) of the passed value is returned.

-   Example

    Raw log entry:

    ```
    atan: 1
    ```

    Transformation rule:

    ```
    e_set("mat_atan", mat_atan(v("atan")))
    ```

    Result:

    ```
    atan: 1
    mat_atan: 0.7853981633974483
    ```


## mat\_atan2

-   Description

    You can call this function to calculate the arc tangent of X- and Y-coordinates.

-   Syntax

    ```
    mat_atan2(x,y)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |x|Number or numeric string|Yes|The X-coordinate.|
    |y|Number or numeric string|Yes|The Y-coordinate.|

-   Response

    The arc tangent of X- and Y-coordinates is returned.

-   Example

    Raw log entry:

    ```
    atan1: 1
    atan2: 2
    ```

    Transformation rule:

    ```
    e_set("mat_atan2", mat_atan2(v("atan1"),v("atan2")))
    ```

    Result:

    ```
    atan1: 1
    atan2: 2
    mat_atan2: 0.4636476090008061
    ```


## mat\_atanh

-   Description

    You can call this function to calculate the inverse hyperbolic tangent of a passed value.

-   Syntax

    ```
    mat_atanh(Value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value|Number or numeric string|Yes|The X-coordinate.|

-   Response

    The inverse hyperbolic tangent of the passed value is returned.

-   Example

    Raw log entry:

    ```
    atanh: 1
    ```

    Transformation rule:

    ```
    e_set("mat_atanh", mat_atanh(v("atanh")))
    ```

    Result:

    ```
    atanh: 1
    mat_atanh: 0.7615941559557649
    ```


## mat\_hypot

-   Description

    You can call this function to calculate the Euclidean norm of passed values.

-   Syntax

    ```
    mat_hypot(Value 1,Value 2)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Value 1|Number or numeric string|Yes|The X-coordinate.|
    |Value 2|Number or numeric string|Yes|The Y-coordinate.|

-   Response

    The Euclidean norm of the passed values is returned.

-   Example

    Raw log entry:

    ```
    hypot1: 1
    hypot2: 2
    ```

    Transformation rule:

    ```
    e_set("mat_hypot", mat_hypot(v("hypot1"),v("hypot2")))
    ```

    Result:

    ```
    hypot1: 1
    hypot2: 2
    mat_hypot: 3.605551275463989
    ```


