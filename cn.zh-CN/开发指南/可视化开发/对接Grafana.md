# 对接Grafana

本文主要通过Grafana示例演示如何将日志服务采集的Nginx日志进行实时可视化分析。

-   已采集Nginx日志数据。详情请参见[Nginx模式](/cn.zh-CN/数据采集/Logtail采集/采集文本日志/Nginx模式.md)。
-   已开启并配置索引，详情请参见[采集并分析Nginx访问日志](/cn.zh-CN/案例与实践/最佳实践/采集/采集并分析Nginx访问日志.md)。

## 流程架构

日志从收集到分析的流程架构如下。

![LogService_user_guide_0206.png ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7664559951/p5856.png)

## 操作步骤

1.  安装Grafana。详情请参见[Grafana官方文档](http://docs.grafana.org/installation/)。

    以安装Ubuntu为例，需执行以下安装命令。

    ```
    wget https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana_4.5.2_amd64.deb
    sudo apt-get install -y adduser libfontconfig
    sudo dpkg -i grafana_4.5.2_amd64.deb
    ```

    若您需要使用饼状图，则需执行以下命令安装Pie chart插件。

    ```
    grafana-cli plugins install grafana-piechart-panel
    ```

2.  安装日志服务插件。

    请确认Grafana的插件目录位置。在Ubuntu的插件目录/var/lib/grafana/plugins/安装插件，重启grafana-server。

    以Ubuntu系统为例，执行以下命令安装插件，并重启grafana-server。

    ```
    cd /var/lib/grafana/plugins/
    git clone https://github.com/aliyun/aliyun-log-grafana-datasource-plugin
    service grafana-server restart
    ```

3.  配置日志服务数据源。

    **说明：** 若您是在本机部署，默认是安装在3000端口。请提前在浏览器打开3000端口。

    1.  登录Grafana。

    2.  在左侧菜单栏，单击**![G1](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7664559951/p112522.png)图标** \> **Data Sources**。

    3.  在Data Sources页签，单击**Add data source**，选中**LogService**，单击**Select**，请参考如下说明配置数据源。

        |配置项|说明|
        |:--|:-|
        |Name|Name表示名称，请自定义一个数据源的名称。|
        |HTTP|        -   URL示例：`http://dashboard-demo.cn-hangzhou.log.aliyuncs.com`。`dashboard-demo`是project名称，`cn-hangzhou.log.aliyuncs.com`是project所在地域的Endpoint，在配置自己的数据源时，需要替换成自己的project和region地址，请参见[服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)。
        -   Access可以选择Server，也可以选择Browser。
        -   Whitelisted：添加访问白名单。 |
        |Auth|采用默认配置。|
        |log service details|日志服务详细配置，分别填写Project和Logstore，以及具备读取权限的AccessKey。AccessKey可以是主账号的AccessKey，也可以是子帐号的AccessKey。|

    4.  单击**Save & Test**。

4.  添加Dashboard。

    1.  在左侧导航栏，选中**![图标1](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7664559951/p112912.png) 图标** \> **Dashboards**，添加一个Dashboard。

    2.  配置模板变量。

        在Grafana中可以配置模板变量，在同一个视图中，通过选择不同的变量值，展示不同的视图。本文档主要配置每个时间区间的大小，以及不同域名的访问情况。

        1.  在New dashboard页面右上角，通过下拉框选择配置一个时间区间，单击页面右上角的**![图标2](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7664559951/p112911.png) 图标** \> **Variables** \> **Add variable**，进入配置页面。
        2.  变量的名称是您在配置中使用的变量，在这里起名为myinterval，在查询条件中，要写成`$myinterval`，参考以下样例进行配置，并单击**Add**。

            |配置项|说明|
            |---|--|
            |Name|变量名称，您可以命名为myinterval。|
            |Type|选择**Interval**。|
            |Lable|输入time interval。|
            |Internal Options|            -   **value**输入`1m,10m,30m,1h,6h,12h,1d,7d,14d,30d`。
            -   选中**Auto Option**。 |

        3.  配置一个域名模板。

            通常一个VPS上可以挂载多个域名，则需要查看不同域名的访问情况。

            在Variable页面，单击**New**，参考以下说明配置域名模板。

            |配置项|说明|
            |:--|:-|
            |Name|变量名称，您可以命名为hostname。|
            |Type|选中**Custom**。|
            |Lable|输入域名。|
            |Custom Options|**Values separated by**输入`*,www.host.com,www.host0.com,www.host1.com`。表示可以查看所有域名的访问情况，也可以分别查看`www.host.com`、`www.host0.com`或`www.host1.com`的访问情况。|
            |Selection Options|配置为默认值。|

        4.  在左侧菜单栏，单击**Save**保存配置完成的模板。
    3.  视图配置示例。

        -   配置PV & UV
            1.  单击右上角**![图标3 ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7664559951/p112908.png)**图标。
            2.  在New Panel页面，单击**Add Query**。
            3.  单击**![G小图标4](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8664559951/p112921.png)**图标，在**Visualization**下拉列表选中**Graph**，创建一个Graph视图。
            4.  单击**![G小图标5](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8664559951/p112926.png)**图标，在**Title**文本框输入**UV & PV**。
            5.  单击**![G小图标6](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8664559951/p112923.png)**图标，在**Query**下拉列表选中**Logservice**，配置项请参见如下说明。

                ![grafana-pv&uv01](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8664559951/p112935.png)

                |配置项|说明|
                |:--|:-|
                |Query|                ```
$hostname| select approx_distinct(remote_addr) as uv ,count(1) as pv , __time__ - __time__ % $$myinterval as time group by time order by time limit 1000
                ```

上述Query中的$hostname，在实际展示时，会替换成用户选择的域名。$$myinterval，则会替换成时间区间，注意myinterval前有两个$符号，而hostname有一个。|
                |X-Column|time|
                |Y-Column|uv,pv|

                若UV和PV的值相差较大，可通过调整为双Y轴图表展示。单击图表下方的图示线条（如下图①），单击**Y-Axis**（如下图②），打开**Use right y-axis**开关（如下图③）。

                ![grafana-pv&uv02](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8664559951/p112952.png)

        -   配置出入网带宽

            添加出入网带宽的视图，创建步骤请参见[配置PV & UV](#table_rh6_y1y_9va)。

            主要配置项如下：

            |配置项|说明|
            |:--|:-|
            |Query|            ```
$hostname | select sum(body_byte_sent) as net_out, sum(request_length) as net_in,__time__ - __time__ % $$myinterval as time group by __time__ - __time__ % $$myinterval limit 10000
            ``` |
            |X-Column|Time|
            |Y-Column|net\_in,net\_out|

        -   不同HTTP方法的占比

            添加不同HTTP方法的占比的视图，创建步骤请参见[配置PV & UV](#table_rh6_y1y_9va)。

            单击**![G小图标4](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8664559951/p112921.png)**图标，在**Visualization**下拉列表选中**Pie Chart**，创建一个饼图。主要配置项如下：

            |配置项|说明|
            |:--|:-|
            |Query|            ```
$hostname | select count(1) as pv ,method group by method
            ``` |
            |X-Column|pie|
            |Y-Column|method,pv|

        -   不同HTTP状态码占比

            添加不同HTTP状态码占比的视图，创建步骤请参见[配置PV & UV](#table_rh6_y1y_9va)。

            单击**![G小图标4](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8664559951/p112921.png)**图标，**Visualization**选中**Pie Chart**，创建一个饼图。主要配置项如下：

            |配置项|说明|
            |:--|:-|
            |Query|            ```
$hostname | select count(1) as pv ,status group by status
            ``` |
            |X-Column|pie|
            |Y-Column|status,pv|

        -   热门来源页面

            添加热门来源页面的视图，创建步骤请参见[配置PV & UV](#table_rh6_y1y_9va)。

            单击**![G小图标4](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8664559951/p112921.png)**图标，在**Visualization**下拉列表选中**Pie Chart**，创建一个饼图。主要配置项如下：

            |配置项|说明|
            |:--|:-|
            |Query|            ```
$hostname | select count(1) as pv , referer group by referer order by pv desc
            ``` |
            |X-Column|pie|
            |Y-Column|referer,pv|

        -   延时最高页面

            添加延时最高页面的视图，创建步骤请参见[配置PV & UV](#table_rh6_y1y_9va)。

            单击**![G小图标4](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8664559951/p112921.png)**图标，在**Visualization**下拉列表选中**Table**，创建一个表格，展示URL和对应的延时。主要配置项如下：

            |配置项|说明|
            |:--|:-|
            |Query|            ```
$hostname | select URL as top_latency_URL ,request_time order by request_time desc limit 10
            ``` |
            |X-Column|X-Column不填写内容|
            |Y-Column|top\_latency\_url,request\_time|

        -   热门页面

            添加热门页面的视图，创建步骤请参见[配置PV & UV](#table_rh6_y1y_9va)。

            单击**![G小图标4](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8664559951/p112921.png)**图标，在**Visualization**下拉列表选中**Table**，创建一个表格。主要配置项如下：

            |配置项|说明|
            |:--|:-|
            |Query|            ```
$hostname | select count(1) as pv, split_part(URL,'?',1) as path group by split_part(URL,'?',1) order by pv desc limit 20
            ``` |
            |X-Column|X-Column不填写内容|
            |Y-Column|path,pv|

        -   非200请求top页面

            添加非200请求top页面的视图，创建步骤请参见[配置PV & UV](#table_rh6_y1y_9va)。

            单击**![G小图标4](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8664559951/p112921.png)**图标，在**Visualization**下拉列表选中**Table**，创建一个表格。主要配置项如下：

            |配置项|说明|
            |:--|:-|
            |Query|            ```
$hostname not status:200| select count(1) as pv , url group by url order by pv desc
            ``` |
            |X-Column|X-Column不填写内容|
            |Y-Column|url,pv|

        -   前后端平均延时

            添加前后端平均延时的视图，创建步骤请参见[配置PV & UV](#table_rh6_y1y_9va)。

            主要配置如下：

            |配置项|说明|
            |:--|:-|
            |Query|            ```
$hostname | select avg(request_time) as response_time, avg(upstream_response_time) as upstream_response_time ,__time__ - __time__ % $$myinterval as time group by __time__ -  __time__ % $$myinterval limit 10000
            ``` |
            |X-Column|time|
            |Y-Column|upstream\_response\_time,response\_time|

        -   设置Logs

            添加Logs视图，创建步骤请参见[配置PV & UV](#table_rh6_y1y_9va)。

            单击**![G小图标4](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8664559951/p112921.png)**图标，在**Visualization**下拉列表选中**Logs**，创建一个Logs。配置页面输入**Logs Per Page**即完成。

            **说明：** 每页最多展示100条，即**Logs Per Page**最大值为100。

        -   客户端统计

            添加客户端统计的视图，创建步骤请参见[配置PV & UV](#table_rh6_y1y_9va)。

            单击**![G小图标4](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8664559951/p112921.png)**图标，在**Visualization**下拉列表选中**Pie Chart**，创建一个饼图。主要配置项如下：

            |配置项|说明|
            |:--|:-|
            |Query|            ```
$hostname  | select count(1) as pv, case when  regexp_like(http_user_agent , 'okhttp') then 'okhttp' when  regexp_like(http_user_agent ,  'iPhone') then 'iPhone' when regexp_like(http_user_agent ,  'Android')  then 'Android' else 'unKnown' end as http_user_agent group by  http_user_agent order by pv desc limit 10
            ``` |
            |X-Column|pie|
            |Y-Column|http\_user\_agent,pv|

        -   设置流图

            添加流图，创建步骤请参见[配置PV & UV](#table_rh6_y1y_9va)。

            主要配置项如下：

            |配置项|说明|
            |:--|:-|
            |Query|            ```
$hostname  | select to_unixtime(time) as time,status,count from (select time_series(__time__, '1m', '%Y-%m-%d %H:%i', '0')  as time,status,count(*) as count from log group by status,time order by time limit 10000)
            ``` |
            |X-Column|配置为时间列。|
            |Y-Column|col1\#:\#col2，其中col1为聚合列，col2为其他列。|

        -   设置地图

            添加地图，创建步骤请参见[配置PV & UV](#table_rh6_y1y_9va)。

            单击**![G小图标4](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8664559951/p112921.png)**图标，在**Visualization**下拉列表选中**Worldmap Panel**，创建一个地图。Queries主要配置项如下：

            |配置项|说明|
            |:--|:-|
            |Query|            ```
$hostname  | select   count(1) as pv ,geohash(ip_to_geo(arbitrary(remote_addr))) as geo,ip_to_country(remote_addr) as country  from log group by country having geo <>'' limit 1000
            ``` |
            |X-Column|map|
            |Y-Column|country,geo,pv|

            Visualization主要配置项如下，其他配置为默认值。

            |配置项|说明|
            |:--|:-|
            |Location Data|geohash|
            |Location Name Field|country|
            |geo\_point/geohash Field|geo|
            |Metric Field|pv|

    4.  单击右上角**![保存按钮](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8664559951/p113049.png)**图标，保存Dashboard。

5.  查看结果。

    打开Dashboard首页查看效果。示例请参见[Demo](http://47.96.36.117:3000/dashboard/db/nginxfang-wen-tong-ji?orgId=1)。

    您可以在Dashboard页面上方选择统计的时间范围，也可以选择统计的时间粒度或不同的域名。整个Nginx访问统计的Dashboard完成，您可以从视图中挖掘有价值的信息。


