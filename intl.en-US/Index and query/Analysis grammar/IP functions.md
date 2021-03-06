# IP functions

IP functions can be used to identify whether an IP address is an internal or external IP address. IP functions can also be used to identify the country, state,and city to which an IP address belongs. This topic describes the syntax of IP functions and provides related examples.

## IP functions

**Note:** The KEY parameter in the following functions indicates a log field, for example, client\_ip. The value of this parameter is an IP address.

|Function|Description|Example|
|:-------|:----------|:------|
|ip\_to\_domain\(KEY\)|Identifies whether an IP address is an internal or external IP address. The returned result is **intranet** or **internet**.

-   **intranet**: indicates an internal IP address.
-   **internet**: indicates an external IP address.

|```
* | SELECT ip_to_domain(client_ip)
``` |
|ip\_to\_country\(KEY\)|Identifies the country to which an IP address belongs. The returned result is the Chinese characters of a country or region.

|```
* | SELECT ip_to_country(client_ip)
``` |
|ip\_to\_country\(KEY,'en'\)|Identifies the country or region to which an IP address belongs. The returned result is the code of a country or region.

|```
* | SELECT ip_to_country(client_ip,'en')
``` |
|ip\_to\_country\_code\(KEY\)|Identifies the country or region to which an IP address belongs. The returned result is the code of a country or region.

|```
* | SELECT ip_to_country_code(client_ip)
``` |
|ip\_to\_province\(KEY\)|Identifies the state to which an IP address belongs. The returned result is the Chinese characters of a state.

|```
* | SELECT ip_to_province(client_ip)
``` |
|ip\_to\_province\(KEY,'en'\)|Identifies the state to which an IP address belongs. The returned result is the administrative region code of a state.

|```
* | SELECT ip_to_province(client_ip,'en')
``` |
|ip\_to\_city\(KEY\)|Identifies the city to which an IP address belongs. The returned result is the Chinese characters of a city.

|```
* | SELECT ip_to_city(client_ip)
``` |
|ip\_to\_city\(KEY,'en'\)|Identifies the city to which an IP address belongs. The returned result is the administrative region code of a city.

|```
* | SELECT ip_to_city(client_ip,'en')
``` |
|ip\_to\_geo\(KEY\)|Identifies the longitude and latitude of an IP address. The returned result is in the `latitude,longitude` format.

For information about geohash functions, see [Geo functions](/intl.en-US/Index and query/Analysis grammar/Geo functions.md).

|```
* | SELECT ip_to_geo(client_ip)
``` |
|ip\_to\_city\_geo\(KEY\)|Identifies the longitude and latitude of the city to which an IP address belongs. This function returns the longitude and latitude of a city. Each city has only one longitude and latitude coordinate. The returned result is in the `latitude,longitude` format.

|```
* | SELECT ip_to_city_geo(client_ip)
``` |
|ip\_to\_provider\(KEY\)|Identifies the Internet service provider \(ISP\) of an IP address. The returned result is the name of an ISP.

|```
* | SELECT ip_to_provider(client_ip)
``` |

## CIDR block functions

**Note:** The KEY parameter in the following functions indicates a log field, for example, client\_ip. The value of this parameter is an IP address. The field values in the ip\_subnet\_min, ip\_subnet\_max, ip\_subnet\_range, is\_subnet\_of, and is\_prefix\_subnet\_of functions are IP addresses in the subnet mask format, for example, 192.168.1.1/24. If the field values are common IP addresses, you can use the concat function \(concat\(key,'/24'\)\) to convert the IP addresses into the subnet mask format.

|Function|Description|Example|
|:-------|:----------|:------|
|ip\_prefix\(KEY,prefix\_bits\)|Obtains the prefix of an IP address. The returned result is an IP address in the subnet mask format, for example, 192.168.1.0/24.

|```
* | SELECT ip_prefix(client_ip,24)
``` |
|ip\_subnet\_min\(KEY\)|Obtains the smallest IP address within a CIDR block. The returned result is an IP address, for example, 192.168.1.0.

|```
* | SELECT ip_subnet_min(concat(client_ip,'/24'))
``` |
|ip\_subnet\_max\(KEY\)|Obtains the largest IP address within a CIDR block. The returned result is an IP address, for example, 192.168.1.255.

|```
* | SELECT ip_subnet_max(concat(client_ip,'/24'))
``` |
|ip\_subnet\_range\(KEY\)|Obtains a CIDR block. The returned result is an Array of IP addresses, for example, \["192.168.1.0","192.168.1.255"\].

|```
* | SELECT ip_subnet_range(concat(client_ip,'/24'))
``` |
|is\_subnet\_of\('CIDR block', KEY\)|Identifies whether an IP address is within a specified CIDR block. The returned result is a Boolean value.

|```
* | SELECT is_subnet_of('192.168.0.1/24',client_ip)
``` |
|is\_prefix\_subnet\_of\(''CIDR block', KEY\)|Identifies whether a CIDR block is a subnet of a specified CIDR block. The returned result is a Boolean value.

|```
* | SELECT is_prefix_subnet_of('192.168.0.1/24',concat(client_ip,'/24'))
``` |

## Examples

The following examples show how to use IP functions in different scenarios. You can also visualize the query and analysis results on a chart.

**Note:** In the following examples, client\_ip, latency, and requestId are log fields.

-   To calculate the total number of requests that are not from the internal network, execute the following query statement:

    ```
    * | SELECT count(*) AS PV where ip_to_domain(client_ip)!='intranet'
    ```

    ![Requests that are not from the internal network](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0818537161/p230090.png)

-   To calculate the top 10 states from which the most requests are sent, execute the following query statement:

    ```
    * | SELECT count(*) as PV, ip_to_province(client_ip) AS province GROUP BY province ORDER BY PV desc LIMIT 10
    ```

    ![Top 10 provinces](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0818537161/p230092.png)

    If the result of the preceding query statement contains requests from the internal network and you want to exclude these requests, execute the following query statement:

    ```
    * | SELECT count(*) AS PV, ip_to_province(client_ip) AS province WHERE ip_to_domain(client_ip) != 'intranet'  GROUP BY province ORDER BY PV DESC LIMIT 10
    ```

-   To calculate the average response latency, maximum response latency, and the requests associated with the maximum latency by country \(region\), execute the following query statement:

    ```
    * | SELECT AVG(latency) AS avg_latency, MAX(latency) AS max_latency, MAX_BY(requestId, latency) AS requestId, ip_to_country(client_ip) AS country GROUP BY country
    ```

    ![Latency](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1818537161/p230160.png)

-   To calculate the average latency of different ISPs, execute the following query statement:

    ```
    * | SELECT AVG(latency) AS avg_latency, ip_to_provider(client_ip) AS provider GROUP BY provider ORDER BY avg_latency
    ```

    ![ISP latency](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2528537161/p230066.png)

-   To query the longitude and latitude to which an IP address belongs and view the distribution of clients, execute the following query statement:

    ```
    * | SELECT count(*) AS PV , ip_to_geo(client_ip) AS geo GROUP BY geo ORDER BY PV DESC
    ```

    ![Client distribution](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1818537161/p230060.png)


