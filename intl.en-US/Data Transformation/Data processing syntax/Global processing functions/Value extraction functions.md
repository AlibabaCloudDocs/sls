# Value extraction functions

This topic describes the syntax of value extraction functions and provides parameter descriptions and function examples.

## Functions

|Type|Function|Description|
|----|--------|-----------|
|Regular expression-based extraction|[e\_regex](#section_1rn_crw_ur9)|Extracts the values of fields in an event by using a regular expression and assigns the values to other fields.|
|JSON-based extraction|[e\_json](#section_o7x_7rl_2qh)|Performs operations on JSON objects in specified event fields. The operations include expanding JSON data, extracting JSON data with JMESPath expressions, and extracting JSON data with JMESPath expressions and then expanding JSON data.|
|Delimiter-based extraction|[e\_csv, e\_psv, and e\_tsv](#section_ffl_ywn_her)|Extracts the values of multiple predefined fields based on user-defined delimiters. -   e\_csv: uses a comma \(,\) as the default delimiter.
-   e\_psv: uses a vertical bar \(\|\) as the default delimiter.
-   e\_tsv: uses a tab \(\\t\) as the default delimiter. |
|Key-value-based extraction|[e\_kv](#section_n3z_qjb_xpp)|Extracts key-value pairs of multiple source fields by using the quote parameter.|
|[e\_kv\_delimit](#section_1xc_xaw_0ph)|Extracts the key-value pairs of multiple source fields by using delimiters.|
|Syslog-based extraction|[e\_syslogrfc](#section_fsa_oy2_ye3)|Calculates the values of the facility and severity fields based on the known value of the priority field. Then, obtains the corresponding level information in the Request for Comments \(RFC\) protocol.|
|Anchor-rules-based extraction|[e\_anchor](#section_ud8_v7y_3rt)|Extracts the values between the specified start and end positions.|

## e\_regex

-   Syntax

    ```
    e_regex(Source field name, Regular expression with named capturing groups)
    e_regex(Source field name, Regular expression with non-capturing groups, fields_info(Destination field name))
    e_regex(Source field name, Regular expression with non-capturing groups, fields_info(An array of destination field names))
    e_regex(Source field name, Regular expression with capturing groups, fields_info(An array of destination field names))
    e_regex(Source field name, Regular expression with capturing groups, fields_info(Destination dictionary))
    e_regex(... Preceding parameters...mode="fill-auto")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Source field name|Arbitrary|Yes|The name of a source field. The field name can contain any characters. If the field does not exist in the specified event, no actions are performed. For more information about how to set special field names, see [Event structure and fields](/intl.en-US/Data Transformation/Data processing syntax/Data structures.md).|
    |Regular expression|String|Yes|The regular expression that is used to extract the value of a field. **Note:** Regular expressions with non-capturing groups are used in some scenarios. A non-capturing group uses the prefix that consists of a question mark and a colon \(`?:`\). For example, `\w+@\w+\.\w(?:\.\cn)?`. For more information about non-capturing groups, see [Non-capturing group](/intl.en-US/Data Transformation/Data processing syntax/General reference/Regular expressions.md). |
    |fields\_info|String/ List/ Dict|No|The name of the destination field. You must specify this parameter if you do not use a regular expression with named capturing groups.|
    |mode|String|No|The overwrite mode for a field. Default value: fill-auto. For information about other values of this parameter, see [Field check and overwrite modes](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).|

-   Response

    An event where new fields and values are added is returned.

-   Example 1: Extract the value of a single field.
    -   Raw log entry:

        ```
        msg:  192.168.1.1 http://... 127.0.0.0
        ```

    -   Transformation rule:

        ```
        e_regex("msg",r"\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}","ip")
        ```

    -   Result:

        ```
        msg:  192.168.1.1 http://... 127.0.0.0
        ip:  192.168.1.1
        ```

-   Example 2: Extract the values of multiple fields.
    -   Raw log entry:

        ```
        msg:  192.168.1.1 http://... 127.0.0.0
        ```

    -   Transformation rule:

        ```
        e_regex("msg",r"\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}",["server_ip","client_ip"])
        ```

    -   Result:

        ```
        msg:  192.168.1.1 http://... 127.0.0.0
        server_ip:  192.168.1.1
        client_ip:  127.0.0.0
        ```

-   Example 3: Extract the values of multiple fields by using capturing groups.
    -   Raw log entry:

        ```
        content:  start sys version: deficience, err: 2
        ```

    -   Transformation rule:

        ```
        e_regex("content",r"start sys version: (\w+),\s*err: (\d+)",["version","error"])
        ```

    -   Result:

        ```
        content:  start sys version: deficience, err: 2
        error:  2
        version:  deficience
        ```

-   Example 4: Extract the values of multiple fields by using named capturing groups.
    -   Raw log entry:

        ```
        content:  start sys version: deficience, err: 2
        ```

    -   Transformation rule:

        ```
        e_regex("content",r"start sys version: (? P<version>\w+),\s*err: (? P<error>\d+)")
        ```

    -   Result:

        ```
        content:  start sys version: deficience, err: 2
        error:  2
        version:  deficience
        ```

-   Example 5: Dynamically change the fields and extract the values of fields.
    -   Raw log entry:

        ```
        dict:  varify:123
        ```

    -   Transformation rule:

        ```
        e_regex("dict",r"(\w+):(\d+)",{r"k_\1": r"v_\2"})
        ```

    -   Result:

        ```
        dict:  varify:123
        k_varify:  v_123
        ```


## e\_json

-   Syntax

    ```
    e_json(Source field name, expand=None, depth=100, prefix="__", suffix="__", fmt="simple", sep=".", 
         expand_array=True, fmt_array="{parent}_{index}", 
         include_node=r"[\u4e00-\u9fa5\u0800-\u4e00a-zA-Z][\w\-\.]*",  
         exclude_node="", include_path="", exclude_path="",
         jmes="", output="", jmes_ignore_none=False, mode='fill-auto'
    )
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Source field name|String|Yes|The name of a source field. The field name can contain any characters. If the field does not exist in the specified event, no actions are performed. For more information about how to set special field names, see [Event structure and fields](/intl.en-US/Data Transformation/Data processing syntax/Data structures.md).|
    |expand|Bool|No|Specifies whether to expand the source field.     -   If the jmes field is not specified, the value of this parameter is set to True by default. This indicates that the source field is expanded.
    -   If the jmes field is specified, the value of this parameter is set to False by default. This indicates that the field is not expanded. |
    |depth|Number|No|The depth of field expanding. Value range: 1 to 2000. Default value: 100. The value 1 indicates that only the first tier of the field is expanded.|
    |prefix|String|No|The prefix added to an expanded field.|
    |suffix|String|No|The suffix added to an expanded field.|
    |fmt|String|No|The formatting method of a field. Valid values:     -   simple: The current node name is used as the field name. This is the default value. The format is `{prefix}{current}{suffix}`.
    -   full: The names of the current node and its closest parent node are combined and used as the field name. The format is `{parent_list_str}{sep}{prefix}{current}{suffix}`. `sep` specifies the delimiter. Default value: `.`.
    -   parent: The names of the current node and all its parent nodes are used as the field name. The format is `{parent}{sep}{prefix}{current}{suffix}`. `sep` specifies the delimiter. Default value: `.`.
    -   root: specifies to combine the names of the current node and its root node as the field name. The format is `{parent_list[0]}{sep}{prefix}{current}{suffix}`. `sep` specifies the delimiter. Default value:`.`. |
    |sep|String|No|The delimiter used to separate parent and child nodes during formatting. You must specify this parameter if the value of the `fmt` parameter is set to full, parent, or root. Default value: `.`.|
    |expand\_array|Bool|No|Specifies whether to expand an array of fields. To expand an array, the following format is used: `{parent}_{index}`.|
    |fmt\_array|String|No|The formatting method to expand an array of fields. The format is `{parent_rlist[0]}_{index}`. Alternatively, you can use up to five placeholders to expand an array: `parent_list`, `current`, `sep`, `prefix`, and `suffix`.|
    |include\_node|String/ Number|No|The whitelist of node names that are expanded. By default, node names that contain digits, letters, and specific special characters \(`_. -`\) are expanded.|
    |exclude\_node|String|No|The blacklist of node names that are not expanded.|
    |include\_path|String|No|The whitelist of node paths that are expanded.|
    |exclude\_path|String|No|The blacklist of node paths that are not expanded.|
    |jmes|String|No|The JMESPath expression used to convert field values into JSON objects and extract specific values.|
    |output|String|No|The field name that is returned for a value extracted by using a JMESPath expression.|
    |jmes\_ignore\_none|Bool|No|Specifies whether to skip a field if the value of this field cannot be extracted by using a JMESpath expression. Default value: True. This values indicates that a field is skipped if the value of this field cannot be extracted by using a JMESpath expression. Otherwise, an empty string is returned.|
    |mode|String|No|The overwrite mode for a field. Default value: fill-auto. For information about other values of this parameter, see [Field check and overwrite modes](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).|

-   JSON field expanding and filtering
    -   If a whitelist is specified, only the content included in the whitelist is displayed in the result. An example regular expression to specify a whitelist is `e_json("json_data_filed", ...., include_node=r'key\d+')`.
    -   If a blacklist is specified, only the content included in the blacklist is not displayed in the result. An example regular expression to specify a blacklist is `e_json("json_data_filed", ...., exclude_node=r'key\d+')`.
    -   The regular expressions `include_path` and `exclue_path` are used to match each specified node path. Periods \(`.`\) are used to separate the matched paths.
-   Filtering fields by using JMESPath expressions

    JMESPath expressions are used to select and compute data.

    -   Selects a list of element attributes from the specified JSON path: `e_json(..., jmes="cve.vendors[*].product",output="product")`.
    -   Splices element attributes from a specified JSON path by using commas \(,\): `e_json(..., jmes="join(',', cve.vendors[*].name)",output="vendors")`.
    -   Computes and retrieves the maximum value of each attribute of each element in the specified JSON path: `e_json(..., jmes="max(words[*].score)",output="hot_word")`.
    -   Returns an empty string if the specified JSON path is not located when computing the maximum value of each attribute of each element in the specified JSON path: `e_json(..., jmes="max(words[*].score)",output="hot_word")`.
-   parent\_list and parent\_rlist

    Examples

    Raw log entry:

    ```
    data: {
      "k1": 100,
      "k2": {
        "k3": 200,
        "k4": {
          "k5": 300
        }
      }
    }
    ```

    -   The parent\_list expression sorts the parent nodes from left to right.

        ```
        e_json("data", fmt='{parent_list[0]}-{parent_list[1]}#{current}')
        ```

        Result:

        ```
        data: ... <Raw log>
        data-k2#k3: 200
        data-k2#k5: 300
        ```

    -   The parent\_rlist expression sorts the parent nodes from right to left.

        ```
        e_json("data", fmt='{parent_list[1]}-{parent_list[0]}#{current}')
        ```

        Result:

        ```
        data: ... <Raw log>
        data-k2#k3: 200
        k2-k4#k5: 300
        ```

-   Response

    An event where new fields and values are added is returned.

    **Note:** If the e\_json function is used to parse a string that does not follow the JSON syntax, the string is not parsed and the raw string is returned.

-   Examples
    -   Example 1: Expand a field.
        -   Raw log entry:

            ```
            data: {"k1": 100, "k2": 200}
            ```

        -   Transformation rule:

            ```
            e_json("data",depth=1)
            ```

        -   Result:

            ```
            data: {"k1": 100, "k2": 200}
            k1: 100
            k2: 200
            ```

    -   Example 2: Add a prefix and suffix to a field.
        -   Raw log entry:

            ```
            data: {"k1": 100, "k2": 200}
            ```

        -   Transformation rule:

            ```
            e_json("data", prefix="data_", suffix="_end")
            ```

        -   Result:

            ```
            data: {"k1": 100, "k2": 200}
            data_k1_end: 100
            data_k2_end: 200
            ```

    -   Example 3: Expand a field in different formats.
        -   Raw log entry:

            ```
            data: {"k1": 100, "k2": {"k3": 200, "k4": {"k5": 300} } }
            ```

        -   Transformation rule:

            ```
            e_json("data", fmt='full')
            e_json("data", fmt='parent')
            e_json("data", fmt='root')
            ```

        -   Result:

            ```
            fmt='full':
              data: {"k1": 100, "k2": {"k3": 200, "k4": {"k5": 300} } }
              data.k1: 100
              data.k2.k3: 200
              data.k2.k4.k5: 300
            fmt='parent'
              data: {"k1": 100, "k2": {"k3": 200, "k4": {"k5": 300} } }
              data.k1: 100
              k2.k3: 200
              k4.k5: 300
            fmt='root':
              data: {"k1": 100, "k2": {"k3": 200, "k4": {"k5": 300} } }
              data.k1: 100
              data.k3: 200
              data.k5: 300
            ```

    -   Example 4: Specify the sep, prefix, and suffix parameters in the e\_json function.
        -   Raw log entry:

            ```
            data: {"k1": 100, "k2": {"k3": 200, "k4": {"k5": 300} } }
            ```

        -   Transformation rule:

            ```
            e_json("data", fmt='parent', sep="@", prefix="__", suffix="__")
            ```

        -   Result:

            ```
            data: {"k1": 100, "k2": {"k3": 200, "k4": {"k5": 300} } }
            data@__k1__: 100
            k2@__k3__: 200
            k4@__k5__: 300
            ```

    -   Example 5: Specify the fmt\_array parameter in the e\_json function.
        -   Raw log entry:

            ```
            people: [{"name": "xm", "sex": "boy"}, {"name": "xz", "sex": "boy"}, {"name": "xt", "sex": "girl"}]
            ```

        -   Transformation rule:

            ```
            e_json("people", fmt='parent', fmt_array="{parent_rlist[0]}-{index}")
            ```

        -   Result:

            ```
            people: [{"name": "xm", "sex": "boy"}, {"name": "xz", "sex": "boy"}, {"name": "xt", "sex": "girl"}]
            people-0.name: xm
            people-0.sex: boy
            people-1.name: xz
            people-1.sex: boy
            people-2.name: xt
            people-2.sex: girl
            ```

    -   Example 6: Use JMESPath expressions in the e\_json function.
        -   Raw log entry:

            ```
            data: { "people": [{"first": "James", "last": "d"}, 
                                 {"first": "Jacob", "last": "e"}],  
                      "foo": {"bar": "baz"}
                    }
            ```

        -   Transformation rule:

            ```
            e_json("data", jmes='foo', output='jmes_output0')
            e_json("data", jmes='foo.bar', output='jmes_output1')
            e_json("data", jmes='people[0].last', output='jmes_output2')
            e_json("data", jmes='people[*].first', output='jmes_output3')
            ```

        -   Result:

            ```
            jmes_output0: {"bar": "baz"}
            jmes_output1: baz
            jmes_output2: d
            jmes_output3: ["james", "jacob"]
            data: { "people": [{"first": "James", "last": "d"}, 
                                 {"first": "Jacob", "last": "e"}],  
                      "foo": {"bar": "baz"}
                    }
            ```


## e\_csv, e\_psv, and e\_tsv

-   Syntax

    ```
    e_csv(Source field name, Destination field list, sep=",", quote='"', restrict=True, mode="fill-auto")
    e_psv(Source field name, Destination field list, sep="|", quote='"', restrict=True, mode="fill-auto")
    e_tsv(Source field name, Destination field list, sep="\t", quote='"', restrict=True, mode="fill-auto")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Source field name|Arbitrary|Yes|The name of a source field. The field name can contain any characters. If the field does not exist in the specified event, no actions are performed. For more information about how to set special field names, see [Event structure and fields](/intl.en-US/Data Transformation/Data processing syntax/Data structures.md).|
    |Destination field list|Arbitrary|Yes|Field names that are separated by the specified delimiter. Field names can be an array of strings, such as `["error", "message", "result"]`.

If the field names do not contain commas \(,\), you can use commas \(,\) as delimiters between the field names, for example, `"error, message, result"`.

For more information about how to set special field names, see [Event structure and fields](/intl.en-US/Data Transformation/Data processing syntax/Data structures.md). |
    |sep|String|No|The delimiter used to separate each field name. It must be a single character.|
    |quote|String|No|The character used to enclose a value. You must specify this parameter if a field value contains delimiters.|
    |restrict|Bool|No|Specifies whether the restricted mode is enabled. Default value: False. This value indicates that the restricted mode is disabled. If the number of values that are separated differs from the number of destination field names:     -   In restricted mode, no actions are performed.
    -   In non-restricted mode, values are assigned to the fields that can be matched. |
    |mode|String|No|The overwrite mode for a field. Default value: fill-auto. For information about other values of this parameter, see [Field check and overwrite modes](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).|

-   Response

    This function returns an event that contains new field values.

-   Example: The `e_csv` function is used to extract field values. The `e_psv` and `e_tsv` functions work in a similar manner to the e\_csv function.
    -   Raw log entry:

        ```
        content:  106.39.189.28,10/Jun/2019:11:32:16 +0800,m.aliyun.cn,GET /zf/11874.html HTTP/1.1,200,0.077,6404,10.11.186.82:8001,200,0.060,https://yz.m.aliyun.cn/s?q=%E8%9B%8B%E8%8A%B1%E9%BE%99%E9%A1%BB%E9%9D%A2%E7%9A%84%E5%81%9A%E6%B3%95&from=wy878378&uc_param_str=dnntnwvepffrgibijbprsvdsei,-,Mozilla/5.0 (Linux; Android 9; HWI-AL00 Build/HUAWEIHWI-AL00) AppleWebKit/537.36,-,-
        ```

    -   Transformation rule:

        ```
        e_csv("content2", "remote_addr, time_local,host,request,status,request_time,body_bytes_sent,upstream_addr,upstream_status, upstream_response_time,http_referer,http_x_forwarded_for,http_user_agent,session_id,guid")
        ```

    -   Result:

        ```
        content:  106.39.189.28,10/Jun/2019:11:32:16 +0800,m.aliyun.cn,GET /zf/11874.html HTTP/1.1,200,0.077,6404,10.11.186.82:8001,200,0.060,https://yz.m.aliyun.cn/s?q=%E8%9B%8B%E8%8A%B1%E9%BE%99%E9%A1%BB%E9%9D%A2%E7%9A%84%E5%81%9A%E6%B3%95&from=wy878378&uc_param_str=dnntnwvepffrgibijbprsvdsei,-,Mozilla/5.0 (Linux; Android 9; HWI-AL00 Build/HUAWEIHWI-AL00) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Mobile Safari/537.36,-,-
          body_bytes_sent:  6404
        guid:  -
        host:  m.aliyun.cn
        http_referer:  https://yz.m.aliyun.cn/s?q=%E8%9B%8B%E8%8A%B1%E9%BE%99%E9%A1%BB%E9%9D%A2%E7%9A%84%E5%81%9A%E6%B3%95&from=wy878378&uc_param_str=dnntnwvepffrgibijbprsvdsei
        http_user_agent:  Mozilla/5.0 (Linux; Android 9; HWI-AL00 Build/HUAWEIHWI-AL00) AppleWebKit/537.36
        http_x_forwarded_for:  -
        remote_addr:  106.39.189.28
        request:  GET /zf/11874.html HTTP/1.1
        request_time:  0.077
        session_id:  -
        status:  200
        time_local:  10/Jun/2019:11:32:16 +0800
        topic:  syslog-forwarder
        upstream_addr:  10.11.186.82:8001
        upstream_response_time:  0.060
        upstream_status:  200
        ```


## e\_kv

-   Syntax

    ```
    e_kv(Source field name or source field list, sep="=", quote='"', escape=False, prefix="", suffix="", mode="fill-auto")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Source field name or source field list|String or string list|Yes|The name of the source field or names of source fields. For more information about how to set special field names, see [Event structure and fields](/intl.en-US/Data Transformation/Data processing syntax/Data structures.md).|
    |sep|String|No|The delimiter used to separate a key and its value in a regular expression. Default value: `=`. A delimiter can contain more than one character. **Note:** You can use non-capturing groups in a regular expression, but you cannot use capturing groups. For more information about grouping, see [Group](/intl.en-US/Data Transformation/Data processing syntax/General reference/Regular expressions.md). |
    |quote|String|No|The parameter used to enclose a field value. Default value: `"`. **Note:** We recommend that you specify this parameter to enclose a dynamic value extracted from a field, for example, `a="abc"` and `b="xyz"`. If you do not specify this parameter, the extracted field values can contain only the following characters: `letters, digits, underscores (_), hyphens (-), percent signs (%), and tildes (~)`. For example, the fields extracted from the expression `a=ab12_-.%~|abc b=123` are `a: ab12_-.%~` and `b: 123`. |
    |escape|Bool|No|Specifies whether to extract escape characters contained in a field value. Default value: `False`. This value indicates that escape characters contained in a field value are not extracted. For example, the value `abc\"xyz` of the `key` field is extracted from the expression `key="abc\"xyz"` by default. If the `escape` parameter is set to True, the extracted value is `abc"xyz`.|
    |prefix|String|No|The prefix added to an extracted field name.|
    |suffix|String|No|The suffix added to an extracted field name.|
    |mode|String|No|The overwrite mode for a field. Default value: fill-auto. For information about other values of this parameter, see [Field check and overwrite modes](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).|

-   Response

    An event where new fields and values are added is returned.

-   Examples
    -   Example 1: Use the e\_kv function in the default format to extract field values.
        -   Raw log entry:

            ```
            http_refer:  https://yz.m.sm.cn/s?q=asd&a=1&b=2
            ```

            **Note:** If the raw log entry is `request_uri: a1=1&a2=&a3=3` and the value of a2 is empty, a2 cannot be extracted by using the e\_kv\(\) function. However, you can extract the value of a2 by using the e\_regex\(\) function, for example, e\_regex\("request\_uri",r'\(\\w +\)=\(\[\>=&\] \*\)',\{r "\\1":r "\\2"\}, mode="overwrite"\).

        -   Transformation rule:

            ```
            e_kv("http_refer")
            ```

        -   Result:

            ```
            http_refer:  https://yz.m.sm.cn/s?q=asd&a=1&b=2
            q: asd
            a: 1
            b: 2
            ```

    -   Example 2: Add a prefix and suffix to extracted fields.
        -   Raw log entry:

            ```
            http_refer:  https://yz.m.sm.cn/s?q=asd&a=1&b=2
            ```

        -   Transformation rule:

            ```
            e_kv("http_refer",sep="=", quote='"', escape=False, prefix="data_", suffix="_end", mode="fill-auto")
            ```

        -   Result:

            ```
            http_refer:  https://yz.m.sm.cn/s?q=asd&a=1&b=2
            data_q_end: asd
            data_a_end: 1
            data_b_end: 2
            ```

    -   Example 3: Avoid extracting escape characters contained in a field value.
        -   Raw log entry:

            ```
            content2: k1:"v1\"abc", k2:"v2", k3: "v3"
            ```

        -   Transformation rule:

            ```
            e_kv("content2", escape=True)
            ```

        -   Result:

            ```
            content2:  k1:"v1\"abc", k2:"v2", k3: "v3"
            k1: v1"abc
            k2: v2
            k3: v3
            ```


## e\_kv\_delimit

-   Syntax

    ```
    e_kv_delimit(Source field name or source field list, pair_sep=r"\s", kv_sep="=", prefix="", suffix="", mode="fill-auto")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Source field name or source field list|String or string list|Yes|The name of the source field or names of source fields. For more information about how to set special field names, see [Event structure and fields](/intl.en-US/Data Transformation/Data processing syntax/Data structures.md).|
    |pair\_sep|String|No|The regular expression used to separate key-value pairs. Default value: `\s`. You can also use `\s\w` and `abc\s` to separate key-value pairs. **Note:** If you want to use strings to separate fields, we recommend that you use [str\_replace](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/String functions.md) or [regex\_replace](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Regular expression functions.md) to convert a string into characters. Then, you can set the characters as the value of the pair\_sep parameter in the e\_kv\_delimiter function to separate fields. |
    |kv\_sep|String|No|The regular expression that is used to separate key-value pairs. Default value: `=`. The regular expression can contain more than one character. **Note:** You can use non-capturing groups in a regular expression, but you cannot use capturing groups. For more information about grouping, see [Group](/intl.en-US/Data Transformation/Data processing syntax/General reference/Regular expressions.md). |
    |prefix|String|No|The prefix added to an extracted field name.|
    |suffix|String|No|The suffix added to an extracted field name.|
    |mode|String|No|The overwrite mode for a field. Default value: fill-auto. For information about other values of this parameter, see [Field check and overwrite modes](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).|

-   Response

    An event where new fields and values are added is returned.

-   Examples
    -   Example 1: Use the e\_kv\_delimit function in the default format to extract field values.
        -   Raw log entry:

            ```
            data: "i=c1 k1=v1 k2=v2 k3=v3"
            ```

            **Note:** If the raw log entry is `request_uri: a1=1&a2=&a3=3` and the value of a2 is empty, a2 cannot be extracted by using the e\_kv\(\) function. However, you can use the e\_regex\(\) function, for example, e\_regex\("request\_uri",r'\(\\w+\)=\(\[^=&\]\*\)',\{r"\\1":r"\\2"\}, mode="overwrite"\).

        -   Transformation rule:

            ```
            e_kv_delimit("data")
            ```

        -   Result:

            ```
            data: "i=c1 k1=v1 k2=v2 k3=v3"
            i: c1
            k2: v2
            k1: v1
            k3: v3
            ```

    -   Example 2: Extract fields whose names and values are separated by `&?`.
        -   Raw log entry:

            ```
            data: "k1=v1&k2=v2? k3=v3"
            ```

        -   Transformation rule:

            ```
            e_kv_delimit("data",pair_sep=r"&?")
            ```

        -   Result:

            ```
            data: "k1=v1&k2=v2? k3=v3"
            k2: v2
            k1: v1
            k3: v3
            ```

    -   Example 3: Extract fields by using a regular expression.
        -   Raw log entry:

            ```
            data: "k1=v1 k2:v2 k3=v3"
            ```

        -   Transformation rule:

            ```
            e_kv_delimit("data", kv_sep=r"(?:=|:)")
            ```

        -   Result:

            ```
            data: "k1=v1 k2:v2 k3=v3"
            k2: v2
            k1: v1
            k3: v3
            ```


## e\_syslogrfc

-   Syntax

    ```
    e_syslogrfc(Source field name, rfc, fields_info=None, mode='overwrite')
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Source field name|Arbitrary|Yes|The name of the source field. You must include content in the field to retrieve the value of the `priority` field.|
    |rfc|String|Yes|The RFC protocol that is used by syslog. Valid values: [SYSLOGRFC3164](https://tools.ietf.org/html/rfc3164) and [SYSLOGRFC5424](https://tools.ietf.org/html/rfc5424).|
    |fields\_info|Dict|No|Each field is represented by a key-value pair. The key is the original field name and the value is the new field name. The fields that can be renamed and their new names are shown below: `{" _severity_":"sev","_facility_":"fac","_severitylabel_":"sevlabel","_facilitylabel_":"faclabel"}`|
    |mode|String|No|The overwrite mode for the field. Default value: overwrite. For information about other values of this parameter, see [Field check and overwrite modes](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).|

-   Response

    An event where new fields and values are added is returned.

-   Examples
    -   Example 1: Use the e\_syslogrfc function in the default format to extract field values.
        -   Raw log entry:

            ```
            receive_time: 1558663265
            _priority_: 13
            _version_: 1
            _log_time_: 2019-05-06 11:50:16.015554+08:00
            _hostname_: iZbp1a65x3r1vhpe94fi2qZ
            _program_: root
            _procid_: -
            _msgid_: -
            _extradata_: -
            _content_: twish
            ```

        -   Transformation rule:

            ```
            e_syslogrfc("_priority_","SYSLOGRFC5424")
            ```

        -   Result:

            ```
            receive_time: 1558663265
            _priority_: 13
            _version_: 1
            _log_time_: 2019-05-06 11:50:16.015554+08:00
            _hostname_: iZbp1a65x3r1vhpe94fi2qZ
            _program_: root
            _procid_: -
            _msgid_: -
            _extradata_: -
            _content_: twish
            _facility_: 1
            _severity_: 5
            _severitylabel_: Notice: normal but significant condition
            _facilitylabel_: user-level messages
            ```

    -   Example 2: Specify the fields\_info parameter to extract renamed fields and values.
        -   Raw log entry:

            ```
            receive_time: 1558663265
            _priority_: 13
            _version_: 1
            _log_time_: 2019-05-06 11:50:16.015554+08:00
            _hostname_: iZbp1a65x3r1vhpe94fi2qZ
            _program_: root
            _procid_: -
            _msgid_: -
            _extradata_: -
            _content_: twish
            ```

        -   Transformation rule:

            ```
            # Rename fields.
            e_syslogrfc("_priority_","SYSLOGRFC5424",{"_facility_": "fac", "_severity_": "sev", "_facilitylabel_": "_facility_label_", "_severitylabel_": "_severity_label_"})
            ```

        -   Result:

            ```
            receive_time: 1558663265
            _priority_: 13
            _version_: 1
            _log_time_: 2019-05-06 11:50:16.015554+08:00
            _hostname_: iZbp1a65x3r1vhpe94fi2qZ
            _program_: root
            _procid_: -
            _msgid_: -
            _extradata_: -
            _content_: twish
            _facility_: 1
            _severity_: 5
            _severity_label_: Notice: normal but significant condition
            _facility_label_: user-level messages
            ```


## e\_anchor

Extracts the values between the specified start and end positions.

-   Syntax

    ```
    e_anchor (Source field name, anchor_rules,fields,restrict=False,mode="overwrite")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Source field name|Arbitrary|Yes|The field name to be extracted.|
    |anchor\_rules|String|Yes|The rules that are used to extract strings, for example, User = \*; Severity = \*;. The content to be extracted is indicated by an asterisk \(\*\). For logs that are displayed in the Log Service console in the Key : Value format, a space exists before the Value by default. When you specify the anchor\_rules parameter, do not enter the default space.

![e_anchor](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3952836161/p86457.png)

**Note:** When you specify source fields, you cannot use asterisks \(\*\) as prefixes or suffixes. |
    |fields|Arbitrary|Yes|The corresponding field of each value after the values of source fields are extracted. The fields can be in a string list, for example, \["user", "job", "result"\]. If the field names do not contain commas \(,\), you can use commas \(,\) to separate strings, for example, "user, job, result". For more information about how to set special field names, see [Event structure and fields](/intl.en-US/Data Transformation/Data processing syntax/Data structures.md). Special field names can contain special characters except for asterisks \(\*\). You can use an asterisk \(\*\) to skip a field. Example: Only user and result are extracted from "user,\*,result". For more information, see Example 10. |
    |restrict|Bool|No|Specifies whether to enable the restricted mode. Default value: False. This value indicates that the restricted mode is disabled. For example, assume that the number of values that are extracted differs from the number of the destination field names.     -   In restricted mode, no actions are performed.
    -   In non-restricted mode, values are assigned to the matched fields. |
    |mode|String|No|Default value: overwrite. For more information, see [Field check and overwrite modes](/intl.en-US/Data Transformation/Data processing syntax/General reference/Field extraction modes.md).|

-   Response

    The extracted content is returned.

-   Examples
    -   Example 1: Extract the values of specified fields from a log entry.

        ```
        e_anchor("content","User=*: severity=*:",["user_field","severity_field"])
        ```

        Raw log entry:

        ```
        content : "Aug 2 04:06:08: host=10.1.1.124: local/ssl2 notice mcpd[3772]: User=jsmith@demo.com: severity=warning: 01070638:5: Pool member 172.31.51.22:0 monitor status down."
        ```

        Result:

        ```
        content : "Aug 2 04:06:08: host=10.1.1.124: local/ssl2 notice mcpd[3772]: User=jsmith@demo.com: severity=warning: 01070638:5: Pool member 172.31.51.22:0 monitor status down."
        user_field : jsmith@demo.com
        severity_field : warning
        ```

    -   Example 2: Extract multiple values in the JSON array format.

        ```
        e_anchor("content",'name_list":*,"university":*},', ["name_list","universities"])
        ```

        Raw log entry:

        ```
        content : '"information":{"name_list":["Twiss","Evan","Wind","like"],"university":["UCL","Stanford University","CMU"]},"other":"graduate"'
        ```

        Result:

        ```
        content : '"information":{"name_list":["Twiss","Evan","Wind","like"],"university":["UCL","Stanford University","CMU"]},"other":"graduate"'
        name_list : ["Twiss","Evan","Wind","like"]
        universities : ["UCL","Stanford University","CMU"]
        ```

    -   Example 3: Extract values that contain special characters.

        ```
        e_anchor("content", "(+*) * \"*\"",["Year","Date","Msg"])
        ```

        Raw log entry:

        ```
        content : (+2019) June 24 "I am iron man"
        ```

        Result:

        ```
        content : (+2019) June 24 "I am iron man"
        Year : 2019
        Date : June 24
        Msg : I am iron man
        ```

    -   Example 4: Extract values that contain special characters.

        ```
        e_anchor("content", "\x09\x09\x09*/55.0 */537.36",["Google", "Apple"])
        ```

        Raw log entry:

        ```
        content : \x09\x09\x09Chrome/55.0 Safari/537.36
        ```

        Result:

        ```
        content : \x09\x09\x09Chrome/55.0 Safari/537.36
        Google : Chrome
        Apple : Safari
        ```

    -   Example 5: Extract values that contain special characters. All the characters that follow "MESSAGE:" are also the value of the content field.

        ```
        e_anchor("content","* INFO *: \n    To: *\n    Subject: *",["time","message","email","subject"])
        ```

        Raw log entry:

        ```
        content : 12:08:10,651 INFO sample_server ReportEmailer:178 - DEBUG SENDING MESSAGE: 
        To: example@aliyun.com
        Subject: New line Breaks in Message
        ```

        Result:

        ```
        content : 12:08:10,651 INFO sample_server ReportEmailer:178 - DEBUG SENDING MESSAGE: 
        To: example@aliyun.com
        Subject: New line Breaks in Message
        
        time : 12:08:10,651
        message : sample_server ReportEmailer:178 - DEBUG SENDING MESSAGE
        email : example@aliyun.com
        subject : New line Breaks in Message
        ```

    -   Example 6: Extract values that contain special characters. The extracted values are delimited by invisible tabs \(\\t\).

        ```
        e_anchor("content","\tI'm * in","word")
        # You can also use the following transformation rule to copy the value of the content field. However, you do not need to copy the default space character.
        e_anchor("content","    I'm * in","word")
        ```

        Raw log entry:

        ```
        content :   I'm tabbed in
        ```

        Result:

        ```
        content :   I'm tabbed in
        word : tabbed
        ```

    -   Example 7: Extract values that contain special characters. The extracted values are delimited by visible tabs \(\\t\).

        ```
        e_anchor("content","\tI'm * in","word")
        # You can also use the following transformation rule:
        e_anchor("content","    I'm * in","word")
        ```

        Raw log entry:

        ```
        content : \tI'm tabbed in
        ```

        Result:

        ```
        content : \tI'm tabbed in
        word : tabbed
        ```

    -   Example 8: Extract the values between the specified start and end positions in restricted mode.

        ```
        e_anchor("content","I * to * having",["v_word", "n_word","asd"],restrict=True)
        ```

        Raw log entry:

        ```
        content :  I used to love having snowball fight with my friends and building snowmen on the streets around our neighborhood
        ```

        Result:

        ```
        content : I used to love having snowball fight with my friends and building snowmen on the streets around our neighborhood
        ```

    -   Example 9: Extract the values between the specified start and end positions in non-restricted mode.

        ```
        e_anchor("content","love * fight with my * and",["test1","test2","test13"],restrict=False)
        ```

        Raw log entry:

        ```
        content :  I used to love having snowball fight with my friends and building snowmen on the streets around our neighborhood
        ```

        Result:

        ```
        content : I used to love having snowball fight with my friends and building snowmen on the streets around our neighborhood
        test1 : having snowball
        test2 : friends
        ```

    -   Example 10: Extract field values and rename field names.

        ```
        e_anchor('content', 'compare the * of natural disasters to man-made *', 'n-word,*')
        ```

        Raw log entry:

        ```
        content :  I used to love having snowball fight with my friends and building snowmen on the streets around our neighborhood
        ```

        Result:

        ```
        content : Could you compare the severity of natural disasters to man-made disasters
        n-word : severity                             
        ```


