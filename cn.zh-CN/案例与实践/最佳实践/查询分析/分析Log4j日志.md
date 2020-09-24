# 分析Log4j日志

本文以电商平台的日志为例，为您介绍Log4j日志的分析操作。

-   已采集Log4j日志，详情请参见[采集Log4j日志](/cn.zh-CN/数据采集/采集常见日志/采集Log4j日志.md)。
-   已开启并配置索引，详情请参见[开启并配置索引](/cn.zh-CN/查询与分析/开启并配置索引.md)。

    本案例的索引如下图所示。

    ![指定字段查询](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4708449951/p5909.png)


Log4j是Apache的一个开放源代码项目，通过Log4j可以控制日志的优先级、输出目的地和输出格式。日志级别从高到低为ERROR、WARN、INFO、DEBUG，日志的输出目的地指定了将日志打印到控制台还是文件中，输出格式控制了输出的日志内容格式。

例如某电商公司，希望通过分析用户行为习惯数据（例如用户登录方式、上线的时间点及时长、浏览页面、页面停留时间、平均下单时间、消费水平等）、平台稳定性、系统报错、数据安全性等信息获取平台的最佳运营方案。针对此需求，日志服务提供一站式数据采集与分析功能，帮忙客户存储并分析日志。日志服务采集到的日志样例如下所示：

-   记录用户登录行为的日志：

    ```
    level:  INFO  
    location:  com.aliyun.log4jappendertest.Log4jAppenderBizDemo.login(Log4jAppenderBizDemo.java:38)
    message:  User login successfully. requestID=id4 userID=user8  
    thread:  main  
    time:  2018-01-26T15:31+0000
    ```

-   记录用户购买行为的日志：

    ```
    level:  INFO  
    location:  com.aliyun.log4jappendertest.Log4jAppenderBizDemo.order(Log4jAppenderBizDemo.java:46)
    message:  Place an order successfully. requestID=id44 userID=user8 itemID=item3 amount=9  
    thread:  main  
    time:  2018-01-26T15:31+0000
    ```


## 操作步骤

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在Project列表区域，单击目标Project。

3.  在**日志管理** \> **日志库**页签中，单击目标Logstore。

4.  在页面右上角，单击**15分钟（相对）**，设置查询的时间范围。

    您可以选择相对时间、整点时间和自定义时间范围。

    **说明：** 查询结果相对于指定的时间范围来说，有1 min以内的误差。

5.  在搜索框中输入查询分析语句，单击**查询/分析**。

    查询分析语句详情请参见[查询分析语句格式](/cn.zh-CN/查询与分析/查询简介.md)。

    -   统计最近1小时发生错误最多的3个位置。

        ```
        level: ERROR | select location ,count(*) as count GROUP BY  location  ORDER BY count DESC LIMIT 3
        ```

    -   统计最近15分钟各种日志级别产生的日志条数。

        ```
        | select level ,count(*) as count GROUP BY level ORDER BY count DESC
        ```

    -   统计最近1小时登录次数最多的三个用户。

        ```
        login | SELECT regexp_extract(message, 'userID=(?<userID>[a-zA-Z\d]+)', 1) AS userID, count(*) as count GROUP BY userID ORDER BY count DESC LIMIT 3
        ```

    -   统计最近15分钟每个用户的付款总额。

        ```
        order | SELECT regexp_extract(message, 'userID=(?<userID>[a-zA-Z\d]+)', 1) AS userID, sum(cast(regexp_extract(message, 'amount=(?<amount>[a-zA-Z\d]+)', 1) AS double)) AS amount GROUP BY userID
        ```


