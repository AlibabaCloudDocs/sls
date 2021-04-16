# Structured data functions

This topic describes the syntax and parameters of functions that parse JSON-formatted, XML-formatted, and Protobuf-formatted data and functions that process IP addresses. This topic also provides some examples.

## Functions

|Type|Function|Description|
|----|--------|-----------|
|JSON|[json\_select](#section_xrt_z1k_awb)|Extracts or calculates a specific value from a JSON expression based on the JMESPath syntax.|
|[json\_parse](#section_n3w_bun_us1)|Converts a specified value to a JSON object.|
|XML|[xml\_to\_json](#section_inr_nya_rp6)|Converts XML-formatted data to JSON-formatted data.|
|Gzip|[gzip\_compress](#section_52q_xji_m92)|Compresses data and then encodes the data by using the Base64 algorithm. The returned data includes the raw data and the encoded data.|
|[gzip\_decompress](#section_thp_iir_o9q)|Decodes data by using the Base64 algorithm and then decompresses the data. The returned data includes the raw data and the decoded data.|
|IP|[geo\_parse](#section_2mh_vv3_9w5)|Identifies the city, province, and country based on an IP address.|
|[ip\_cidrmatch](#section_2bd_uyj_rho)|Checks whether an IP address belongs to a Classless Inter-Domain Routing \(CIDR\) block.|
|[ip\_version](#section_xbo_zvp_eg8)|Checks whether the version of an IP address is IPv4 or IPv6.|
|[ip\_type](#section_bhb_m61_hwh)|Identifies the type of an IP address.|
|[ip\_makenet](#section_r64_xsl_h67)|Converts an IP address to a CIDR block.|
|[ip\_to\_format](#section_e4e_d96_1m3)|Converts the format of a CIDR block to a format that specifies the netmask or prefix length of the CIDR block.|
|[ip\_overlaps](#section_ny8_rdb_ouw)|Checks whether two CIDR blocks overlap.|

## json\_select

The json\_select function is used to extract or calculate a specific value from a JSON expression based on the JMESPath syntax.

-   Syntax

    ```
    json_select(value, jmes, default=None, restrict=False)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|Arbitrary|Yes|The JSON expression or field from which a field is extracted.|
    |jmes|String|Yes|The JMESPath expression that specifies the field to be extracted.|
    |default|Arbitrary|No|If the field to be extracted does not exist, the value of the default parameter is returned. Default value: None.|
    |restrict|Bool|No|Specifies whether the restricted mode is enabled if the value of the field to be extracted is in the invalid JSON format. Default value: False. Valid values:     -   False: The invalid format is ignored and the data continues to be transformed. The value of the default parameter is returned.
    -   True: The invalid format is reported and the data stops being transformed. The log entry is dropped. |

-   Response

    The extracted field value is returned.

-   Examples
    -   Example 1: Extract the name field from the content field and the value of the name field is returned.
        -   Raw log entry:

            ```
            content:  {"name": "xiaoming", "age": 10}
            ```

        -   Transformation rule:

            ```
            e_set("json_filter",json_select(v("content"), "name"))
            ```

        -   Result:

            ```
            content:  {"name": "xiaoming", "age": 10}
            json_filter:  xiaoming
            ```

    -   Example 2: Extract the name field from the content field and all values in the name field are returned.
        -   Raw log entry:

            ```
            content:  {"name": ["xiaoming", "xiaowang", "xiaoli"], "age": 10}
            ```

        -   Transformation rule:

            ```
            e_set("json_filter",json_select(v("content"), "name[*]"))
            ```

        -   Result:

            ```
            content:  {"name": ["xiaoming", "xiaowang", "xiaoli"], "age": 10}
            json_filter:  ["xiaoming", "xiaowang", "xiaoli"]
            ```

    -   Example 3: Extract the value of the name3 field from the content field. If the name3 field does not exist, None is returned.
        -   Raw log entry:

            ```
            content:  {"name": "xiaoming", "age": 10}
            ```

        -   Transformation rule:

            ```
            e_set("json_filter",json_select(v("content"), "name3", default="None"))
            ```

        -   Result:

            ```
            content:  {"name": "xiaoming", "age": 10}
            json_filter: None
            ```


## json\_parse

The json\_parse function is used to convert a specified value to a JSON object.

-   Syntax

    ```
    json_parse(value, default=None, restrict=False)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |value|String|Yes|The field to be parsed.|
    |default|Arbitrary|No|If the field to be parsed does not exist, the value of the default parameter is returned. Default value: None.|
    |restrict|Bool|No|Specifies whether the restricted mode is enabled if the value of the field to be parsed is in the invalid JSON format. Default value: False. Valid values:     -   False: The invalid format is ignored and the data continues to be transformed. The value of the default parameter is returned.
    -   True: The invalid format is reported and the data stops being transformed. The log entry is dropped. |

-   Response

    A JSON object is returned.

-   Example
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

The xml\_to\_json function is used to convert XML-formatted data to JSON-formatted data.

-   Syntax

    ```
    xml_to_json(source)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |source|String|Yes|The field to be converted.|

-   Response

    JSON-formatted data is returned.

-   Example
    -   Raw log entry:

        ```
        str : <?xmlversion="1.0"?><data><countryname="Liechtenstein"><rank>1</rank><year>2008</year><gdppc>141100</gdppc><neighborname="Austria"direction="E"/><neighborname="Switzerland"direction="W"/></country><countryname="Singapore"><rank>4</rank><year>2011</year><gdppc>59900</gdppc><neighborname="Malaysia"direction="N"/></country><countryname="Panama"><rank>68</rank><year>2011</year><gdppc>13600</gdppc><neighborname="Costa Rica"direction="W"/><neighborname="Colombia"direction="E"/></country></data>
        ```

    -   Transformation rule:

        ```
        e_set("str_json",xml_to_json(v("str")))
        ```

    -   Result:

        ```
        str : <?xmlversion="1.0"?><data><countryname="Liechtenstein"><rank>1</rank><year>2008</year><gdppc>141100</gdppc><neighborname="Austria"direction="E"/><neighborname="Switzerland"direction="W"/></country><countryname="Singapore"><rank>4</rank><year>2011</year><gdppc>59900</gdppc><neighborname="Malaysia"direction="N"/></country><countryname="Panama"><rank>68</rank><year>2011</year><gdppc>13600</gdppc><neighborname="Costa Rica"direction="W"/><neighborname="Colombia"direction="E"/></country></data>
        str_json : {"data": {"country": [{"@name": "Liechtenstein", "rank": "1", "year": "2008", "gdppc": "141100", "neighbor": [{"@name": "Austria", "@direction": "E"}, {"@name": "Switzerland", "@direction": "W"}]}, {"@name": "Singapore", "rank": "4", "year": "2011", "gdppc": "59900", "neighbor": {"@name": "Malaysia", "@direction": "N"}}, {"@name": "Panama", "rank": "68", "year": "2011", "gdppc": "13600", "neighbor": [{"@name": "Costa Rica", "@direction": "W"}, {"@name": "Colombia", "@direction": "E"}]}]}}
        ```


## gzip\_compress

The gzip\_compress function is used to compress data and then encode the data by using the Base64 algorithm. The returned data includes the raw data and the encoded data.

-   Syntax

    ```
    gzip_compress(data, compresslevel=6, to_format="base64", encoding="utf-8")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |data|Arbitrary|Yes|The data to be compressed.|
    |compresslevel|Int|No|The compression level. Valid values: 0 to 9. Default value: 6.     -   1: Data is compressed at the highest speed but with the lowest compression ratio.
    -   9: Data is compressed at the lowest speed but with the highest compression ratio.
    -   0: Data is not compressed. |
    |to\_format|String|No|Encodes the compressed data by using the Base64 algorithm. Only the Base64 algorithm is supported.|
    |encoding|String|No|The encoding format. Default value: utf-8. For more information about other encoding formats, see [Standard encoding formats](/intl.en-US/Data Transformation/Data processing syntax/General reference/Standard encoding formats.md).|

-   Response

    Compressed and Base64-encoded data is returned.

-   Example
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

The gzip\_decompress function is used to decode data by using the Base64 algorithm and then decompress the data. The returned data includes the raw data and the decoded data.

-   Syntax

    ```
    gzip_decompress(data, from_format="base64", encoding="utf-8")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |data|Arbitrary|Yes|The data to be decompressed.|
    |from\_format|String|No|Decodes the compressed data by using the Base64 algorithm. Only the Base64 algorithm is supported.|
    |encoding|String|No|The encoding format. Default value: utf-8. For information about other encoding formats, see [Standard encoding formats](/intl.en-US/Data Transformation/Data processing syntax/General reference/Standard encoding formats.md).|

-   Response

    Decompressed and Base64-decoded data is returned.

-   Syntax
    -   Raw log entry:

        ```
        content: H4sIAA8JXl4C/xXK0QmAMAwFwFXeBO7RMQKNREx5kAZDtle/7wbES3rDyRsnoyQmklgNo1/ztzJN08BAhjzqYGCnNCS/tPR4AcgrnWVGAAAA
        ```

    -   Transformation rule:

        ```
        e_set("gzip_decompress",gzip_decompress(v("content"),from_fmat="base64"))
        ```

    -   Result:

        ```
        content: H4sIAA8JXl4C/xXK0QmAMAwFwFXeBO7RMQKNREx5kAZDtle/7wbES3rDyRsnoyQmklgNo1/ztzJN08BAhjzqYGCnNCS/tPR4AcgrnWVGAAAA
        gzip_decompress: I always look forward to my holidays whether I travel or stay at home.
        ```


## geo\_parse

The geo\_parse function is used to identify the city, province, and country based on an IP address.

-   Syntax

    ```
    geo_parse(ip, ip_db="SLS-GeoIP", keep_fields=None, provider="ipip", ip_sep=None)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |ip|IP address string|Yes|The IP address to be parsed into the city, province, and country information. If multiple IP addresses are included in a string, you can specify the ip\_sep parameter to split the string. After these IP addresses are parsed, the result is returned in the JSON format.|
    |ip\_db|IP address database|Yes|The IP address database that is used to parse an IP address into the country, province, and city information. Valid values:     -   SLS-GeoIP \(default value\): The built-in IP address database of Log Service.
    -   Custom IP address database. The `res_oss_file(endpoint, ak_id, ak_key, bucket, file, format='text', change_detect_interval=0,fetch_interval=2,refresh_retry_max=60,encoding='utf8',error='ignore')` function is used to access the IP address database. The format parameter is set to binary in the `format='binary'` format. For more information about the parameters in the res\_oss\_file function, see [res\_oss\_file](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.md). |
    |keep\_fields|Tuple|No|The keys that are contained in the returned result.     -   If you use the built-in IP address database, the following keys are contained in the returned result by default:
        -   city: the name of the city
        -   province: the name of the province
        -   country: the name of the country
        -   city\_en: the administrative region code or name of the city
        -   province\_en: the administrative region code or name of the province
        -   country\_en: the code or name of the country or region
        -   isp: the name of the Internet service provider \(ISP\)
        -   lat: the latitude of the location to which the IP address belongs
        -   lon: the longitude of the location which the IP address belongs
    -   If you use a custom IP address database, the following keys are contained in the returned result by default:
        -   city: the name of the city
        -   province: the name of the province
        -   country: the name of the country
For example, `keep_fields=("city","country")` indicates that the `city` and `country` keys are returned.

The `keep_fields` parameter can also be used to rename the keys. For example, `(("city","cty"),("country","state"))` indicates that the city and country keys are renamed `cty` and `state` in the returned result. |
    |provider|String|No|This parameter is valid only when the ip\_db parameter is set to Custom IP address database. Valid values:     -   ipip \(default value\): The binary IP address database provided by IPIP in the IPDB format is used to parse IP addresses. To download the database, visit [ipip](https://en.ipip.net/?origin=CN).
    -   ip2location: The global binary IP address database provided by IP2Location is used to parse IP addresses. To download the database, visit [ip2location](https://lite.ip2location.com/database/ip-country-region-city). Only a binary IP address database is supported. |
    |ip\_sep|String|No|The IP address delimiter. The delimiter is used to delimit a string of IP addresses into multiple IP addresses. The parsing result is returned in the JSON format. The default value is None. This value indicates that IP addresses are not delimited.|

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
    -   Example 1: Query data by using the SLS built-in IP address database.
        -   Raw log entry:

            ```
            ip : 203.0.113.1
            ```

        -   Transformation rule:

            ```
            e_set("geo", geo_parse(v("ip"))
            ```

        -   Result:

            ```
            ip : 203.0.113.1
            geo: {"city":"Hangzhou","province":"Zhejiang province","country":"China","isp":"China Mobile","lat":30.16,"lon":120.12}
            ```

    -   Example 2: Use the SLS built-in IP address database to query data and parse a log field that contains multiple IP addresses. The information of the country, province, and city to which each IP address belongs is returned.
        -   Raw log entry:

            ```
            ip : 203.0.113.4, 192.0.2.2, 198.51.100.2
            ```

        -   Transformation rule:

            ```
            e_set("geo", geo_parse(v("ip"), ip_sep=","))
            ```

        -   Result:

            ```
            ip : 203.0.113.4, 192.0.2.2, 198.51.100.2
            geo : {"203.0.113.4": {"country_en": "CN", "province_en": "330000", "city_en": "330200", "country": "China", "province": "Zhejiang province", "city": "Ningbo", "isp": "China Telecom", "lat": 29.8782, "lon": 121.549}, "192.0.2.2": {"country_en": "CN", "province_en": "320000", "city_en": "321300", "country": "China", "province": "Jiangsu province", "city": "Suqian", "isp": "China Telecom", "lat": 33.9492, "lon": 118.296}, "198.51.100.2": {"country_en": "CN", "province_en": "330000", "city_en": "330500", "country": "China", "province": "Zhejiang province", "city": "Huzhou", "isp": "China Telecom", "lat": 30.8703, "lon": 120.093}}
            ```

    -   Example 3: Query data by using a custom IP address database.
        -   Raw log entry:

            ```
            ip : 203.0.113.1
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
            ip : 203.0.113.1
            geo : {"city": "Hangzhou", "province":"Zhejiang province","country": "China"}
            ```

    -   Example 4: Use a custom IP address database to query data. The specified keys are returned and renamed.
        -   Raw log entry:

            ```
            ip : 203.0.113.1
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
            ip : 203.0.113.1
            geo : { "state": "China","pro": "Zhejiang province","cty": Hangzhou"}
            ```

    -   Example 5: Use a custom IP address database to query data. The specified keys are returned.
        -   Raw log entry:

            ```
            ip : 203.0.113.1
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
            ip : 203.0.113.1
            geo : { "country": "China","province": "Zhejiang province"}
            ```

    -   Example 6: Use a custom IP address database to query data. Then, use the global binary IP address database provided by IP2Location to parse the data. The related keys are returned.

        -   Raw log entry:

            ```
            ip : 203.0.113.2
            ```

        -   Transformation rule:

            ```
            e_set("geo", geo_parse(v("ip"), ip_db=res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',ak_id="your ak_id", ak_key="your ak_serect", bucket='log-etl-staging', file='your ip2location bin file', format='binary', change_detect_interval=20),provider="ip2location"))
            ```

        -   Result:

            ```
            ip : 203.0.113.2
            geo : {"city":"Dearborn","province":"Michigan","country":"United States"}
            ```

        If you set the value of the provider parameter to ip2location, the open source SDK for Python provided by IP2Location is used to transform input IP addresses. For more information, visit [ip2location python sdk](https://www.ip2location.com/development-libraries/ip2location/python). The SDK for Python provided by IP2Location can be used to parse the following fields. If a field cannot be parsed, check whether the field is included in the IP address database provided by IP2Location.

        ```
        country_short
        country_long / The country field is specified for data transformation.
        region / The province field is specified for data transformation.
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

    -   Example 7: Use a custom IP address database to query data and parse a log field that contains multiple IP addresses. The information of the country, province, and city to which each IP address belongs is returned.
        -   Raw log entry:

            ```
            ip : 203.0.113.3, 192.0.2.1, 198.51.100.1
            ```

        -   Transformation rule:

            ```
            e_set("geo", geo_parse(v("ip"), ip_db=res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',
                                                                           ak_id="ak_id",
                                                                           ak_key="ak_serect",
                                                                           bucket='log-etl-staging',
                                                                           file='calendar.csv/IP2LOCATION-LITE-DB3.BIN',
                                                                           format='binary', change_detect_interval=20),
                                            provider="ip2location", ip_sep=","))
            ```

        -   Result:

            ```
            ip : 203.0.113.3, 192.0.2.1, 198.51.100.1
            geo : {"203.0.113.3": {"city": "Dearborn", "province": "Michigan", "country": "United States"}, "192.0.2.1": {"city": "Hangzhou", "province": "Zhejiang", "country": "China"}, "198.51.100.1": {"city": "Hangzhou", "province": "Zhejiang", "country": "China"}}
            ```


## ip\_cidrmatch

The ip\_cidrmatch function is used to return a Boolean value based on whether an IP address matches a specified CIDR subnet. This function checks whether an IPv4 address or IPv6 address belongs to a CIDR block. If the specified IP address belongs to the specified CIDR block, true is returned. Otherwise, false is returned. IPv4 and IPv6 addresses are supported.

-   Syntax

    ```
    ip_cidrmatch(cidr\_subnet, ip, default="")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |cidr\_subnet|String|Yes|The CIDR block, for example: 203.0.113.1/25.|
    |ip|String|Yes|The IP address.|
    |default|Arbitrary|No|If the specified IP address does not match the specified CIDR block, the value of this parameter is returned. You can set the value of this parameter to an empty string.|

-   Response

    If the specified IP address matches the specified CIDR block, true is returned. Otherwise, false is returned.

-   Examples
    -   Example 1: The specified IPv4 address matches the specified CIDR block, and true is returned.
        -   Raw log entry:

            ```
            cidr_subnet: 192.168.1.0/24
            ip: 192.168.1.100
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

    -   Example 2: The specified IPv4 address does not match the specified CIDR block, and false is returned.
        -   Raw log entry:

            ```
            cidr_subnet: 192.168.1.0/24
            ip: 10.10.1.100
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

    -   Example 3: The specified IP address cannot match the specified CIDR block, and unkown is returned.
        -   Raw log entry:

            ```
            cidr_subnet: -11
            ip: 10.10.1.100
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

The ip\_version function is used to check whether the version of an IP address is IPv4 or IPv6. If an IP address is an IPv4 address, IPv4 is returned. If an IP address is an IPv6 address, IPv6 is returned.

-   Syntax

    ```
    ip_version(ip, default="")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |ip|String|Yes|The IP address.|
    |default|Arbitrary|No|If the version of the specified IP address cannot be identified, the value of this parameter is returned. You can set the value of this parameter to an empty string.|

-   Response

    IPv6 or IPv4 is returned.

-   Examples
    -   Example 1: The specified IP address is an IPv4 address, and IPv4 is returned.
        -   Raw log entry:

            ```
            ip: 192.168.1.100
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

    -   Example 2: The specified IP address is an IPv6 address, and IPv6 is returned.
        -   Raw log entry:

            ```
            ip: ::1
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

The ip\_type function is used to identify the type of an IP address. A value that indicates the type of an IP address is returned, for example, private, reserved, loopback, public, or allocated ripe ncc.

-   Syntax

    ```
    ip_type(ip, default="")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |ip|String|Yes|The IP address.|
    |default|Arbitrary|No|If the type of the specified IP address cannot be identified, the value of this parameter is returned. You can set the value of this parameter to an empty string.|

-   Response

    A value that indicates the type of an IP address is returned, for example, private, reserved, loopback, public, or allocated ripe ncc.

-   Examples
    -   Example 1: Identify the type of an IP address, and loopback is returned.
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

    -   Example 2: Identify the type of an IP address, and private is returned.
        -   Raw log entry:

            ```
            ip: 192.168.1.1
            ```

        -   Transformation rule:

            ```
            e_set("type",ip_type(v("ip")))
            ```

        -   Result:

            ```
            ip: 192.168.1.1
            type: private
            ```

    -   Example 3: Identify the type of an IP address, and public is returned.
        -   Raw log entry:

            ```
            ip: 195.185.1.2
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

    -   Example 1: Identify the type of an IPv6 address, and loopback is returned.
        -   Raw log entry:

            ```
            ip: ::1
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

    -   Example 5: Identify the type of an IPv6 address, and allocated ripe ncc is returned.
        -   Raw log entry:

            ```
            ip: 2001:0658:022a:cafe:0200::1
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

The ip\_makenet function is used to convert an IP address to a CIDR block.

-   Syntax

    ```
    ip_makenet(ip, subnet_mask=None, default="")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |ip|String|Yes|The IP address.|
    |subnet\_mask|String|Yes|The subnet mask, for example, 255.255.255.0. **Note:** If you set the ip parameter to a CIDR block, you can set the subnet\_mask parameter to an empty string. |
    |default|Arbitrary|No|If the specified IP address cannot be converted to a CIDR block, the value of this parameter is returned. You can set the value of this parameter to an empty string.|

-   Response

    A CIDR block is returned.

-   Examples
    -   Example 1: Convert an IP address to a CIDR block.
        -   Raw log entry:

            ```
            ip: 192.168.1.0
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
            ip: 192.168.1.0-192.168.1.255
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

The ip\_to\_format function is used to convert the format of a CIDR block to a format that specifies the netmask or prefix length of the CIDR block.

-   Syntax

    ```
    ip_to_format(cidr\_subnet, want_prefix_len=0, default="")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |cidr\_subnet|String|Yes|The CIDR block, for example, 192.168.1.0/24.|
    |want\_prefix\_len|Int|No|The format of the returned CIDR block. Default value: 0. Valid values:     -   0: returns the original CIDR block.
    -   1: returns a CIDR block that specifies the prefix length of the CIDR block.
    -   2: returns a CIDR block that specifies the netmask of the CIDR block.
    -   3: returns an IP address range. |
    |default|Arbitrary|No|If the format of the specified CIDR block cannot be converted, the value of this parameter is returned. You can set the value of this parameter to an empty string.|

-   Response

    A CIDR block of the specified format is returned.

-   Examples
    -   Example 1: The format of a CIDR block is not converted.
        -   Raw log entry:

            ```
            ip: 192.168.1.0/24
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

    -   Example 2: Convert the format of a CIDR block to the format that specifies the prefix length of the CIDR block.
        -   Raw log entry:

            ```
             ip: 192.168.1.0/24
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

    -   Example 3: Convert the format of a CIDR block to the format that specifies the netmask of the CIDR block.
        -   Raw log entry:

            ```
            ip: 192.168.1.0/24
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
            ip: 192.168.1.0/24
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

Checks whether two CIDR blocks overlap.

-   Syntax

    ```
    ip_overlaps(cidr\_subnet, cidr\_subnet2, default="")
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |cidr\_subnet|String|Yes|The first CIDR block.|
    |cidr\_subnet2|String|Yes|The second CIDR block.|
    |default|Arbitrary|No|If the function cannot check whether the specified CIDR blocks overlap, the value of this parameter is returned. You can set the value of this parameter to an empty string.|

-   Response
    -   If the specified CIDR blocks do not overlap, 0 is returned.
    -   If the specified CIDR blocks overlap at the end of the blocks, 1 is returned.
    -   If the specified CIDR blocks overlap at the start of the blocks, -1 is returned.
-   Examples
    -   Example 1: The specified two CIDR blocks do not overlap.
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
            cidr1: 192.168.0.0/23
            cidr2: 192.168.2.0/24
            overlaps: 0
            ```

    -   Example 2: The specified two CIDR blocks overlap at the start of the blocks.
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

    -   Example 3: The specified two CIDR blocks overlap at the end of the blocks.
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


