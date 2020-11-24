# Display query results on a map

This topic describes how to configure a map to display query results.

## Background information

You can color and mark a map to display geographic data. Log Service provides three types of maps: map of China, world map, and AMap. The display modes of an AMap include the anchor point and heat map. You can include specific functions in query statements to display analysis results as maps.

Each map consists of the following elements:

-   Map canvas
-   Colored area

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project that you want to manage.

3.  Choose **Log Storage** \> **Logstores**, and then click the Logstore that you want to manage.

4.  Enter a query statement in the search box, specify a time range, and then click **Search & Analyze**.

    -   To display query results on a map of China, include the ip\_to\_province function in a query statement.
    -   To display query results on a world map, include the ip\_to\_country function in a query statement.
    -   To display query results on an AMap, include the ip\_to\_geo function in a query statement.
5.  On the Graph tab, click the ![Map - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4357895951/p93123.png) icon.

    In this example, the map of China is selected.

6.  On the **Properties** tab, configure the properties of the map of China.

    |Parameter|Description|
    |:--------|:----------|
    |Location information|The location information that is recorded in logs. The information is displayed in one of the following dimensions based on the map type:     -   Provinces \(Map of China\)
    -   Country \(World Map\)
    -   Longitude/Latitude \(AMap\) |
    |Value Column|The data volume of the location information.|
    |Show Legend|If you turn on the Show Legend switch, the legend information is displayed.|


## Example of a map of China

To display query results on a map of China, you can execute the following query statement that includes the ip\_to\_province function:

```
* | select  ip_to_province(remote_addr) as address, count(1) as count group by address order by count desc limit 10
```

Select address for Provinces and count for Value Column.

![1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0652222061/p129975.png)

## Example of a world map

To display query results in a world map, you can execute the following query statement that includes the ip\_to\_country function:

```
* | select  ip_to_country(remote_addr) as address, count(1) as count group by address order by count desc limit 10
```

Select address for Country and count for Value Column.

![2](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1652222061/p129976.png)

## Example of an AMap

To display query results on an AMap, you can execute the following query statement that includes the ip\_to\_geo function. The address column in the dataset contains the latitude and longitude values, which are separated by a comma \(,\). If the longitude and latitude values are contained in two separate columns named lng and lat, you can use the `concat('lat', ',', lng')` function to combine the two columns into one column.

```
* | select  ip_to_geo(remote_addr) as address, count(1) as count group by address order by count desc limit 10
```

By default, the display mode of the anchor points is returned. If the anchor points are densely distributed on the map, you can select the display mode of heat map.

