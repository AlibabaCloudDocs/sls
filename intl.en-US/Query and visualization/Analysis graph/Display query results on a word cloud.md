# Display query results on a word cloud

This topic describes how to configure a word cloud to display query results.

## Background information

A word cloud visualizes text data. It is a cloud-like and colored image composed of words. You can use a word cloud to display a large amount of text data. The font size or color of a word indicates the significance of the word. This allows you to recognize the most significant words in an efficient way.

The words in a word cloud are sorted.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project that you want to manage.

3.  Choose **Log Storage** \> **Logstores**, and then click the Logstore that you want to manage.

4.  Enter a query statement in the search box, specify a time range, and then click **Search & Analyze**.

5.  On the Graph tab, click the ![Word cloud - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7177895951/p93128.png) icon.

6.  On the **Properties** tab, configure the properties of the word cloud.

    |Parameter|Description|
    |:--------|:----------|
    |Word Column|The words to be displayed.|
    |Value Column|The numeric value that corresponds to a word.|
    |Font Size|The font size of the word.     -   The minimum font size ranges from 10 pixels to 24 pixels.
    -   The maximum font size ranges from 50 pixels to 80 pixels. |


## Examples

To query the distribution of hostnames in NGINX logs, execute the following query statement:

```
* | select hostname, count(1) as count group by hostname order by count desc limit 1000
```

Select `hostname` for Word Column and `count` for Value Column.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3650026061/p5749.png)

