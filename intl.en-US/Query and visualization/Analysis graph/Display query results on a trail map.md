# Display query results on a trail map

This topic describes how to configure a trail map to display query results.

## Background information

A trail map uses AMap to display the trail of a target over a period of time. Log Service provides two display modes for trail maps: longitude and latitude and points of interest \(POI\).

Each trail map consists of the following elements:

-   Map canvas
-   Points and lines
-   Time line

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project that you want to manage.

3.  Choose **Log Storage** \> **Logstores**, and then click the Logstore that you want to manage.

4.  Enter a query statement in the search box, specify a time range, and then click **Search & Analyze**.

5.  On the Graph tab, click the ![Trail chart - 003](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3208895951/p95428.png) icon.

6.  On the **Properties** tab, configure the properties of the trail map.

    |Parameter|Description|
    |---------|-----------|
    |Trail Type|The type of the trail map. Valid values: Longitude & Latitude and POI.|
    |Identity Column|The field that is selected to identify trail data.|
    |Information Column|The information to be displayed on the map. You can select one or more fields for this parameter. You can click a point on the trail map to view the information that is displayed on a pop-up window. |
    |Longitude|You must set this parameter if you select **Longitude & Latitude** for **Trail Type**. You must select the field that includes the longitude data.|
    |Latitude|You must set this parameter if you select **Longitude & Latitude** for **Trail Type**. You must select the field that includes the latitude data.|
    |POI Field|You must set this parameter if you select **POI** for **Trail Type**. You must select the field that includes the POI data.|


## Examples

To view the trails of specific confirmed COVID-19 cases in Hainan province during the epidemic period, execute the following query statement.

-   SQL statement

    ```
    type:trace | select location, __time__, id, action, text
    ```

-   Dataset

    |location|\_\_time\_\_|action|id|text|
    |--------|------------|------|--|----|
    |Village A, Longhua District|1582892272|Drive home|170|On the morning of February 24, confirmed case No.170 drove home with confirmed cases No.136 and No.143 from Haikou city center to Village A of Longhua District.|
    |Village A, Longhua District|1582892272|Stay home|170|Confirmed case No.170 stayed home from February 25 to February 27, but met confirmed case No.123.|
    |Village A, Longhua District|1582892272|Drive to Haikou|170|On the morning of February 28, confirmed case No.170 drove from Village A of Longhua District to Haikou and bought food at the wet market in Longqiao Town.|

-   Properties

    ![Properties](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3208895951/p95425.png)

-   Trail map

    ![Trail map](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3208895951/p95426.png)


