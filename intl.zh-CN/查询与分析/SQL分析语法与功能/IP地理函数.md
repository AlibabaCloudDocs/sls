# IP地理函数

IP地理函数可以识别一个IP地址是内网IP地址还是外网IP地址，也可以判断IP地址所属的国家、省份、城市。本文档介绍IP地理函数的基本语法及示例。

## 基本语法

|函数名|说明|样例|
|:--|:-|:-|
|ip\_to\_domain\(ip\)|判断某IP地址是内网地址还是外网地址。返回值为intranet或internet。|SELECT ip\_to\_domain\(ip\)|
|ip\_to\_country\(ip\)|判断某IP地址所属国家。|SELECT ip\_to\_country\(ip\)|
|ip\_to\_province\(ip\)|判断某IP地址所属省份。|SELECT ip\_to\_province\(ip\)|
|ip\_to\_city\(ip\)|判断某IP地址所属城市。|SELECT ip\_to\_city\(ip\)|
|ip\_to\_geo\(ip\)|判断某IP地址所在位置的经纬度。返回结果格式为`纬度,经度`。关于geohash函数的详细信息，请参见[地理函数](/intl.zh-CN/查询与分析/SQL分析语法与功能/地理函数.md)。

|SELECT ip\_to\_geo\(ip\)|
|ip\_to\_city\_geo\(ip\)|判断某IP地址所属城市的经纬度，返回结果格式为`纬度,经度`。此函数返回的是城市经纬度，每个城市只有一个经纬度。|SELECT ip\_to\_city\_geo\(ip\)|
|ip\_to\_provider\(ip\)|获取某IP地址对应的网络运营商。|SELECT ip\_to\_provider\(ip\)|
|ip\_to\_country\(ip,'en'\)|判断某IP地址所属国家，返回国家码。|SELECT ip\_to\_country\(ip,'en'\)|
|ip\_to\_country\_code\(ip\)|判断某IP地址所属国家，返回国家码。|SELECT ip\_to\_country\_code\(ip\)|
|ip\_to\_province\(ip,'en'\)|判断某IP地址所属省份，返回英文省名或者中文拼音。|SELECT ip\_to\_province\(ip,'en'\)|
|ip\_to\_city\(ip,'en'\)|判断某IP地址所属城市，返回英文城市名或者中文拼音。|SELECT ip\_to\_city\(ip,'en'\)|

## 示例

此处列举了IP地理函数在不同场景下的查询分析示例。您在执行查询分析语句后，还可以选择合适的统计图表展示查询分析结果。

-   统计不包含内网请求的请求总数，相关查询分析语句如下所示：

    ```
    * | SELECT count(1) where ip_to_domain(ip)!='intranet'
    ```

-   统计Top10的访问省份，相关查询分析语句如下所示：

    ```
    * | SELECT count(1) as pv, ip_to_province(ip) as province GROUP BY province order by pv desc limit 10
    ```

    如果上述结果中包含了内网请求，且您希望过滤这部分请求，可以使用如下查询分析语句：

    ```
    * | SELECT count(1) as pv, ip_to_province(ip) as province WHERE ip_to_domain(ip) != 'intranet'  GROUP BY province ORDER BY pv desc limit 10
    ```

-   统计不同国家的平均响应延时、最大响应延时以及最大延时对应的请求，相关查询分析语句如下所示：

    ```
    * | SELECT AVG(latency),MAX(latency),MAX_BY(requestId, latency) ,ip_to_country(ip) as country group by country limit 100
    ```

-   统计不同网络运营商的平均延时，相关查询分析语句如下所示：

    ```
    * | SELECT AVG(latency) , ip_to_provider(ip) as provider group by provider limit 100
    ```

-   统计IP地址的经纬度，确认请求客户端的分布情况，相关查询分析语句如下所示：

    ```
    * | SELECT count(1) as pv , ip_to_geo(ip) as geo group by geo order by pv desc
    ```


