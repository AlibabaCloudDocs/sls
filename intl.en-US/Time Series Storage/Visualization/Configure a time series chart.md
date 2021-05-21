# Configure a time series chart

This topic describes how to configure a time series chart.

## Introduction

A time series chart visualizes the results of one or more queries on a Metricstore. The Metricstore stores metric data that is collected from Prometheus.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project that you want to manage.

3.  On the **Metricstore Storage** \> **Metricstore** tab, click a Metricstore name.

4.  On the **Properties** tab, set the properties of the time series chart.

    |Parameter|Description|
    |:--------|:----------|
    |Fill Missing Data|If you turn on the **Fill Missing Data** switch, Log Service automatically generates substitutes for missing samples in a time series.|
    |Y-axis Minimum Value|Specify the minimum value of the Y-axis.|
    |Y-axis Maximum Value|Specify the maximum value of the Y-axis.|
    |Format Left Y-axis|Select the format of the left Y-axis.|
    |X-axis Scale Density|Set the scale density of the X-axis. Valid values: 3 to 30.|
    |Line Type|Select the type of the lines in the time series chart. Valid values: Straight Line and Curve.|
    |Show Points|If you turn on the **Show Points** switch, sample values are displayed in the time series chart.|
    |Top Margin, Right Margin, Bottom Margin, and Left Margin|The distance between an axis and a border of the time series chart.|

5.  On the Query Statements tab, search and analyze the time series data.

    ![Time series chart of a Metricstore](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5039751261/p128663.png)


