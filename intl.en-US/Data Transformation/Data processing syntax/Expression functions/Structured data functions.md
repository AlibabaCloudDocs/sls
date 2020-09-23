# Structured data functions

This topic describes the syntax and parameters of functions that parse JSON-formatted, XML-formatted, and Protobuf-formatted data and functions that process IP addresses. This topic also provides examples of these functions.

## Functions

|Type|Function|Description|
|----|--------|-----------|
|JSON|[json\_select](#section_xrt_z1k_awb)|Extracts or calculates specific values from a JSON expression based on the JMESPath syntax.|
|[json\_parse](#section_n3w_bun_us1)|Parses a JSON text to a JSON object.|
|XML|[xml\_to\_json](#section_inr_nya_rp6)|Converts XML-formatted data to JSON-formatted data.|
|Protobuf|[pb\_to\_json](#section_0rt_ljv_e56)|Converts Protobuf-formatted data to JSON-formatted data.|
|Gzip|[gzip\_compress](#section_52q_xji_m92)|Compresses data and then encodes the data by using the Base64 algorithm. The returned data includes the raw data and the encoded data.|
|[gzip\_decompress](#section_thp_iir_o9q)|Decodes data by using the Base64 algorithm and then decompresses the data. The returned data includes the raw data and the decoded data.|
|IP|[geo\_parse](#section_a6e_5e9_q0c)|Identifies the city, province, and country to which data belongs based on an IP address.|
|[ip\_cidrmatch](#section_2bd_uyj_rho)|Checks whether an IP address belongs to a Classless Inter-Domain Routing \(CIDR\) block.|
|[ip\_version](#section_xbo_zvp_eg8)|Checks whether the version of an IP address is IPv4 or IPv6.|
|[ip\_type](#section_bhb_m61_hwh)|Determines the type of an IP address.|
|[ip\_makenet](#section_r64_xsl_h67)|Transforms an IP address to a CIDR block.|
|[ip\_to\_format](#section_e4e_d96_1m3)|Converts the format of a CIDR block to a format that specifies the netmask or prefix length of the CIDR block.|
|[ip\_overlaps](#section_ny8_rdb_ouw)|Checks whether two CIDR blocks overlap.|

## json\_select

-   Syntax

    ```
    json_select(JSON text, "JMESPath expression", default=None, restrict=False)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |JSON text|Arbitrary|Yes|The JSON expression or fields from which a field is extracted.|
    |JMESPath expression|String|Yes|The JMESPath expression, which represents the field to be extracted.|
    |default|Arbitrary|No|If the field to be extracted does not exist, the value of the default parameter is returned. The default value of the default parameter is None.|
    |restrict|Boolean|No|Default value: False. Indicates whether the restricted mode is enabled. Default value: False. If the value of the `restrict` parameter is set to True, the restricted mode is enabled. In restricted mode, an error is returned if non-standard JSON text is specified.|

-   Response

    The extracted field value is returned.

-   Examples
    -   Example 1: Extract a value of the name field.
        -   Raw log entry:

            ```
            content:  {"name": "xiaoming", "age": 10}
            ```

        -   Transformation rule:

            ```
            e_set("json_filter",json_select(v("content"), "name"))
            ```

        -   Transformation rule:

            ```
            content:  {"name": "xiaoming", "age": 10}
            json_filter:  xiaoming
            ```

    -   Example 2: Extract all values of the name field.
        -   Raw log entry:

            ```
            content:  {"name": ["xiaoming", "xiaowang", "xiaoli"], "age": 10}
            ```

        -   Result:

            ```
            e_set("json_filter",json_select(v("content"), "name[*]"))
            e_set("json_filter2",str_join("*", json_select(v('content'), "name[*]")))
            ```

        -   Result:

            ```
            content:  {"name": ["xiaoming", "xiaowang", "xiaoli"], "age": 10}
            json_filter:  ["xiaoming", "xiaowang", "xiaoli"]
            json_filter2: xiaoming*xiaowang*xiaoli
            ```

    -   Example 3: Extract the value of the name field and return the value of the default parameter.
        -   Raw log entry:

            ```
            content:  {"name": "xiaoming", "age": 10}
            ```

        -   Transformation rule:

            ```
            e_set("json_filter",json_select(v("content"), "name1.name2", default="default"))
            ```

        -   Result:

            ```
            content:  {"name": "xiaoming", "age": 10}
            json_filter: default
            ```


## json\_parse

-   Syntax

    ```
    json_parse(JSON text, default=None, restrict=False)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |JSON text|String|Yes|The field that needs to be parsed.|
    |default|Arbitrary|No|If the field to be parsed does not exist, the value of the default parameter is returned. The default value of the default parameter is None.|
    |restrict|Boolean|No|Default value: False. Indicates whether the restricted mode is enabled. Default value: False. If the value of the `restrict` parameter is set to True, the restricted mode is enabled. In restricted mode, an error is returned if non-standard JSON text is specified.|

-   Response

    A JSON object is returned.

-   Examples
    -   Raw log entry:

        ```
        content:  {"abc": 123, "xyz": "test" }
        ```

    -   Transformation rule:

        ```
        e_set("json",json_parse(v("content")))
        ```

    -   Result:

        ```
        content:  {"abc": 123, "xyz": "test" }
        json:  {"abc": 123, "xyz": "test"}
        ```


## xml\_to\_json

-   Syntax

    ```
    xml_to_json(XML text)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |XML text|String|Yes|The XML field that needs to be converted.|

-   Response

    JSON-formatted data is returned.

-   Examples
    -   Raw log entry:

        ```
        str : <? xmlversion="1.0"? ><data><countryname="Liechtenstein"><rank>1</rank><year>2008</year><gdppc>141100</gdppc><neighborname="Austria"direction="E"/><neighborname="Switzerland"direction="W"/></country><countryname="Singapore"><rank>4</rank><year>2011</year><gdppc>59900</gdppc><neighborname="Malaysia"direction="N"/></country><countryname="Panama"><rank>68</rank><year>2011</year><gdppc>13600</gdppc><neighborname="Costa Rica"direction="W"/><neighborname="Colombia"direction="E"/></country></data>
        ```

    -   Transformation rule:

        ```
        e_set("str_json",xml_to_json(v("str")))
        ```

    -   Result:

        ```
        str : <? xmlversion="1.0"? ><data><countryname="Liechtenstein"><rank>1</rank><year>2008</year><gdppc>141100</gdppc><neighborname="Austria"direction="E"/><neighborname="Switzerland"direction="W"/></country><countryname="Singapore"><rank>4</rank><year>2011</year><gdppc>59900</gdppc><neighborname="Malaysia"direction="N"/></country><countryname="Panama"><rank>68</rank><year>2011</year><gdppc>13600</gdppc><neighborname="Costa Rica"direction="W"/><neighborname="Colombia"direction="E"/></country></data>
        str_json : {"data": {"country": [{"@name": "Liechtenstein", "rank": "1", "year": "2008", "gdppc": "141100", "neighbor": [{"@name": "Austria", "@direction": "E"}, {"@name": "Switzerland", "@direction": "W"}]}, {"@name": "Singapore", "rank": "4", "year": "2011", "gdppc": "59900", "neighbor": {"@name": "Malaysia", "@direction": "N"}}, {"@name": "Panama", "rank": "68", "year": "2011", "gdppc": "13600", "neighbor": [{"@name": "Costa Rica", "@direction": "W"}, {"@name": "Colombia", "@direction": "E"}]}]}}
        ```


## pb\_to\_json

-   Syntax

    ```
    pb_to_json(pb_data,pb_py_path_str,pb_fun_name)
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |pb\_data|String|Yes|The Protobuf-formatted data or fields that need to be converted into JSON-formatted data.|
    |pb\_py\_path\_str|String|Yes|The name of the Protobuf file. For example, the name of the `addressbook.proto` file is `addressbook`.|
    |pb\_fun\_name|String|Yes|The name of the message in the Protobuf file.|

-   Response

    JSON-formatted data is returned.

-   Examples

    addressbook.proto file:

    ```
    '''addressbook.proto file'''
    syntax = "proto3";
    message Person {
      string name = 1;
      int32 id = 2;
      string email = 3;
      string university = 5;
      int32 age = 6;
    
      enum PhoneType {
        MOBILE = 0;
        HOME = 1;
        WORK = 2;
      }
    
      message PhoneNumber {
        string number = 1;
        PhoneType type = 2;
      }
    
      repeated PhoneNumber phones = 4;
    }
    
    message AddressBook {
      repeated Person people = 1;
    }
    ```

    -   Raw log entry:

        ```
        content: \n3\n\x05twiss\x10\x01\x1a\x0ftwiss@gkate.com"\x0f\n\x0b13099922287\x10\x01*\x04Henu0\x18\nC\n\x04Iran\x10\x02\x1a\x10Iran@alibaba.com"\x0f\n\x0b13022244455\x10\x01*\x14West Leak University0\x02
        ```

    -   Transformation rule:

        ```
        e_set("data_json",pb_to_json(v("content"),"addressbook","AddressBook"))
        ```

    -   Result:

        ```
        content: \n3\n\x05twiss\x10\x01\x1a\x0ftwiss@gkate.com"\x0f\n\x0b13099922287\x10\x01*\x04Henu0\x18\nC\n\x04Iran\x10\x02\x1a\x10Iran@alibaba.com"\x0f\n\x0b13022244455\x10\x01*\x14West Leak University0\x02
        data_json : {'people': [{'name': 'twiss', 'id': 1, 'email': 'twiss@gkate.com', 'phones': [{'number': '13099922287', 'type': 1}], 'university': 'Henu', 'age': 24}, {'name': 'Iran', 'id': 2, 'email': 'Iran@alibaba.com', 'phones': [{'number': '13022244455', 'type': 1}], 'university': 'West Leak University', 'age': 2}]}
        ```


## geo\_parse

-   Syntax

    ```
    geo_parse("ip",ip_db,keep_fields=None,provider="ipip")
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |IP|IP address string|Yes|The IP address.|
    |ip\_db|IP address database|Yes|The IP address database. The `res_oss_file(endpoint, ak_id, ak_key, bucket, file, format='text', change_detect_interval=0,fetch_interval=2,refresh_retry_max=60,encoding='utf8',error='ignore')` method is used to access the IP address database. The `format` parameter is set to binary. For more information about the parameters in the res\_oss\_file method, see [res\_oss\_file](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.md).|
    |keep\_fields|Tuple|No|The keys that are contained in the returned dictionary. By default, the following keys are contained in the returned dictionary:     -   city: the name of the city
    -   region: the name of the province
    -   country: the name of the country
For example, `keep_fields=("city","country")` indicates that the `city` and `country` keys are returned.

The `keep_fields` parameter can also be used to rename the keys in the dictionary. For example, `(("city","cty"),("country","state"))` indicates that the city and country keys are respectively renamed `cty` and `state` in the returned data. |
    |provider|String|No|Valid values:     -   ipip: uses the IPDB-formatted binary IP address library provided by IPIP to parse IP addresses. To download the library, visit [ipip](https://en.ipip.net/?origin=CN). This value is the default value.
    -   ip2location: uses the global binary IP address library provided by IP2Location to parse IP addresses. To download the library, visit [ip2location](https://lite.ip2location.com/database/ip-country-region-city). Only a binary IP address library is supported. |

-   Response

    A dictionary is returned in the following format:

    ```
    {
      "city": "...",
      "province":"...",
      "country": "..."
    }
    ```

-   Examples
    -   Example 1: Identify the country, province, and city of origin of an IP address.
        -   Raw log entry:

            ```
            ip : 1.2.3.4
            ```

        -   Transformation rule:

            ```
            e_set("geo",geo_parse(v("ip"), ip_db=res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',
                                                             ak_id='your ak_id',
                                                             ak_key='your ak_key',
                                                             bucket='your bucket', file='ipipfree.ipdb',
                                                                           format='binary',change_detect_interval=20)))
            ```

        -   Result:

            ```
            ip : 1.2.3.4
            geo :{'city': 'Hangzhou', 'province':'Zhejiang Province','country': 'China'}
            ```

    -   Example 2: Select keys to return and rename the keys.
        -   Raw log entry:

            ```
            ip : 1.2.3.4
            ```

        -   Transformation rule:

            ```
            e_set("geo",geo_parse(v("ip"), ip_db=res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',
                                                             ak_id='your ak_id',
                                                             ak_key='your ak_key',
                                                             bucket='your bucket', file='ipipfree.ipdb',
                                                                           format='binary',change_detect_interval=20),keep_fields=(("city","cty"),("country","state"),("province","pro"))))
            ```

        -   Result:

            ```
            ip : 1.2.3.4
            geo :{ "state": "China","pro": "Zhejiang Province","cty": "Hangzhou"}
            ```

    -   Example 3: Select keys to return.
        -   Raw log entry:

            ```
            ip : 1.2.3.4
            ```

        -   Transformation rule:

            ```
            e_set("geo",geo_parse(v("ip"), ip_db=res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',
                                                             ak_id='your ak_id',
                                                             ak_key='your ak_key',
                                                             bucket='your bucket', file='ipipfree.ipdb',
                                                                           format='binary',change_detect_interval=20),keep_fields=("country","province")))
            ```

        -   Result:

            ```
            ip : 1.2.3.4
            geo :{ "country": "China","province": "Zhejiang Province"}
            ```

    -   Example 4: Use the global binary IP address library provided by IP2Location to parse an IP address.

        -   Raw log entry:

            ```
            ip:19.0.0.0
            ```

        -   Transformation rule:

            ```
            e_set("geo", geo_parse(v("ip"), ip_db=res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',ak_id="your ak_id", ak_key="your ak_serect", bucket='log-etl-staging', file='your ip2location bin file', format='binary', change_detect_interval=20),provider="ip2location"))
            ```

        -   Result:

            ```
            ip:19.0.0.0
            geo: {"city":"Dearborn","province":"Michigan","country":"United States"}
            ```

        If you set the value of the provider parameter to ip2location, the open source SDK for Python provided by IP2Location is used to transform input IP addresses. For more information, visit [ip2location python sdk](https://www.ip2location.com/development-libraries/ip2location/python). The SDK for Python provided by IP2Location can parse the following fields. If a field cannot be parsed, check whether the field is included in the IP address library provided by IP2Location.

        ```
        country_short
        country_long /  The country field is specified for data transformation.
        region  / The province field is specified for data transformation.
        city
        isp
        latitude
        longitude
        domain
        zipcode
        timezone
        netspeed
        idd_code
        area_code
        weather_code
        weather_name
        mcc
        mnc
        mobile_brand
        elevation
        usage_type                               
        ```


## gzip\_compress

This function compresses data and then encodes the data by using the Base64 algorithm. The returned data includes the raw data and the encoded data.

-   Syntax

    ```
    gzip_compress(v("data"),compresslevel=6,to_format="base64",encoding="utf-8'")
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |data|Arbitrary|Yes|The data to be compressed.|
    |compresslevel|Integer|No|The compression level. Valid values: 0 to 9. Default value: 6.     -   1: Data is compressed at the highest speed but with the lowest compression ratio.
    -   9: Data is compressed at the lowest speed but with the highest compression ratio.
    -   0: Data is not compressed. |
    |to\_format|String|No|Encode the compressed data by using the Base64 algorithm. Only the Base64 algorithm is supported.|
    |encoding|String|No|The encoding format. Default value: utf-8. For more information about other encoding formats, see [Standard encoding formats](/intl.en-US/Data Transformation/Data processing syntax/General reference/Standard encoding formats.md).|

-   Response

    Compressed and Base64-encoded data is returned.

-   Examples
    -   Raw log entry:

        ```
        content: I always look forward to my holidays whether I travel or stay at home.
        ```

    -   Transformation rule:

        ```
        e_set("base64_encode_gzip_compress",gzip_compress(v("content"),to_format="base64"))
        ```

    -   Result:

        ```
        content: I always look forward to my holidays whether I travel or stay at home.
        base64_encode_gzip_compress: H4sIAA8JXl4C/xXK0QmAMAwFwFXeBO7RMQKNREx5kAZDtle/7wbES3rDyRsnoyQmklgNo1/ztzJN08BAhjzqYGCnNCS/tPR4AcgrnWVGAAAA
        ```


## gzip\_decompress

This function decodes data by using the Base64 algorithm and then decompresses the data. The returned data includes the raw data and the decoded data.

-   Syntax

    ```
    gzip_decompress(v("data"),from_format="base64",encoding="utf-8'")
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |data|Arbitrary|Yes|The data to be decompressed.|
    |from\_format|String|No|Decode the compressed data by using the Base64 algorithm. Only the Base64 algorithm is supported.|
    |encoding|String|No|The encoding format. Default value: utf-8. For more information about other encoding formats, see [Standard encoding formats](/intl.en-US/Data Transformation/Data processing syntax/General reference/Standard encoding formats.md).|

-   Response

    Decompressed and Base64-decoded data is returned.

-   Syntax
    -   Raw log entry:

        ```
        content: H4sIAA8JXl4C/xXK0QmAMAwFwFXeBO7RMQKNREx5kAZDtle/7wbES3rDyRsnoyQmklgNo1/ztzJN08BAhjzqYGCnNCS/tPR4AcgrnWVGAAAA
        ```

    -   Transformation rule:

        ```
        e_set("gzip_decompress",gzip_decompress(v("content"),from_format="base64"))
        ```

    -   Result:

        ```
        content: H4sIAA8JXl4C/xXK0QmAMAwFwFXeBO7RMQKNREx5kAZDtle/7wbES3rDyRsnoyQmklgNo1/ztzJN08BAhjzqYGCnNCS/tPR4AcgrnWVGAAAA
        gzip_decompress: I always look forward to my holidays whether I travel or stay at home.
        ```


## ip\_cidrmatch

This function checks whether an IPv4 address or IPv6 address belongs to a CIDR block. If an IP address belongs to a CIDR block, True is returned.

-   Syntax

    ```
    ip_cidrmatch("CIDR block","IP address",default="")
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |CIDR block|String|Yes|The CIDR block, for example, 123.132.32.0/25.|
    |IP address|String|Yes|The IP address.|
    |default|Arbitrary|No|If the specified IP address does not belong to the specified CIDR block, the value of this parameter is returned. You can set the value of this parameter to an empty string.|

-   Response

    If the specified IP address belongs to the specified CIDR block, True is returned. Otherwise, False is returned.

-   Examples
    -   Example 1: The specified IPv4 address belongs to the specified CIDR block.
        -   Raw log entry:

            ```
            cidr_subnet: 192.168.1.0/24
            Ip: 192.168.1.100
            ```

        -   Transformation rule:

            ```
            e_set("is_belong",ip_cidrmatch(v("cidr_subnet"),v("ip")))
            ```

        -   Result:

            ```
            cidr_subnet: 192.168.1.0/24
            ip: 192.168.1.100
            is_belong: true
            ```

    -   Example 2: The specified IPv4 address does not belong to the specified CIDR block.
        -   Raw log entry:

            ```
            cidr_subnet: 192.168.1.0/24
            Ip: 10.10.1.100
            ```

        -   Transformation rule:

            ```
            e_set("is_belong",ip_cidrmatch(v("cidr_subnet"),v("ip")))
            ```

        -   Result:

            ```
            cidr_subnet: 192.168.1.0/24
            ip: 10.10.1.100
            is_belong: false
            ```

    -   Example 3: The specified IP address cannot be matched with the specified CIDR block.
        -   Raw log entry:

            ```
            cidr_subnet: -11
            Ip: 10.10.1.100
            ```

        -   Transformation rule:

            ```
            e_set("is_belong",ip_cidrmatch(v("cidr_subnet"),v("ip"),default="unknown"))
            ```

        -   Result:

            ```
            cidr_subnet: -11
            ip: 10.10.1.100
            is_belong: unknown
            ```


## ip\_version

This function checks whether the version of an IP address is IPv4 or IPv6.

-   Syntax

    ```
    ip_version("IP address",default="")
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |IP address|String|Yes|The IP address.|
    |default|Arbitrary|No|If the version of the specified IP address cannot be determined, the value of this parameter is returned. You can set the value of this parameter to an empty string.|

-   Response

    IPv6 or IPv4 is returned.

-   Examples
    -   Example 1: The specified IP address is an IPv4 address.
        -   Raw log entry:

            ```
            Ip: 192.168.1.100
            ```

        -   Transformation rule:

            ```
            e_set("version",ip_version(v("ip")))
            ```

        -   Result:

            ```
            ip: 192.168.1.100
            version: IPv4
            ```

    -   Example 2: The specified IP address is an IPv6 address.
        -   Raw log entry:

            ```
            Ip: ::1
            ```

        -   Transformation rule:

            ```
            e_set("version",ip_version(v("ip")))
            ```

        -   Result:

            ```
            ip: ::1
            version: IPv6
            ```


## ip\_type

This function determines the type of an IP address.

-   Syntax

    ```
    ip_type("IP address",default="")
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |IP address|String|Yes|The IP address.|
    |default|Arbitrary|No|If the type of the specified IP address cannot be determined, the value of this parameter is returned. You can set the value of this parameter to an empty string.|

-   Response

    A value that indicates the type of the IP address is returned, for example, private, reserved, loopback, public, or unknown.

-   Examples
    -   Example 1: The specified IP address is a loopback address.
        -   Raw log entry:

            ```
            ip: 127.0.0.1
            ```

        -   Transformation rule:

            ```
            e_set("type",ip_type(v("ip")))
            ```

        -   Result:

            ```
            ip: 127.0.0.1
            type: loopback
            ```

    -   Example 2: The specified IP address is a private IP address.
        -   Raw log entry:

            ```
            ip:  192.168.1.1
            ```

        -   Transformation rule:

            ```
            e_set("type",ip_type(v("ip")))
            ```

        -   Result:

            ```
            ip:  192.168.1.1
            type: private
            ```

    -   Example 3: The specified IP address is a public IP address.
        -   Raw log entry:

            ```
            Ip: 195.185.1.2
            ```

        -   Transformation rule:

            ```
            e_set("type",ip_type(v("ip")))
            ```

        -   Result:

            ```
            ip: 195.185.1.2
            type: public
            ```

    -   Example 4: The specified IP address is an IPv6 loopback address.
        -   Raw log entry:

            ```
            Ip: ::1
            ```

        -   Transformation rule:

            ```
            e_set("type",ip_type(v("ip")))
            ```

        -   Result:

            ```
            ip: ::1
            type: loopback
            ```

    -   Example 5: The specified IP address is an allocated RIPE NCC address.
        -   Raw log entry:

            ```
            Ip: 2001:0658:022a:cafe:0200::1
            ```

        -   Transformation rule:

            ```
            e_set("type",ip_type(v("ip")))
            ```

        -   Result:

            ```
            ip: 2001:0658:022a:cafe:0200::1
            type: allocated ripe ncc
            ```


## ip\_makenet

This function transforms an IP address to a CIDR block.

-   Syntax

    ```
    ip_makenet("IP address","netmask",default="")
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |IP address|String|Yes|The IP address.|
    |Netmask|String|Yes|The netmask, for example, 255.255.255.0. **Note:** If you specify an IP address range for the IP address parameter, you can set the Subnet parameter to an empty string. |
    |default|Arbitrary|No|If the specified IP address cannot be transformed to a CIDR block, the value of this parameter is returned. You can set the value of this parameter to an empty string.|

-   Response

    A CIDR block is returned.

-   Examples
    -   Example 1: Transform an IP address to a CIDR block.
        -   Raw log entry:

            ```
            Ip: 192.168.1.0
            ```

        -   Transformation rule:

            ```
            e_set("makenet",ip_makenet(v("ip"),"255.255.255.0"))
            ```

        -   Result:

            ```
            ip: 192.168.1.0
            makenet: 192.168.1.0/24
            ```

    -   Example 2: Convert an IP address range to a CIDR block.
        -   Raw log entry:

            ```
            Ip: 192.168.1.0-192.168.1.255
            ```

        -   Transformation rule:

            ```
            e_set("makenet",ip_makenet(v("ip")))
            ```

        -   Result:

            ```
            ip: 192.168.1.0-192.168.1.255
            makenet: 192.168.1.0/24
            ```

    -   Example 3: Convert an IP address range to a CIDR block.
        -   Raw log entry:

            ```
            Ip: 192.168.1.0/255.255.255.0
            ```

        -   Transformation rule:

            ```
            e_set("makenet",ip_makenet(v("ip")))
            ```

        -   Result:

            ```
            ip: 192.168.1.0/255.255.255.0
            makenet: 192.168.1.0/24
            ```


## ip\_to\_format

This function converts the format of a CIDR block to a format that specifies the netmask or prefix length of the CIDR block.

-   Syntax

    ```
    ip_to_format("CIDR block", format code, default="")
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |CIDR block|String|Yes|The CIDR block, for example, 192.168.1.0/24.|
    |Format code|Integer|No|The format of the returned CIDR block. Valid values:     -   0: returns the original CIDR block.
    -   1: returns a CIDR block that specifies the prefix length of the CIDR block.
    -   2: returns a CIDR block that specifies the netmask of the CIDR block.
    -   3: returns an IP address range. |
    |default|Arbitrary|No|If the specified CIDR block cannot be formatted, the value of this parameter is returned. You can set the value of this parameter to an empty string.|

-   Response

    A CIDR block of the specified format is returned.

-   Examples
    -   Example 1: Return the original CIDR block.
        -   Raw log entry:

            ```
            Ip: 192.168.1.0/24
            ```

        -   Transformation rule:

            ```
            e_set("strNormal",ip_to_format(v("ip"),0))
            ```

        -   Result:

            ```
            ip: 192.168.1.0/24
            strNormal: 192.168.1.0/24
            ```

    -   Example 2: Convert the format of a CIDR block to a format that specifies the prefix length of the CIDR block.
        -   Raw log entry:

            ```
             Ip: 192.168.1.0/24
            ```

        -   Transformation rule:

            ```
            e_set("strNormal",ip_to_format(v("ip"),1))
            ```

        -   Result:

            ```
            ip: 192.168.1.0/24
            strNormal: 192.168.1.0/24
            ```

    -   Example 3: Convert the format of a CIDR block to a format that specifies the netmask of the CIDR block.
        -   Raw log entry:

            ```
            Ip: 192.168.1.0/24
            ```

        -   Transformation rule:

            ```
            e_set("strNormal",ip_to_format(v("ip"),2))
            ```

        -   Result:

            ```
            ip: 192.168.1.0/24
            strNormal: 192.168.1.0/255.255.255.0
            ```

    -   Example 4: Convert a CIDR block to an IP address range.
        -   Raw log entry:

            ```
            Ip: 192.168.1.0/24
            ```

        -   Transformation rule:

            ```
            e_set("strNormal",ip_to_format(v("ip"),3))
            ```

        -   Result:

            ```
            ip: 192.168.1.0/24
            strNormal: 192.168.1.0-192.168.1.255
            ```


## ip\_overlaps

This function determines whether two CIDR blocks overlap.

-   Syntax

    ```
    ip_overlaps("CIDR block 1","CIDR block 2",default="")
    ```

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |CIDR block 1|String|Yes|The first CIDR block.|
    |CIDR block 2|String|Yes|The second CIDR block.|
    |default|Arbitrary|No|If the function cannot determine whether the specified CIDR blocks overlap, the value of this parameter is returned. You can set the value of this parameter to an empty string.|

-   Response
    -   If the specified CIDR blocks do not overlap, 0 is returned.
    -   If the specified CIDR blocks overlap at the end of the blocks, 1 is returned.
    -   If the specified CIDR blocks overlap at the beginning of the blocks, -1 is returned.
-   Examples
    -   Example 1: The specified CIDR blocks do not overlap.
        -   Raw log entry:

            ```
            cidr1: 192.168.0.0/23
            cidr2: 192.168.2.0/24
            ```

        -   Transformation rule:

            ```
            e_set("overlaps",ip_overlaps(v("cidr1"),v("cidr2")))
            ```

        -   Result:

            ```
            idr1: 192.168.0.0/23
            cidr2: 192.168.2.0/24
            overlaps: 0
            ```

    -   Example 2: The specified CIDR blocks overlap at the beginning of the blocks.
        -   Raw log entry:

            ```
            cidr1: 192.168.1.0/24
            cidr2: 192.168.0.0/23
            ```

        -   Transformation rule:

            ```
            e_set("overlaps",ip_overlaps(v("cidr1"),v("cidr2")))
            ```

        -   Result:

            ```
            cidr1: 192.168.1.0/24
            cidr2: 192.168.0.0/23
            overlaps: -1
            ```

    -   Example 3: The specified CIDR blocks overlap at the end of the blocks.
        -   Raw log entry:

            ```
            cidr1: 192.168.0.0/23
            cidr2: 192.168.1.0/24
            ```

        -   Transformation rule:

            ```
            e_set("overlaps",ip_overlaps(v("cidr1"),v("cidr2")))
            ```

        -   Result:

            ```
            cidr1: 192.168.0.0/23
            cidr2: 192.168.1.0/24
            overlaps: 1
            ```


