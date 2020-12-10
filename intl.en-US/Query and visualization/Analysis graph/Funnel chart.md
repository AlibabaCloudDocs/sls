# Funnel chart

This topic describes how to configure a funnel chart to display query results.

## Background information

A funnel chart displays data of different business processes in an intuitive manner. This type of chart can be used to analyze standard, long-running, and multi-flow business processes. You can use a funnel chart to locate problems and make informative decisions. A trapezoid area in a funnel chart represents the difference of data volume between a specified business process and its previous process.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project that you want to manage.

3.  Choose **Log Storage** \> **Logstores**, and then click the Logstore that you want to manage.

4.  Enter a query statement in the search box, specify a time range, and then click **Search & Analyze**.

5.  On the Graph tab, click the ![Funnel chart - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7294588951/p94028.png) icon.

6.  On the **Properties** tab, set the properties of the chart.

    |Parameter|Description|
    |:--------|:----------|
    |Group by Column|The categorical field.|
    |Value Column|Select the field that contains numeric values. The larger the value, the higher the layer.|


## Example

Execute the following query statement to query the page views \(PVs\) of a document:

```
event.type:click and (event.target: register  or event.target: button_19 or event.target:button_18 or event.target: button_17  or event.target:  button_16) | select "event.target" as target ,  count(1) as click_count  group by target order by strpos('register,button_19,button_18,button_17,button_16'  , "event.target")
```

![Funnel chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7294588951/p127137.png)

