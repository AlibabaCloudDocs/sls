# 采集-通过WebTracking采集日志

本文档为您介绍如何通过WebTracking采集日志数据到日志服务中，并对采集到的日志数据进行查询和分析。

当发送重要邮件时为了确认对方已读，都会在邮件中设置**读取回执**标签，可以在对方已读时收到提醒信息。读取回执这种模式用途很广，例如。

-   发送传单时，确保对方已读。
-   推广网页时，多少用户做了点击。
-   移动App运营活动页面，分析用户访问情况。

对这类个性化的采集与统计，针对网站与站长的传统方案都无法胜任，主要难点在于。

-   个性化需求难满足：用户产生行为并非移动端场景，其中会包括一些运营个性化需求字段，例如：来源、渠道、环境、行为等参数。
-   开发难度大/成本高：为完成一次数据采集、分析需求，首先需要购买云主机、公网IP、开发数据接收服务器、消息中间件等，并且通过互备保障服务高可用。接下来需要开发服务端并进行测试。
-   使用不易：数据达到服务端后，还需要工程师先清洗结果并导入数据库，生成运营需要的数据。
-   无法弹性：无法预估用户的使用量，因此需要预留很大的资源池。

从以上几点看，当一个面向内容投放的运营需求来了后，如何能以快捷的手段满足这类用户行为采集、分析需求是一个很大的挑战。

[日志服务](https://www.aliyun.com/product/sls?spm)提供Web Tracking/JS/Tracking Pixel SDK 就是为解决以上轻量级埋点采集场景而生，我们可以在1分钟时间内完成埋点和数据上报工作。此外日志服务每账号每月提供500MB [免费额度](/cn.zh-CN/产品定价/按量付费.md)，让您不花钱也能处理业务。

## 功能特点

这里引入采集 + 分析方案基于阿里云日志服务，该服务是针对日志类数据的一站式服务，无需开发就能快捷完成海量日志数据的采集、消费、投递以及查询分析等功能，提升运维、运营效率。服务功能包括。

-   LogHub：实时采集与消费。与Blink、Flink、Spark Streaming、Storm、Kepler等打通。
-   数据投递：LogShipper。与MaxCompute、E-MapReduce、OSS、Function Compute等打通。
-   查询与实时分析：LogSearch/Analytics。与DataV、Grafana、Zipkin、Tableau等打通。

![功能特点](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9240559951/p32482.png)

## 采集端优势

日志服务提供30+种数据采集方式，针对服务器、移动端、嵌入式设备及各种开发语言都提供完整的解决方案。

-   Logtail：针对X86服务器设计Agent。
-   Android/iOS：针对移动端SDK。
-   Producer Library：面向受限CPU/内存、智能设备。

![采集端优势](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9240559951/p32483.png)

本文档中介绍的轻量级采集方案（Web Tracking）只需一个http get请求即可将数据传输至日志服务Logstore端，适应各种无需任何验证的静态网页、广告投放、宣传资料和移动端数据采集。相比其他日志采集方案，特点如下。

![Web Tracking特点](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9240559951/p32484.png)

## Web Tracking接入流程

Web Tracking（也叫Tracking Pixel）术语来自于HTML语法中的图片标签：我们可以在页面上嵌入一个0 Pixel图片，该图片默认对用户不可见，当访问该页面显示加载图片时，会顺带发起一个Get请求到服务端，这个时候就会把参数传给服务端。

Web Tracking接入流程如下。

1.  为Logstore打开Web Tracking标签。

    Logstore默认不允许匿名写，在使用前需要先开通Logstore的Web Tracking开关。

2.  通过埋点方式向Logstore写入数据。

    您可以通过以下三种方式写入数据。

    -   直接通过HTTP Get方式上报数据。

        ```
        curl --request GET 'http://${project}.${sls-host}/logstores/${logstore}/track?APIVersion=0.6.0&key1=val1&key2=val2'
        ```

    -   通过嵌入HTML 下Image标签，当页面方式时自动上报数据。

        ```
        <img src='http://${project}.${sls-host}/logstores/${logstore}/track.gif?APIVersion=0.6.0&key1=val1&key2=val2'/>
        or
        <img src=‘http://${project}.${sls-host}/logstores/${logstore}/track_ua.gif?APIVersion=0.6.0&key1=val1&key2=val2’/> 
        track_ua.gif
        ```

        除了将自定义的参数上传外，在服务端还会将http头中的UserAgent、referer也作为日志中的字段。

    -   通过Java Script SDK 上报数据。

        ```
        <script type="text/javascript" src="loghub-tracking.js" async></script>
        
        var logger = new window.Tracker('${sls-host}','${project}','${logstore}');
        logger.push('customer', 'zhangsan');
        logger.push('product', 'iphone 6s');
        logger.push('price', 5500);
        logger.logger();
                                        
        ```


详细步骤请参见[使用Web Tracking采集日志](/cn.zh-CN/数据采集/其他采集方式/使用Web Tracking采集日志.md)。

## 应用场景

当我们有一个新内容时（例如新功能、新活动、新游戏、新文章），作为运营人员总是迫不及待地希望能尽快传达到用户，因为这是获取用户的第一步、也是最重要的一步。

以游戏发行为例，市场很大一笔费用进行游戏推广，例如投放了1W次广告。广告成功加载的有2000人次，约占20%。其中点击的有800人次，最终下载并注册账号试玩的往往少之又少。

![应用场景](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9240559951/p32485.png)

可见，能够准确、实时地获得内容推广有效性对于业务非常重要。为了达到整体推广目标，运营人员往往会挑选各个渠道来进行推广。

-   用户站内信（Mail）、官网博客（Blog）、首页文案（Banner等）。
-   短信、用户Email、传单等。
-   新浪微博、钉钉用户群、微信公众账号、知乎论坛、今日头条等新媒体。

![推广渠道](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9240559951/p32486.png)

## 操作步骤

1.  开启Web Tracking功能。

    在日志服务中创建一个Logstore（例如叫：myclick），并开启WebTracking功能。

2.  生成Web Tracking标签。

    1.  为需要宣传的文档（article=1001）面对每个宣传渠道增加一个标识，并生成Web Tracking标签（以Img标签为例）。

        -   站内信渠道（mailDec）。

            ```
            <img src="http://example.cn-hangzhou.log.aliyuncs.com/logstores/myclick/track_ua.gif?APIVersion=0.6.0&from=mailDec&article=1001" alt="" title="">
            ```

        -   官网渠道（aliyunDoc）。

            ```
            <img src="http://example.cn-hangzhou.log.aliyuncs.com/logstores/myclick/track_ua.gif?APIVersion=0.6.0&from=aliyundoc&article=1001" alt="" title="">
            ```

        -   用户邮箱渠道（email）。

            ```
            <img src="http://example.cn-hangzhou.log.aliyuncs.com/logstores/myclick/track_ua.gif?APIVersion=0.6.0&from=email&article=1001" alt="" title="">
            ```

        其他更多渠道可以在from参数后加上，也可以在URL中加入更多需要采集的参数。

    2.  将img标签放置在宣传内容中，并进行发布。

3.  分析日志。

    在完成埋点采集后，我们使用日志服务[LogSearch/Analytics](/cn.zh-CN/查询与分析/查询简介.md)功能可以对海量日志数据进行实时查询与分析。在结果分析可视化上，除[自带Dashboard](/cn.zh-CN/可视化与告警/仪表盘/创建仪表盘.md)外，还支持[DataV](/cn.zh-CN/开发指南/可视化开发/对接DataV.md)、[Grafana](/cn.zh-CN/开发指南/可视化开发/对接Grafana.md)、Tableau等对接方式。

    以下是截止目前采集日志数据，您可以在搜索框中输入关键词进行查询。

    ![关键词查询](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9240559951/p32487.png)

    也可以在查询后输入SQL进行秒级的实时分析并可视化。

    ![实时分析](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9240559951/p32488.png)

    1.  设计查询语句。

        以下是我们对用户点击/阅读日志的实时分析语句，更多字段和分析场景可以参见[分析语法](/cn.zh-CN/查询与分析/分析简介.md)。

        -   当前投放总流量与阅读数。

            ```
            * | select count(1) as c
            ```

        -   每个小时阅读量的曲线。

            ```
            * | select count(1) as c, date_trunc('hour',from_unixtime(__time__)) as time group by time order by time desc limit 100000
            ```

        -   每种渠道阅读量的比例。

            ```
            * | select count(1) as c, f group by f desc
            ```

        -   阅读量来自哪些设备。

            ```
            * | select count_if(ua like '%Mac%')  as mac, count_if(ua like '%Windows%')  as win, count_if(ua like '%iPhone%')  as ios, count_if(ua like '%Android%')  as android
            ```

        -   阅读量来自哪些省市。

            ```
            * | select ip_to_province(__source__) as province, count(1) as c group by province order by c desc limit 100
            ```

    2.  将这些实时数据配置到一个实时刷新Dashboard中，效果如下。

        ![仪表盘](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0340559951/p32489.png)


**说明：** 当您看完本文时，会有一个不可见的Img标签记录本次访问，您可以在本页面源代码中查看该标签。

