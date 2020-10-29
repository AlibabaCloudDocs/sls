# Extract dynamic key-value pairs from a string

This topic describes how to extract dynamic key-value pairs from a string by using different functions.

## Functions

Extracting dynamic key-value pairs is a process that extracts and transforms keywords and values. You can use the e\_kv function, e\_kv\_delimit function, and e\_regex function to extract dynamic key-value pairs. The following table describes the functions that you can use in different scenarios.

|Function|Keyword extraction|Value extraction|Keyword transformation|Value transformation|
|--------|------------------|----------------|----------------------|--------------------|
|e\_kv|Uses specific regular expressions.|Supports the default character set and specific delimiters such as \(,\) and \("\).|Supports prefixes and suffixes.|Supports text escape.|
|e\_kv\_delimit|Uses specific regular expressions.|Uses delimiters.|Supports prefixes and suffixes.|None.|
|e\_regex|Uses custom regular expressions and the default character set.|Custom.|Custom.|Custom.|

In most cases, you can use the `e_kv` function to extract key-value pairs, especially when you need to extract and escape enclosed characters or backslashes \(\\\). In complicated scenarios, you can use the `e_regex` function to extract key-value pairs. In specific scenarios, you need to extract key-value pairs by using the `e_kv_delemit` function.

## Extract keywords

-   Method

    When you use the `e_kv` function, `e_kv_delimit` function, or `e_regex` function to extract keywords, the functions must comply with the extraction constraints. For more information, see [Constraints on field name extraction](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).

-   Example 1

    The following example describes three methods that you can use to extract keywords and values from the `k1: q=asd&a=1&b=2&__1__=3` log entry.

    -   Use the e\_kv function.
        -   Raw log entry

            ```
            k1: q=asd&a=1&b=2&__1__=3
            ```

        -   Transformation rule

            ```
            # By default, keywords are extracted by using a specified character set.
            e_kv("k1")
            ```

        -   Result

            ```
            k1: q=asd&a=1&b=2
            q: asd
            a: 1
            b: 2
            ```

            **Note:** The keyword `__1__` is not extracted because it does not comply with the extraction constraints. For more information, see [Constraints on field name extraction](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).

    -   Use the e\_kv\_delimit function.
        -   Raw log entry

            ```
            k1: q=asd&a=1&b=2&__1__=3
            ```

        -   Transformation rule

            ```
            # After the key-value pair is separated by an ampersand (&), extract the keywords by using the ampersand (&).
            e_kv_delimit("k1", pair_sep=r"&")
            ```

        -   Result

            ```
            k1: q=asd&a=1&b=2
            q: asd
            a: 1
            b: 2
            ```

    -   Use the e\_regex function.
        -   Raw log entry

            ```
            k1: q=asd&a=1&b=2&__1__=3
            ```

        -   Transformation rule

            ```
            # Keywords and values are extracted by using a custom character set.
            e_regex("k1",r"(\w+)=([a-zA-Z0-9]+)",{r"\1": r"\2"})
            ```

        -   Result

            ```
            k1: q=asd&a=1&b=2
            q: asd
            a: 1
            b: 2
            ```

-   Example 2

    The following example describes three methods that you can use to extract keywords from the `content:k1=v1&k2=v2? k3:v3` log entry by using regular expressions:

    -   Use the e\_kv function.
        -   Raw log entry

            ```
            content:k1=v1&k2=v2? k3:v3
            ```

        -   Transformation rule

            ```
            e_kv("content",sep="(?:=|:)")
            ```

        -   Result

            ```
            content:k1=v1&k2=v2? k3:v3
            k1: v1
            k2: v2
            k3: v3
            ```

            **Note:** When the character set is passed to the `pari_sep`, `kv_sep`, or `sep` field, regular expressions that include a non-capturing group are used in the format of `(?:character set)`.

    -   Use the e\_kv\_delimit function.
        -   Raw log entry

            ```
            content:k1=v1&k2=v2? k3:v3
            ```

        -   Transformation rule

            ```
            e_kv_delimit("content",pair_sep=r"&?",kv_sep="(?:=|:)")
            ```

        -   Result

            ```
            content:k1=v1&k2=v2? k3:v3
            k1: v1
            k2: v2
            k3: v3
            ```

    -   Use the e\_regex function.
        -   Raw log entry

            ```
            content:k1=v1&k2=v2? k3:v3
            ```

        -   Transformation rule

            ```
            e_regex("content",r"([a-zA-Z0-9]+)[=|:]([a-zA-Z0-9]+)",{r"\1": r"\2"})
            ```

        -   Result

            ```
            content:k1=v1&k2=v2? k3:v3
            k1: v1
            k2: v2
            k3: v3
            ```

-   Example 3

    The following example shows how to use the `e_regex` function to extract keywords from complex strings.

    -   Raw log entry

        ```
        content :"ak_id:"LTAiscW,"ak_key:"rsd7r8f
        ```

    -   Transformation rule

        If double quotation marks \("\) exist in front of the keywords, you can use the `e_regex` function.

        ```
        e_regex("str",r'(\w+):(\"\w+)',{r"\1":r"\2"})
        ```

    -   Result

        The log format after DSL orchestration:

        ```
        content :"ak_id:"LTAiscW,"ak_key:"rsd7r8f
        ak_id: LTAiscW
        ak_key: rsd7r8f
        ```


## Extract values

-   Use the `e_kv` function to extract values if clear identifiers exist between dynamic key-value pairs or between keywords and values, such as `a=b`, or `a="cxxx"`. Example:
    -   Raw log entry

        ```
        content1:  k="helloworld",the change world, k2="good"
        ```

    -   Transformation rule

        In this case, `the change world` is not extracted.

        ```
        e_kv("content1")
        # The syntax of the e_kv_delimit function: A space character is required before k2. Therefore, k2 can be parsed only when the pair_sep parameter of the e_kv_delimit function is set to ",\s".
        e_kv_delimit("content1",kv_sep="=", pair_sep=",\s")
        # The syntax of the e_regex function.
        e_regex("str",r"(\w+)=(\"\w+)",{r"\1": r"\2"})
        ```

    -   Result

        The extracted log entry:

        ```
        content1:  k="helloworld",the change world, k2="good"
        k1: helloworld
        k2: good
        ```

-   To extract values from log entries that contain the `"` character in the `content:k1="v1=1"&k2=v2? k3=v3` format, we recommend that you use the `e_kv` function.

    -   Raw log entry

        ```
        content:k1="v1=1"&k2=v2? k3=v3
        ```

    -   Transformation rule

        ```
        e_kv("content",sep="=", quote="'")
        ```

    -   Result

        The extracted log entry:

        ```
        content: k1='v1=1'&k2=v2? k3=v3
        k1: v1=1
        k2:v2
        k3:v3
        ```

    If you use the `e_kv_delimit` function to extract values and the syntax is `e_kv_delimit("ctx", pair_sep=r"&?", kv_sep="=")`, only `k2: v2` and `k3: v3` can be parsed. The keyword `k1="v1` in the first key-value pair is dropped because the keyword does not comply with the extraction constraints. For more information, see [Constraints on field name extraction](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).

-   Some key-value pairs separated by delimiters contain special characters but they are not enclosed in specific characters. We recommend that you use the e\_kv\_delimit function to extract values from such key-value pairs. Example:
    -   Raw log entry

        ```
        content:  rats eat rice, oil|chicks eat bugs, rice|kittens eat fish, mice|
        ```

    -   Transformation rule \(recommended\)

        Use the e\_kv\_delimit function.

        ```
        e_kv_delimit("content", pair_sep="|", kv_sep=" eat ")
        ```

    -   Result \(recommended\)

        The parsed log entry:

        ```
        content:  rats eat rice, oil|chicks eat bugs, rice|kittens eat fish, mice|
        kittens:  fish, mice
        chicks:  bugs, rice
        rats:  rice, oil
        ```

    -   Transformation rule \(not recommended\)

        If you use the `e_kv` function, some log fields cannot be parsed.

        ```
        e_kv("f1", sep="eat")
        ```

    -   Result \(not recommended\)

        The parsed log entry:

        ```
        content:  rats eat rice, oil|chicks eat bugs, rice|kittens eat fish, mice|
        kittens:  fish
        chicks:  bugs
        rats:  rice
        ```


## Transform keywords

-   You can use the `e_kv` and `e_kv_delimit` functions to transform keywords and values by setting the prefix and suffix parameters in the format of `prefix="", suffix=""`.
    -   Raw log entry

        ```
        k1: q=asd&a=1&b=2
        ```

    -   Transformation rule

        ```
        e_kv("k1", sep="=", quote='"', prefix="start_", suffix="_end")
        e_kv_delimit("k1", pair_sep=r"&", kv_sep="=", prefix="start_", suffix="_end")
        e_regex("k1",r"(\w+)=([a-zA-Z0-9]+)",{r"start_\1_end": r"\2"})
        ```

    -   Result

        Log data is transformed into keywords in the following format:

        ```
        k1: q=asd&a=1&b=2
        start_q_end: asd
        start_a_end: 1
        start_b_end: 2
        ```

-   You can also use the `e_regex` function to transform the log entry. Example:
    -   Transformation rule

        ```
        e_regex("k1",r"(\w+)=([a-zA-Z0-9]+)",{r"\1_\1": r"\2"})
        ```

    -   Result

        Log data is transformed into keywords in the following format:

        ```
        k1: q=asd&a=1&b=2
        q_q: asd
        a_a: 1
        a_a: 2
        ```


## Transform values

-   Use the `e_kv` function if the log format is `k1:"v1\"abc"`, or double quotation marks exist in the log content. Example:
    -   Raw log entry

        ```
        """
        In this example, the backlash (\) character is not an escape character.
        """
        content2:  k1:"v1\"abc", k2:"v2", k3: "v3"
        ```

    -   Transformation rule 1

        ```
        e_kv("content2",sep=":", quote='"')
        ```



    -   Result 1

        The extracted log entry:

        ```
        content2:  k1:"v1\"abc", k2:"v2", k3: "v3"
        k1: v1\
        k2: v2
        k3: v3
        ```

    -   Transformation rule 2

        You can use the `e_kv` function to escape the `\` character by using the `escape` parameter. Example:

        ```
        e_kv("content2",sep=":", quote='"',escape=True)
        ```

    -   Result 2

        The extracted log entry:

        ```
        content2:  k1:"v1\"abc", k2:"v2", k3: "v3"
        k1: v1"abc
        k2: v2
        k3: v3
        ```

-   Use the `e_kv` function to extract key-value pairs if the log format is `a='k1=k2\';k2=k3'`. For example:
    -   Raw log entry

        ```
        data: i=c10 a='k1=k2\';k2=k3'
        ```

    -   Transformation rule 1

        In the `e_kv` function, the value of the `escape` parameter is False by default.

        ```
        e_kv("data", quote="'")
        ```

    -   Result 1

        The extracted log entry:

        ```
        a:  k1=k2\
        i:  c10
        k2:  k3
        ```

    -   Transformation rule 2

        You can use the `e_kv` function to escape the `\` character by using the `escape` parameter. Example:

        ```
        e_kv("data", quote="'", escape=True)
        ```

    -   Result 2

        The extracted log entry:

        ```
        data: i=c10 a='k1=k2\';k2=k3'
        i: c10
        a: k1=k2';k2=k3
        ```

-   Advanced transformation of key-value pairs
    -   Raw log entry

        ```
        content:  rats eat rice|chicks eat bugs|kittens eat fish|
        ```

    -   Transformation rule

        Use the `e_regex` function:

        ```
        e_regex("content", r"\b(\w+) eat ([^\|]+)", {r"\1": r"\2 by \1"})
        ```

    -   Result

        The transformed log entry:

        ```
        content:  rats eat rice|chicks eat bugs|kittens eat fish|
        kittens:  fish by kittens
        chicks:  bugs by chicks
        rats:  rice by rats
        ```


## Case studies

Assume that your company needs to extract the URL data from your website logs. You can customize the transformation rules based on your business requirements.

-   Initial transformation
    -   Requirements
        -   Requirement 1: Parse the `proto`, `domain`, and `param` fields from the log entries.
        -   Requirement 2: Expand the key-value pairs in the `param` field.
    -   Raw log entry

        ```
        __source__:  10.43.xx.xx
        __tag__:__client_ip__:  12.120.xx.xx
        __tag__:__receive_time__:  1563517113
        __topic__:  
        request:  https://yz.m.sm.cn/video/getlist/s?ver=3.2.3&app_type=supplier&os=Android8.1.0
        ```

    -   Functions
        -   General orchestration

            ```
            # Parse the request field.
            e_regex('request',grok("%{URIPROTO:uri_proto}://(?:%{USER:user}(?::[^@]*)? @)?(?:%{URIHOST:uri_domain})?(?:%{URIPATHPARAM:uri_param})?"))
            # Parse the uri_param field.
            e_regex('uri_param',grok("%{GREEDYDATA:uri_path}\? %{GREEDYDATA:uri_query}"))
            # Expand the key-value pairs.
            e_kv("uri_query")
            ```

        -   Specific orchestration and the transformation results
            1.  Use the Grok function to parse the `request` field.

                You can also use regular expressions to parse this field. For more information, see [Grok function](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Grok function.md) and [Grok patterns](/intl.en-US/Data Transformation/Data processing syntax/General reference/Grok patterns.md).

                ```
                e_regex('request',grok("%{URIPROTO:uri_proto}://(?:%{USER:user}(?::[^@]*)? @)?(?:%{URIHOST:uri_domain})?(?:%{URIPATHPARAM:uri_param})?"))
                ```

                Sub-result

                ```
                uri_domain:  yz.m.sm.cn
                uri_param:  /video/getlist/s? ver=3.2.3&app_type=supplier&os=Android8.1.0
                uri_proto:  https
                ```

            2.  Use the Grok function to parse the `uri_param` field.

                ```
                e_regex('uri_param',grok("%{GREEDYDATA:uri_path}\? %{GREEDYDATA:uri_query}"))
                ```

                Sub-result

                ```
                uri_path:  /video/getlist/s
                uri_query:  ver=3.2.3&app_type=supplier&os=Android8.1.0
                ```

            3.  Extract the `uri_param` field.

                ```
                e_kv("uri_query")
                ```

                Sub-result

                ```
                app_type:  supplier
                os:  Android8.1.0
                ver:  3.2.3
                ```

-   Result

    Preview the transformed log entry:

    ```
    __source__:  10.43.xx.xx
    __tag__:__client_ip__:  12.120.xx.xx
    __tag__:__receive_time__:  1563517113
    __topic__:  
    request:  https://yz.m.sm.cn/video/getlist/s?ver=3.2.3&app_type=supplier&os=Android8.1.0
    uri_domain:  yz.m.sm.cn
    uri_path:  /video/getlist/s
    uri_proto:  https
    uri_query:  ver=3.2.3&app_type=supplier&os=Android8.1.0
    app_type:  supplier
    os:  Android8.1.0
    ver:  3.2.3
    ```

    If you only need to parse the `request` field, you can use the [e\_kv](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value extraction functions.md) function. For example:

    ```
    e_kv("request")
    ```

    Preview the transformed log entry:

    ```
    __source__:  10.43.xx.xx
    __tag__:__client_ip__:  12.120.xx.xx
    __tag__:__receive_time__:  1563517113
    __topic__:  
    request:  https://yz.m.sm.cn/video/getlist/s?ver=3.2.3&app_type=supplier&os=Android8.1.0
    app_type:  supplier
    os:  Android8.1.0
    ver:  3.2.3
    ```

-   Advanced transformation

    If you want to extract the dynamic fields, such as the `ver`, `app_type`, and `os` fields, you can use regular expressions or the e\_kv\_delimit function. Example:

    -   Use regular expressions.

        ```
        e_regex("url", r"\b(\w+)=([^=&]+)", {r"\1": r"\2"})
        ```

    -   Use the `e_kv_delmit` function.

        ```
        e_kv_delimit("url", pair_sep=r"? &")
        ```

-   Conclusion

    Most URLs can be parsed by using the preceding functions. We recommend that you use the `e_kv` function to parse URLs from raw log entries.


