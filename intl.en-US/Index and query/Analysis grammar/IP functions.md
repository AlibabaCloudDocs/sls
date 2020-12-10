# IP functions

IP functions can identify whether an IP address is an internal or external IP address. IP functions can also identify the country, province, and city to which an IP address belongs. This topic describes the syntax of IP functions and provides examples.

## Basic syntax

|Function|Description|Example|
|:-------|:----------|:------|
|ip\_to\_domain\(ip\)|Identifies whether an IP address is an internal or external IP address. The returned result is intranet or internet.|SELECT ip\_to\_domain\(ip\)|
|ip\_to\_country\(ip\)|Identifies the country to which an IP address belongs.|SELECT ip\_to\_country\(ip\)|
|ip\_to\_province\(ip\)|Identifies the province to which an IP address belongs.|SELECT ip\_to\_province\(ip\)|
|ip\_to\_city\(ip\)|Identifies the city to which an IP address belongs.|SELECT ip\_to\_city\(ip\)|
|ip\_to\_geo\(ip\)|Identifies the longitude and latitude of an IP address. The result is returned in the `latitude, longitude` format.For more information about geohash functions, see [Geo functions](/intl.en-US/Index and query/Analysis grammar/Geo functions.md).

|SELECT ip\_to\_geo\(ip\)|
|ip\_to\_city\_geo\(ip\)|Identifies the longitude and latitude of the city to which an IP address belongs. The result is returned in the `latitude, longitude` format. This function returns the longitude and latitude of a city. Each city has only one longitude and latitude coordinate.|SELECT ip\_to\_city\_geo\(ip\)|
|ip\_to\_provider\(ip\)|Identifies the Internet Service Provider \(ISP\) corresponding to an IP address.|SELECT ip\_to\_provider\(ip\)|
|ip\_to\_country\(ip,'en'\)|Identifies the country to which an IP address belongs. The result is returned as a country code.|SELECT ip\_to\_country\(ip,'en'\)|
|ip\_to\_country\_code\(ip\)|Identifies the country to which an IP address belongs. The result is returned as a country code.|SELECT ip\_to\_country\_code\(ip\)|
|ip\_to\_province\(ip,'en'\)|Identifies the province to which an IP address belongs. The result is returned in English or Chinese Pinyin.|SELECT ip\_to\_province\(ip,'en'\)|
|ip\_to\_city\(ip,'en'\)|Identifies the city to which an IP address belongs. The result is returned in English or Chinese Pinyin.|SELECT ip\_to\_city\(ip,'en'\)|

## Examples

The following examples show how to use IP functions in different scenarios. You can also display the query and analysis results on a chart.

-   To exclude the access requests over the internal network from the query results and view the total number of access requests, use the following query statement:

    ```
    * | SELECT count(1) where ip_to_domain(ip)! ='intranet'
    ```

-   To query the top 10 provinces from which access requests originate, use the following query and analysis statement:

    ```
    * | SELECT count(1) as pv, ip_to_province(ip) as province GROUP BY province order by pv desc limit 10
    ```

    If the preceding result contains access requests from the internal network and you want to filter out these requests, use the following query and analysis statement:

    ```
    * | SELECT count(1) as pv, ip_to_province(ip) as province WHERE ip_to_domain(ip) ! = 'intranet'  GROUP BY province ORDER BY pv desc limit 10
    ```

-   To view the average latency, maximum latency, and the request with the maximum latency by country, use the following query and analysis statement:

    ```
    * | SELECT AVG(latency),MAX(latency),MAX_BY(requestId, latency) ,ip_to_country(ip) as country group by country limit 100
    ```

-   To view the average latency of different ISPs, use the following query and analysis statement:

    ```
    * | SELECT AVG(latency) , ip_to_provider(ip) as provider group by provider limit 100
    ```

-   To view the longitude and latitude of an IP address, use the following query and analysis statement:

    ```
    * | SELECT count(1) as pv , ip_to_geo(ip) as geo group by geo order by pv desc
    ```


