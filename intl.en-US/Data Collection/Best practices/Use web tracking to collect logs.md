# Use web tracking to collect logs

This topic describes how to collect log data to Log Service by using the web tracking feature, and how to query and analyze the collected log data.

When you send an important email, you can set the **read receipt** tag in the email. Then, you can receive a receipt when the recipient reads the email. The read receipt mode is widely used in the following scenarios:

-   Check whether a recipient has read a leaflet after the leaflet is sent to the recipient.
-   Check how many users have clicked a promotional web page.
-   Analyze page views \(PVs\) of a marketing page on a mobile app.

Traditional solutions are developed for websites and webmasters. These solutions do not apply to data collection or analysis in the preceding scenarios due to the following reasons:

-   Traditional solutions cannot meet personalized requirements. User behavior data is not generated at the mobile client. The user behavior data includes some parameters that are specific to personalized campaigns, such as the source, channel, environment, and behavior parameters.
-   Traditional solutions are difficult and expensive to develop. To collect and analyze data, you must purchase cloud hosts, public IP addresses, servers used to receive development data, and message-oriented middleware. You must configure mutual backup to ensure the high service availability. In addition, you must develop and test the server.
-   Traditional solutions are not user-friendly. After data is transmitted to the server, you must clear the data and import the data to the database. Then, you can generate data for your business operation
-   Traditional solutions cannot provide auto scaling and cannot estimate the resource usage of users. Therefore, a large resource pool must be reserved.

For these reasons, if you need to deliver content to intended users, these users require an efficient method to collect and analyze user behavior data.

[Log Service](https://www.alibabacloud.com/zh/product/log-service?spm) provides Web Tracking, JavaScript, and Tracking Pixel SDKs for the preceding lightweight scenarios where you need to collect data based on tracking points. This allows you to report tracking points and data within 1 minute. In addition, Log Service provides 500 MB of free quota per month for each Alibaba Cloud account. For more information, see [Pricing](/intl.en-US/Pricing/Pay-as-you-go.md).

## Features

Log Service is a one-stop service that is developed to collect and analyze log data. Log Service allows you to quickly collect, consume, ship, query, and analyze large amounts of log data without the need for custom development. This improves both O&M efficiency and operational efficiency. Log Service has the following modules:

-   LogHub: This module allows you to collect and consume log data in real time. LogHub is connected with Blink, Flink, Spark Streaming, Storm, and Kepler.
-   LogShipper: This module allows you to ship data to consumers. LogShipper is connected with MaxCompute, E-MapReduce, Object Storage Service \(OSS\), and Function Compute.
-   LogSearch and Analytics: This module allows you to query and analyze log data in real time. LogSearch and Analytics is connected with DataV, Grafana, Zipkin, and Tableau.

![Features](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0607549951/p32482.png)

## Benefits on the collection side

Log Service allows you to import data from 30 data sources and provides end-to-end solutions for servers, mobile clients, and embedded devices in multiple programming languages.

-   Logtail: a log collection agent for x86 servers.
-   Android or iOS SDK: an SDK for mobile clients.
-   Producer Library: a library for smart devices and devices that have limited CPU or memory.

![Benefits on the collection side](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0607549951/p32483.png)

Web tracking is a lightweight collection solution in which you only need to use the HTTP GET request method to transmit data to a Log Service Logstore. This solution applies to scenarios where no verification is required to collect data from different sources, such as static web pages, online advertisements, promotional documents, and mobile clients. The following figure shows benefits of the web tracking feature.

![Benefits of the web tracking feature](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0607549951/p32484.png)

## Web tracking process

Web tracking \(also known as tracking pixel\) is an HTML image tag. In web tracking, a 0-pixel image can be embedded on an HTML page and the image is invisible to users by default. When you access the page and the image is loaded, a GET request is initiated to transmit relevant parameters to the server.

To use web tracking, perform the following steps:

1.  Enable the web tracking feature for a Logstore.

    By default, the Logstore does not allow anonymous write accessing. Before you can use the Logstore, you must turn on the WebTracking switch of the Logstore.

2.  Write data to the Logstore based on tracking points.

    You can write data by using one of the following methods:

    -   Use an HTTP GET request method to write data.

        ```
        curl --request GET 'http://${project}.${sls-host}/logstores/${logstore}/track? APIVersion=0.6.0&key1=val1&key2=val2'
        ```

    -   Embed an HTML image tag. Data is automatically written to the Logstore when a user visits the page.

        ```
        <img src='http://${project}.${sls-host}/logstores/${logstore}/track.gif? APIVersion=0.6.0&key1=val1&key2=val2'/>
        or
        <img src='http://${project}.${sls-host}/logstores/${logstore}/track_ua.gif? APIVersion=0.6.0&key1=val1&key2=val2'/> 
        track_ua.gif
        ```

        The server uploads both the custom parameters, and the UserAgent and Referer HTTP headers as log fields.

    -   Use the SDK for JavaScript to write data.

        ```
        <script type="text/javascript" src="loghub-tracking.js" async></script>
        
        var logger = new window.Tracker('${sls-host}','${project}','${logstore}');
        logger.push('customer', 'zhangsan');
        logger.push('product', 'iphone 6s');
        logger.push('price', 5500);
        logger.logger();
        								
        ```


For more information, see [Use web tracking to collect logs](/intl.en-US/Data Collection/Other collection methods/Use web tracking to collect logs.md).

## Scenarios

After you create new content such as new features, promotional campaigns, games, and articles, you can send the contents to users at the earliest opportunity. This is the first and most important step to attract and retain users.

For example, your company distributed 10,000 advertisements to promote a game. Only 2,000 advertisements were loaded for users, accounting for 20% of the total number of advertisements. Only 800 users clicked the adverting link. Fewer users downloaded the game, created accounts, and tried the game.

![Scenarios](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0607549951/p32485.png)

In this example, if you can monitor the effectiveness of the promotion in real time, you can promote the game with better results. To reach your promotion goals, you can use the following channels:

-   Internal messages, official blogs, and homepage banners.
-   SMS messages, emails, and leaflets.
-   New media, such as Sina Weibo, DingTalk group, WeChat public account, Zhihu forum, and TouTiao.

![Promotion channels](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0607549951/p32486.png)

## Procedure

1.  Enable the web tracking feature.

    Create a Logstore \(for example, myclick\) in Log Service and enable the web tracking feature.

2.  Generate a web tracking tag.

    1.  Add an identifier to each promotion channel for the article \(named 1001\) that you want to promote. Then, generate a web tracking tag named img.

        -   Internal message \(mailDec\)

            ```
            <img src="http://example.cn-hangzhou.log.aliyuncs.com/logstores/myclick/track_ua.gif?APIVersion=0.6.0&from=mailDec&article=1001" alt="" title="">
            ```

        -   Official website \(aliyunDoc\)

            ```
            <img src="http://example.cn-hangzhou.log.aliyuncs.com/logstores/myclick/track_ua.gif?APIVersion=0.6.0&from=aliyundoc&article=1001" alt="" title="">
            ```

        -   Email \(email\)

            ```
            <img src="http://example.cn-hangzhou.log.aliyuncs.com/logstores/myclick/track_ua.gif?APIVersion=0.6.0&from=email&article=1001" alt="" title="">
            ```

        You can add more channels at the end of the "from" parameter, or add more parameters in the URL to collect their values.

    2.  Add the img tag to the promotion content and release the content.

3.  Analyze logs.

    After you collect logs, you can use the log query and analysis feature of Log Service to query and analyze large amounts of log data in real time. For more information, see [Overview](/intl.en-US/Index and query/Overview.md). Log Service can show log analysis results on built-in dashboards and connect with DataV, Grafana, and Tableau to visualize the log analysis results. For more information, see [Create a dashboard](/intl.en-US/Query and visualization/Dashboard/Create a dashboard.md), [Connect Log Service with DataV](/intl.en-US/Developer Guide/Other visualization methods/Connect Log Service with DataV.md), and [Connect to Log Service by using Grafana](/intl.en-US/Developer Guide/Other visualization methods/Connect to Log Service by using Grafana.md).

    The following figure shows the collected log data. You can enter a keyword in the search box to query logs.

    ![Query by keyword](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0607549951/p32487.png)

    You can also enter an SQL statement after the query to analyze the log data and visualize the analysis results within seconds.

    ![Analyze log data in real time](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0607549951/p32488.png)

    1.  Create query statements.

        The following examples describe how to create query statements to obtain page click and page view \(PV\) statistics. For more information, see [Real-time log analysis](/intl.en-US/Index and query/Real-time log analysis.md).

        -   To query the current total traffic and PVs, execute the following statement:

            ```
            * | select count(1) as c
            ```

        -   To query the curve of the PVs per hour, execute the following statement:

            ```
            * | select count(1) as c, date_trunc('hour',from_unixtime(__time__)) as time group by time order by time desc limit 100000
            ```

        -   To query the ratios of PVs in each channel, execute the following statement:

            ```
            * | select count(1) as c, f group by f desc
            ```

        -   To query the devices to which the page views belong, execute the following statement:

            ```
            * | select count_if(ua like '%Mac%')  as mac, count_if(ua like '%Windows%')  as win, count_if(ua like '%iPhone%')  as ios, count_if(ua like '%Android%')  as android
            ```

        -   To query the locations to which the page views belong, execute the following statement:

            ```
            * | select ip_to_province(__source__) as province, count(1) as c group by province order by c desc limit 100
            ```

    2.  Configure a dashboard to visualize the collected data in real time, as shown in the following figure.

        ![Dashboard](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1607549951/p32489.png)


**Note:** The img tag records your access to this topic. You can find the tag in the source code of the page.

