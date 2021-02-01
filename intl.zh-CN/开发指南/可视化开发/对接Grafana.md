# 对接Grafana

本文介绍如何通过Grafana可视化分析日志服务所采集到的Nginx日志。

-   已采集Nginx日志数据。更多信息，请参见[使用Nginx模式采集日志](/intl.zh-CN/数据采集/Logtail采集/采集文本日志/使用Nginx模式采集日志.md)。
-   已开启并配置索引。更多信息，请参见[分析Nginx访问日志](/intl.zh-CN/查询与分析/最佳实践/分析Nginx访问日志.md)。

## 步骤1：安装Grafana和插件

1.  安装Grafana。具体操作，请参见[Grafana官方文档](http://docs.grafana.org/installation/)。

    **说明：**

    -   如果您是在本机上安装Grafana，请提前在浏览器中打开3000端口。
    -   如果您需要使用饼图，需执行如下命令安装Pie Chart插件。

        ```
        grafana-cli plugins install grafana-piechart-panel
        ```

2.  安装日志服务插件。

    1.  执行如下命令进入Grafana插件安装目录。

        例如在Ubuntu系统，您需要在/var/lib/grafana/plugins/中安装插件。

        ```
        cd /var/lib/grafana/plugins/
        ```

    2.  执行如下命令安装插件。

        ```
        git clone --depth 1 https://github.com/aliyun/aliyun-log-grafana-datasource-plugin
        ```

        您也可以下载[master.zip](https://github.com/aliyun/aliyun-log-grafana-datasource-plugin/archive/master.zip)到/var/lib/grafana/plugins/目录中，进行安装。

    3.  执行如下命令重启服务。

        ```
        service grafana-server restart
        ```

3.  如果您安装的是Grafana 7.0及以上版本，需修改Grafana配置文件。

    1.  打开配置文件。

        -   macOS系统中的文件路径：/usr/local/etc/grafana/grafana.ini
        -   Linux系统中的文件路径：/etc/grafana/grafana.ini
    2.  在plugins中设置allow\_loading\_unsigned\_plugins参数。

        ```
        allow_loading_unsigned_plugins = aliyun-log-service-datasource,grafana-log-service-datasource
        ```


## 步骤2：配置数据源

1.  登录Grafana。

2.  在左侧菜单栏，选择**![G1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7664559951/p112522.png)** \> **Data Sources**。

3.  在Data Sources页签，单击**Add data source**。

4.  在Add data source页面，单击**LogService**对应的**Select**。

5.  配置数据源。

    重要参数说明如下表所示。

    |参数|说明|
    |:-|:-|
    |Name|数据源的名称。|
    |HTTP|配置URL、Access和Whitelisted Cookies，具体说明如下：    -   URL：格式为`http://Endpoint`，请根据实际情况替换Endpoint，更多信息，请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。例如`http://cn-qingdao.log.aliyuncs.com`。
    -   Access：取值为Server\(default\)或Browser。
    -   Whitelisted Cookies：添加访问白名单。 |
    |Auth|保持默认配置。|
    |log service details|配置日志服务详细信息，包括Project名称、Logstore名称和具备读取权限的AccessKey。为了您的阿里云账号安全，建议您使用RAM用户的AccessKey。|

6.  单击**Save & Test**。


## 步骤3：添加仪表盘

1.  在左侧导航栏，选择**![图标1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7664559951/p112912.png)** \> **Dashboards**。

2.  在New Panel面板中，单击**Choose Visualization**。

3.  配置模板变量。

    您可以在Grafana中配置模板变量，实现在同一个图表中通过选择不同的变量值，展示不同的结果。

    1.  配置时间区间大小的模板变量。

        1.  在New dashboard页面右上角，配置时间区间，然后单击![图标2](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7664559951/p112911.png)图标。
        2.  单击**Variables**。
        3.  单击**Add variable**。
        4.  按照如下参数配置模板变量，然后单击**Add**。

            重要参数说明如下表所示。

            |参数|说明|
            |--|--|
            |Name|变量名称，例如myinterval。该名称是您配置中使用的变量，此处为myinterval，则查询条件中需写成`$myinterval`。|
            |Type|选择**Interval**。|
            |Lable|配置为**time interval**。|
            |Values|配置为**1m,10m,30m,1h,6h,12h,1d,7d,14d,30d**。|
            |Auto Option|打开**Auto Option**开关，其他参数保持默认配置。|

    2.  配置域名的模板变量。

        1.  在Variables页面，单击**New**。
        2.  按照如下参数配置模板变量，然后单击**Add**。

            |参数|说明|
            |:-|:-|
            |Name|变量名称，例如hostname。该名称是您配置中使用的变量，此处为hostname，则查询条件中需写成`$hostname`。|
            |Type|选择**Custom**。|
            |Lable|输入域名。|
            |Custom Options|配置为`*,www.host.com,www.host0.com,www.host1.com`，表示可以查看所有域名的访问情况，也可以分别查看`www.host.com`、`www.host0.com`或`www.host1.com`的访问情况。|
            |Selection Options|保持默认配置。|

    3.  在左侧菜单栏，单击**Save**。

4.  添加可视化图表。

    -   用于展示PV&UV的图表（Graph）
        1.  单击右上角**![图标3 ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7664559951/p112908.png)**图标。
        2.  在New Panel页面，单击**Add Query**。
        3.  单击**![G小图标4](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8664559951/p112921.png)**图标，在**Visualization**下拉列表中选择**Graph**。
        4.  单击**![G小图标5](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8664559951/p112926.png)**图标，在**Title**文本框输入**UV&PV**。
        5.  单击**![G小图标6](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8664559951/p112923.png)**图标，在**Query**下拉列表中选择**Logservice**，并完成如下配置。

            ![grafana-pv&uv01](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3684178061/p112935.png)

            |参数|说明|
            |:-|:-|
            |Query|查询和分析语句示例如下：            ```
$hostname| select approx_distinct(remote_addr) as uv ,count(1) as pv , __time__ - __time__ % $$myinterval as time group by time order by time limit 1000
            ```

在展示结果中，`$hostname`会被替换成您选择的域名。`$$myinterval`会被替换成您选择的时间区间。

**说明：** myinterval前有2个美元符号（$$），hostname前只有1个美元符号（$）。 |
            |X-Column|配置为**time**。|
            |Y-Column|配置为**uv,pv**。|

        6.  如果UV和PV的值相差较大，可配置双Y轴图表。

            ![grafana-pv&uv02](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3684178061/p112952.png)

        7.  单击右上角**![保存按钮](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8664559951/p113049.png)**图标，根据页面提示完成保存。
    -   用于展示流入流出流量的图表（Graph）

        添加步骤请参见[添加PV&UV信息图表](#step_718_ird_p4y)，相关参数说明如下表所示。

        |参数|说明|
        |:-|:-|
        |Query|查询和分析语句示例如下：        ```
$hostname | select sum(body_byte_sent) as net_out, sum(request_length) as net_in,__time__ - __time__ % $$myinterval as time group by __time__ - __time__ % $$myinterval limit 10000
        ```

在展示结果中，`$hostname`会被替换成您选择的域名。`$$myinterval`会被替换成您选择的时间区间。

**说明：** myinterval前有2个美元符号（$$），hostname前只有1个美元符号（$）。 |
        |X-Column|配置为**time**。|
        |Y-Column|配置为**net\_in,net\_out**。|

    -   用于展示HTTP请求方法占比的图表（Pie Chart）

        添加步骤请参见[添加PV&UV信息图表](#step_718_ird_p4y)，相关参数说明如下表所示。

        |参数|说明|
        |:-|:-|
        |Query|查询和分析语句示例如下：        ```
$hostname | select count(1) as pv ,method group by method
        ```

在展示结果中，`$hostname`会被替换成您选择的域名。 |
        |X-Column|配置为**pie**。|
        |Y-Column|配置为**method,pv**。|

    -   用于展示HTTP请求状态码占比的图表（Pie Chart）

        添加步骤请参见[添加PV&UV信息图表](#step_718_ird_p4y)，相关参数说明如下表所示。

        |参数|说明|
        |:-|:-|
        |Query|查询和分析语句示例如下：        ```
$hostname | select count(1) as pv ,status group by status
        ```

在展示结果中，`$hostname`会被替换成您选择的域名。 |
        |X-Column|配置为**pie**。|
        |Y-Column|配置为**status,pv**。|

    -   用于展示热门访问来源的图表（Pie Chart）

        添加步骤请参见[添加PV&UV信息图表](#step_718_ird_p4y)，相关参数说明如下表所示。

        |参数|说明|
        |:-|:-|
        |Query|查询和分析语句示例如下：        ```
$hostname | select count(1) as pv , referer group by referer order by pv desc
        ```

在展示结果中，`$hostname`会被替换成您选择的域名。 |
        |X-Column|配置为**pie**。|
        |Y-Column|配置为**referer,pv**。|

    -   用于展示延时最高页面的图表（Table）

        添加步骤请参见[添加PV&UV信息图表](#step_718_ird_p4y)，相关参数说明如下表所示。

        |参数|说明|
        |:-|:-|
        |Query|查询和分析语句示例如下：        ```
$hostname | select URL as top_latency_URL ,request_time order by request_time desc limit 10
        ```

在展示结果中，`$hostname`会被替换成您选择的域名。 |
        |X-Column|无需配置。|
        |Y-Column|配置为**top\_latency\_url,request\_time**。|

    -   用于展示热门页面的图表（Table）

        添加步骤请参见[添加PV&UV信息图表](#step_718_ird_p4y)，相关参数说明如下表所示。

        |配置项|说明|
        |:--|:-|
        |Query|查询和分析语句示例如下：        ```
$hostname | select count(1) as pv, split_part(URL,'?',1) as path group by split_part(URL,'?',1) order by pv desc limit 20
        ```

在展示结果中，`$hostname`会被替换成您选择的域名。 |
        |X-Column|无需配置。|
        |Y-Column|配置为**path,pv**。|

    -   用于展示非200请求的Top页面图表（Table）

        添加步骤请参见[添加PV&UV信息图表](#step_718_ird_p4y)，相关参数说明如下表所示。

        |配置项|说明|
        |:--|:-|
        |Query|查询和分析语句示例如下：        ```
$hostname not status:200| select count(1) as pv , url group by url order by pv desc
        ```

在展示结果中，`$hostname`会被替换成您选择的域名。 |
        |X-Column|无需配置。|
        |Y-Column|配置为**url,pv**。|

    -   用于展示平均延时的图表（Singlestat）

        添加步骤请参见[添加PV&UV信息图表](#step_718_ird_p4y)，相关参数说明如下：

        |配置项|说明|
        |:--|:-|
        |Query|查询和分析语句示例如下：        ```
$hostname | select avg(request_time) as response_time, avg(upstream_response_time) as upstream_response_time ,__time__ - __time__ % $$myinterval as time group by __time__ -  __time__ % $$myinterval limit 10000
        ```

在展示结果中，`$hostname`会被替换成您选择的域名。`$$myinterval`会被替换成您选择的时间区间。

**说明：** myinterval前有2个美元符号（$$），hostname前只有1个美元符号（$）。 |
        |X-Column|配置为**time**。|
        |Y-Column|配置为**upstream\_response\_time,response\_time**。|

    -   用于展示详细日志的图表（Logs）

        添加步骤请参见[添加PV&UV信息图表](#step_718_ird_p4y)。

        **说明：** 每页最多展示100条，即**Logs Per Page**最大值为100。

    -   用于展示客户端统计的图表（Pie Chart）

        添加步骤请参见[添加PV&UV信息图表](#step_718_ird_p4y)，相关参数说明如下：

        |配置项|说明|
        |:--|:-|
        |Query|查询和分析语句示例如下：        ```
$hostname  | select count(1) as pv, case when  regexp_like(http_user_agent , 'okhttp') then 'okhttp' when  regexp_like(http_user_agent ,  'iPhone') then 'iPhone' when regexp_like(http_user_agent ,  'Android')  then 'Android' else 'unKnown' end as http_user_agent group by  http_user_agent order by pv desc limit 10
        ```

在展示结果中，`$hostname`会被替换成您选择的域名。 |
        |X-Column|配置为**pie**。|
        |Y-Column|配置为**http\_user\_agent,pv**。|

    -   用于展示1分钟内各个HTTP请求状态数量的图表（Graph）

        添加步骤请参见[添加PV&UV信息图表](#step_718_ird_p4y)，相关参数说明如下所示。

        |配置项|说明|
        |:--|:-|
        |Query|查询和分析语句示例如下：        ```
$hostname  | select to_unixtime(time) as time,status,count from (select time_series(__time__, '1m', '%Y-%m-%d %H:%i', '0')  as time,status,count(*) as count from log group by status,time order by time limit 10000)
        ```

在展示结果中，`$hostname`会被替换成您选择的域名。 |
        |X-Column|配置为时间列。|
        |Y-Column|配置为col1\#:\#col2。其中col1为聚合列，col2为其他列。|

    -   用于展示来源IP分布的图表（Worldmap Panel）

        添加步骤请参见[添加PV&UV信息图表](#step_718_ird_p4y)，相关参数说明如下。

        |参数|说明|
        |:-|:-|
        |Query|查询和分析语句示例如下：        ```
$hostname  | select   count(1) as pv ,geohash(ip_to_geo(arbitrary(remote_addr))) as geo,ip_to_country(remote_addr) as country  from log group by country having geo <>'' limit 1000
        ```

在展示结果中，`$hostname`会被替换成您选择的域名。 |
        |X-Column|配置为**map**。|
        |Y-Column|配置为**country,geo,pv**。|

        |参数|说明|
        |:-|:-|
        |Location Data|配置为**geohash**。|
        |Location Name Field|配置为**country**。|
        |geo\_point/geohash Field|配置为**geo**。|
        |Metric Field|配置为**pv**。|

5.  查看结果。

    您可以在Dashboard页面上方选择统计的时间范围，还可以筛选time interval和hostname。


## 常见问题

-   Grafana日志保存在哪里？

    Grafana日志保存在如下文件中：

    -   macOS系统：/usr/local/var/log/grafana
    -   Linux系统：/var/log/grafana
-   如果日志中提示**aliyun-log-plugin\_linux\_amd64: permission denied**，怎么处理？

    请授予插件目录下的dist/aliyun-log-plugin\_linux\_amd64目录执行权限。


