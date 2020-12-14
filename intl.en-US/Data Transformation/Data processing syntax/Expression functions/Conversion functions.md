# Conversion functions

This topic describes the syntax of operator functions and provides parameter descriptions and function examples.

## Functions

|Type|Function|Description|
|----|--------|-----------|
|Basic type conversion|[ct\_int](#section_wkt_n3o_jde)|Converts the value of a field or expression into an integer.|
|[ct\_float](#section_lne_g2c_8q5)|Converts the value of a field or expression into a floating-point number.|
|[ct\_str](#section_68a_c09_ugc)|Converts the value of a field or expression into a string.|
|[ct\_bool](#section_cs4_pw3_tgn)|Converts the value of a field or expression into a Boolean value.|
|Number conversion|[ct\_chr](#section_v89_ivt_7kv)|Converts the ANSI or Unicode value of a field or expression into a character.|
|[ct\_ord](#section_xv3_c4r_xl5)|Converts the value of a field or expression into an ANSI or Unicode value.|
|[ct\_hex](#section_n3n_pw1_5vx)|Converts the value of a field or expression into a hexadecimal number.|
|[ct\_oct](#section_fz6_rx6_z83)|Converts the value of a field or expression into an octal number.|
|[ct\_bin](#section_nm4_1ng_gj3)|Converts the value of a field or expression into a binary number.|
|Numeral system conversion|[bin2oct](#section_bga_bww_o0v)|Converts a binary string into an octal string.|
|[bin2hex](#section_bgr_fnl_yr5)|Converts a number or string into a hexadecimal string.|

## ct\_int

This function allows you to convert the value of a field or expression into an integer.

-   Syntax

    ```
    ct_int(value, base=10)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Number or numeric string|Yes|The value to be converted.|
    |base|Number|No|The numeral system. Valid value: 10. If you set base=8, this function converts an octal number into a decimal number.|

-   Response

    This function returns an octal number.

-   Examples
    -   Example 1: Convert a string into an integer.
        -   Raw log

            ```
            number: 2
            ```

        -   Transformation rule

            ```
            e_set("int_number", ct_int(v("number")))
            ```

        -   Result

            ```
            number: 2
            int_number:  2
            ```

    -   Example 2: Convert a hexadecimal value into a decimal value.
        -   Raw log

            ```
            number: AB
            ```

        -   Transformation rule

            ```
            e_set("int_number", ct_int(v("number"),base=16))
            ```

        -   Result:

            ```
            number: AB
            int_number:  171
            ```


## ct\_float

This function allows you to convert the value of a field or expression into a floating-point number.

-   Syntax

    ```
    ct_float(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Number or numeric string|Yes|The value to be converted.|

-   Response

    This function returns a floating-point number.

-   Examples
    -   Raw log

        ```
        price: 2
        ```

    -   Transformation rule

        ```
        e_set("price_float", ct_float(v("price")))
        ```

    -   Result

        ```
        price: 2
        price_float:  2.0
        ```


## ct\_str

This function allows you to convert the value of a field or expression into a string.

-   Syntax

    ```
    ct_str(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary|Yes|The value to be converted.|

-   Response

    This function returns a string.

-   Examples
    -   Transformation rule

        ```
        e_set("ct_str", ct_str(b'test byte'))
        ```

    -   Result

        ```
        ct_str: test byte
        ```


## ct\_bool

This function allows you to convert the value of a field or expression into a Boolean value. For more information about how this function returns a Boolean value based on the type of the specified value, see [True or false evaluation](/intl.en-US/Data Transformation/Data processing syntax/Basic syntax.md).

-   Syntax

    ```
    ct_bool(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary|Yes|The value to be converted.|

-   Response

    This function returns a Boolean value.

-   Examples
    -   Raw log

        ```
        num: 2
        ```

    -   Transformation rule

        ```
        e_set("ct_bool", ct_bool(v("number")))
        ```

    -   Result

        ```
        num: 2
        ct_bool: true
        ```


## ct\_chr

This function allows you to convert the ANSI or Unicode value of a field or expression into a character.

-   Syntax

    ```
    ct_chr(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Number or numeric string|Yes|The value to be converted.|

-   Response

    This function returns a character.

-   Examples
    -   Raw log

        ```
        number: 78
        ```

    -   Transformation rule

        ```
        e_set("ct_chr", ct_chr(v("number")))
        ```

    -   Result

        ```
        number: 78
        ct_chr: N
        ```


## ct\_ord

This function allows you to convert the character of a field or expression into an ANSI or Unicode value.

-   Syntax

    ```
    ct_ord(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|String|Yes|The value to be converted. The value includes only one character.|

-   Response

    This function returns an ANSI or Unicode value.

-   Examples
    -   Raw log

        ```
        world: a
        ```

    -   Transformation rule

        ```
        e_set("ct_ord", ct_ord(v("world")))
        ```

    -   Result

        ```
        world: a
        ct_ord: 97
        ```


## ct\_hex

This function allows you to convert the value of a field or expression into a hexadecimal number.

-   Syntax

    ```
    ct_hex(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Number or numeric string.|Yes|The value to be converted.|

-   Response

    This function returns a hexadecimal number.

-   Examples
    -   Raw log

        ```
        number: 123
        ```

    -   Transformation rule

        ```
        e_set("ct_hex", ct_hex(v("number")))
        ```

    -   Result

        ```
        number: 123
        ct_hex: 0x7b
        ```


## ct\_oct

This function allows you to convert the value of a field or expression into an octal number.

-   Syntax

    ```
    ct_oct(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Number or numeric string|Yes|The value to be converted.|

-   Response

    This function returns an octal number.

-   Examples
    -   Raw log

        ```
        number: 123
        ```

    -   Transformation rule

        ```
        e_set("ct_oct", ct_oct(v("number")))
        ```

    -   Result

        ```
        number: 123
        ct_oct: 0o173
        ```


## ct\_bin

This function allows you to convert the value of a field or expression into a binary number.

-   Syntax

    ```
    ct_bin(value)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Number or numeric string|Yes|The value to be converted.|

-   Response

    This function returns a binary number.

-   Examples
    -   Raw log

        ```
        number: 123
        ```

    -   Transformation rule

        ```
        e_set("ct_bin", ct_bin(v("number")))
        ```

    -   Result

        ```
        number: 123
        ct_bin: 0b1111011
        ```


## bin2oct

This function allows you to convert a binary string into an octal string.

-   Syntax

    ```
    bin2oct(binary)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |binary|Binary|Yes|The binary string to be converted.|

-   Response

    This function returns an octal string.

-   Examples
    -   Raw log

        ```
        test : test
        ```

    -   Transformation rule

        ```
        e_set("new",bin2oct(base64_decoding("ARi8WnFiLAAACHcAGgkADV37Xs8BXftezgAdqwF9")))
        ```

    -   Result

        ```
        test : test
        new : 0118bc5a71622c00000877001a09000d5dfb5ecf015dfb5ece001dab017d
        ```


## bin2hex

This function allows you to convert a binary string into a hexadecimal string.

-   Syntax

    ```
    bin2hex(binary)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |binary|Binary|Yes|The binary string to be converted.|

-   Response

    This function returns a hexadecimal string.

-   Examples
    -   Raw log

        ```
        test : test
        ```

    -   Transformation rule

        ```
        e_set("new",bin2hex(base64_decoding("ARi8WnFiLAAACHcAGgkADV37Xs8BXftezgAdqwF9")))
        ```

    -   Result

        ```
        test : test
        new :0118bc5a71622c00000877001a09000d5dfb5ecf015dfb5ece001dab017d
        ```


