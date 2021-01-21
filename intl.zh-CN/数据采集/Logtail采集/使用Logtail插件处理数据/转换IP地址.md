# 转换IP地址

您可以使用processor\_geoip插件将日志中的IP地址转换为地理位置（国家、省份、城市、经纬度）。本文介绍processor\_geoip插件的参数说明和配置示例。

## 参数说明

配置type为processor\_geoip，detail说明如下表所示。

**说明：**

-   Logtail安装包中没有GeoIP数据库，您需要手动下载GeoIP数据库到Logtail所在服务器并配置，建议下载精确到City粒度的数据库。更多信息，请参见[MaxMind GeoLite2](https://dev.maxmind.com/geoip/geoip2/geolite2/)。
-   请确保数据库格式为MMDB类型。

|参数|类型|是否必选|说明|
|:-|:-|:---|:-|
|SourceKey|String|是|待进行IP地址转换的原始字段名。|
|DBPath|String|是|GeoIP数据库的全路径。例如/user/data/GeoLite2-City\_20180102/GeoLite2-City.mmdb。|
|NoKeyError|Boolean|否|无匹配的原始字段名时是否报错。如果未添加该参数，则默认使用false，表示不报错。|
|NoMatchError|Boolean|否|IP地址无效或在数据库中未匹配到该IP地址时是否报错。如果未添加该参数，则默认使用false，表示不报错。|
|KeepSource|Boolean|否|是否保留原始字段。如果未添加该参数，则默认使用true，表示保留。|
|Language|String|否|语言属性。如果未添加该参数，则默认使用zh-CN。请确保您的GeoIP数据库中包含相应的语言。|

## 配置示例

将日志中的IP地址转换成对应的地理位置，示例如下：

-   原始日志

    ```
    "source_ip" : "**.**.**.**"
    ```

-   Logtail插件处理配置

    ```
    {
       "type": "processor_geoip",
        "detail": {
             "SourceKey": "ip",
             "NoKeyError": true,
             "NoMatchError": true,
             "KeepSource": true,
             "DBPath" : "/user/local/data/GeoLite2-City_20180102/GeoLite2-City.mmdb"
        }
    }
    ```

-   处理结果

    ```
    "source_ip_city_" : "**.**.**.**"
    "source_ip_province_" : "浙江省"
    "source_ip_city_" : "杭州"
    "source_ip_province_code_" : "ZJ"
    "source_ip_country_code_" : "CN"
    "source_ip_longitude_" : "120.********"
    "source_ip_latitude_" : "30.********"
    ```


