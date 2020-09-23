# Parsing functions

This topic describes the syntax and parameters of the functions that are used to parse the User-Agent HTTP header. The topic also provides examples of these functions.

## Functions

|Function|Description|
|--------|-----------|
|[ua\_parse\_device](#section_fuu_dvd_msf)|Parses the device information in the User-Agent HTTP header.|
|[ua\_parse\_os](#section_2ut_enh_o37)|Parses the operating system information in the User-Agent HTTP header.|
|[ua\_parse\_agent](#section_msx_vt6_ew5)|Parses the browser information in the User-Agent HTTP header.|
|[ua\_parse\_all](#section_ikw_v71_zj9)|Parses all the information in the User-Agent HTTP header.|

**Note:** The parsing functions of the User-Agent HTTP header delete fields whose parsed values are None. For example, if the device information is parsed into \{'brand': None, 'family': 'Other', 'model': None\}, the brand and model fields are deleted and the parsing result is \{'family': 'Other'\}.

## ua\_parse\_device

Parses the device information in the User-Agent HTTP header.

-   Syntax

    ```
    ua_parse_device(value)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |value|String|Yes|The User-Agent HTTP header in the format of a string, for example, ua\_parse\_agent\(v\("http\_user\_agent"\)\).|

-   Response

    JSON-formatted data is returned.

-   Examples
    -   Raw log entry:

        ```
        http_user_agent:Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/192.168.0.0 Safari/537.36
        ```

    -   Transformation rule:

        ```
        e_set("new_column",ua_parse_device(v("http_user_agent")))
        ```

    -   Result:

        ```
        http_user_agent:Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/192.168.0.0 Safari/537.36
        new_column:{'family': 'Other'}
        ```


## ua\_parse\_os

Parses the operating system information in the User-Agent HTTP header.

-   Syntax

    ```
    ua_parse_os(value)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |value|String|Yes|The User-Agent HTTP header in the format of a string, for example, ua\_parse\_os\(v\("http\_user\_agent"\)\).|

-   Response

    JSON-formatted data is returned.

-   Examples
    -   Raw log entry:

        ```
        http_user_agent:Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/192.168.0.0 Safari/537.36
        ```

    -   Transformation rule:

        ```
        e_set("new_column",ua_parse_os(v("http_user_agent")))
        ```

    -   Result:

        ```
        http_user_agent:Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/192.168.0.0 Safari/537.36
        new_column:{'family': 'Mac OS X',
                            'major': '10',
                            'minor': '9',
                            'patch': '4'}
        ```


## ua\_parse\_agent

Parses the browser information in the User-Agent HTTP header.

-   Syntax

    ```
    ua_parse_agent(value)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |value|String|Yes|The User-Agent HTTP header in the format of a string, for example, ua\_parse\_all\(v\("http\_user\_agent"\)\).|

-   Response

    JSON-formatted data is returned.

-   Examples
    -   Raw log entry:

        ```
        http_user_agent:Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/192.168.0.0 Safari/537.36
        ```

    -   Transformation rule:

        ```
        e_set("new_column",ua_parse_agent(v("http_user_agent")))
        ```

    -   Result:

        ```
        http_user_agent:Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/192.168.0.0 Safari/537.36
        new_column:{'family': 'Chrome', 'major': '192', 'minor': '168', 'patch': '0'}
        ```


## ua\_parse\_all

Parses all the information in the User-Agent HTTP header.

-   Syntax

    ```
    ua_parse_all(value)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |value|String|Yes|The User-Agent HTTP header in the format of a string, for example, ua\_parse\_all\(v\("http\_user\_agent"\)\).|

-   Response

    JSON-formatted data is returned.

-   Examples
    -   Raw log entry:

        ```
        http_user_agent:Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/192.168.0.0 Safari/537.36
        ```

    -   Transformation rule:

        ```
        e_set("new_column",ua_parse_all(v("http_user_agent")))
        ```

    -   Result:

        ```
        http_user_agent:Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/192.168.0.0 Safari/537.36
        new_column:{   
            'device': {'family': 'Other',},
            'os': {   'family': 'Mac OS X',
                      'major': '10',
                      'minor': '9',
                      'patch': '4'},
            'user_agent': {   'family': 'Chrome',
                              'major': '192',
                              'minor': '168',
                              'patch': '0'}}
        ```


