# IP地理函数

IP地理函数可用于判断IP地址属于内网还是外网，也可用于分析IP地址所属的国家、州、城市。本文介绍IP地理函数的基本语法及示例。

## IP地址函数

**说明：** 如下函数中的KEY参数表示日志字段（例如client\_ip），其值为IP地址。

|函数名称|说明|示例|
|:---|:-|:-|
|ip\_to\_domain\(KEY\)|判断目标IP地址是内网地址还是外网地址。返回结果为**intranet**或**internet**。

-   **intranet**表示内网。
-   **internet**表示外网。

|```
* | SELECT ip_to_domain(client_ip)
``` |
|ip\_to\_country\(KEY\)|分析目标IP地址所属国家或地区。返回结果为国家或地区的中文名称。

|```
* | SELECT ip_to_country(client_ip)
``` |
|ip\_to\_country\(KEY,'en'\)|分析目标IP地址所属国家或地区。返回结果为国家或地区的代码。

|```
* | SELECT ip_to_country(client_ip,'en')
``` |
|ip\_to\_country\_code\(KEY\)|分析目标IP地址所属国家或地区。返回结果为国家或地区的代码。

|```
* | SELECT ip_to_country_code(client_ip)
``` |
|ip\_to\_province\(KEY\)|分析目标IP地址所属州。返回结果为州的中文名称。

|```
* | SELECT ip_to_province(client_ip)
``` |
|ip\_to\_province\(KEY,'en'\)|分析目标IP地址所属州。返回结果为州的行政区划代码。

|```
* | SELECT ip_to_province(client_ip,'en')
``` |
|ip\_to\_city\(KEY\)|分析目标IP地址所属城市。返回结果为城市的中文名称。

|```
* | SELECT ip_to_city(client_ip)
``` |
|ip\_to\_city\(KEY,'en'\)|分析目标IP地址所属城市。返回结果为城市的行政区划代码。

|```
* | SELECT ip_to_city(client_ip,'en')
``` |
|ip\_to\_geo\(KEY\)|分析目标IP地址所在位置的经纬度。返回结果格式为`纬度,经度`。

关于geohash函数的详细信息，请参见[地理函数](/intl.zh-CN/查询与分析/SQL分析语法与功能/地理函数.md)。

|```
* | SELECT ip_to_geo(client_ip)
``` |
|ip\_to\_city\_geo\(KEY\)|分析目标IP地址所属城市的经纬度。此函数返回的是城市经纬度，每个城市只有一个经纬度。返回结果格式为`纬度,经度`。

|```
* | SELECT ip_to_city_geo(client_ip)
``` |
|ip\_to\_provider\(KEY\)|分析目标IP地址对应的网络运营商。返回结果为网络运营商名称。

|```
* | SELECT ip_to_provider(client_ip)
``` |

## IP网段函数

**说明：** 如下函数中的KEY参数表示日志字段（例如client\_ip），其值为IP地址。其中ip\_subnet\_min、ip\_subnet\_max、ip\_subnet\_range、is\_subnet\_of和is\_prefix\_subnet\_of函数中的字段值为子网掩码格式的IP地址（例如192.168.1.1/24），如果字段值为通用的IP地址，则需使用其他函数（例如concat函数，concat\(key,'/24'\)）将其转换为子网掩码格式。

|函数名称|说明|示例|
|:---|:-|:-|
|ip\_prefix\(KEY,prefix\_bits\)|获取目标IP地址的前缀。返回结果为子网掩码格式的IP地址，例如192.168.1.0/24。

|```
* | SELECT ip_prefix(client_ip,24)
``` |
|ip\_subnet\_min\(KEY\)|获取IP网段中的最小IP地址。返回结果为IP地址，例如192.168.1.0。

|```
* | SELECT ip_subnet_min(concat(client_ip,'/24'))
``` |
|ip\_subnet\_max\(KEY\)|获取IP网段中最大IP地址。返回结果为IP地址，例如192.168.1.255。

|```
* | SELECT ip_subnet_max(concat(client_ip,'/24'))
``` |
|ip\_subnet\_range\(KEY\)|获取IP网段范围。返回结果为Array类型的IP地址，例如\["192.168.1.0","192.168.1.255"\]。

|```
* | SELECT ip_subnet_range(concat(client_ip,'/24'))
``` |
|is\_subnet\_of\('IP网段',KEY\)|判断目标IP地址是否在某网段内。返回结果为布尔值。

|```
* | SELECT is_subnet_of('192.168.0.1/24',client_ip)
``` |
|is\_prefix\_subnet\_of\('IP网段',KEY\)|判断目标网段是否为某网段的子网。返回结果为布尔值。

|```
* | SELECT is_prefix_subnet_of('192.168.0.1/24',concat(client_ip,'/24'))
``` |

## 示例

此处列举了IP地理函数在不同场景下的查询和分析示例。您在执行查询和分析操作后，还可以选择合适的统计图表展示查询和分析结果。

**说明：** 如下示例中的client\_ip、latency和requestId为日志字段。

-   统计不是来自内网的请求总数。

    ```
    * | SELECT count(*) AS PV where ip_to_domain(client_ip)!='intranet'
    ```

    ![不来自内网的请求](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8908921161/p230090.png)

-   统计请求总数Top10的州。

    ```
    * | SELECT count(*) as PV, ip_to_province(client_ip) AS province GROUP BY province ORDER BY PV desc LIMIT 10
    ```

    ![top10省份](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2938921161/p230092.png)

    如果上述结果中包含了内网请求，且您希望过滤这部分请求，可参考如下查询和分析语句。

    ```
    * | SELECT count(*) AS PV, ip_to_province(client_ip) AS province WHERE ip_to_domain(client_ip) != 'intranet'  GROUP BY province ORDER BY PV DESC LIMIT 10
    ```

-   统计不同国家（地区）的平均响应延时、最大响应延时以及最大延时对应的请求。

    ```
    * | SELECT AVG(latency) AS avg_latency, MAX(latency) AS max_latency, MAX_BY(requestId, latency) AS requestId, ip_to_country(client_ip) AS country GROUP BY country
    ```

    ![延时情况](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8908921161/p230160.png)

-   统计不同网络运营商的平均延时。

    ```
    * | SELECT AVG(latency) AS avg_latency, ip_to_provider(client_ip) AS provider GROUP BY provider ORDER BY avg_latency
    ```

    ![运营商延时](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8908921161/p230066.png)

-   统计IP地址的经纬度，确认客户端分布情况。

    ```
    * | SELECT count(*) AS PV , ip_to_geo(client_ip) AS geo GROUP BY geo ORDER BY PV DESC
    ```

    ![客户端分布](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8908921161/p230060.png)


