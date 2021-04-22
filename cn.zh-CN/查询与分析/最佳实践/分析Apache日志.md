# 分析Apache日志

日志服务支持采集Apache日志，并进行多维度分析。本文通过PV、UV、访问地域分布、错误请求、客户端类型等维度分析Apache日志，以评估网站访问情况。

已采集Apache日志，详情请参见[使用Apache模式采集日志](/cn.zh-CN/数据采集/Logtail采集/采集文本日志/使用Apache模式采集日志.md)。

在数据接入向导中，已根据日志字段自动生成索引，如果您要修改索引，详情请参见[配置索引](/cn.zh-CN/查询与分析/配置索引.md)。

Apache是一款主流的网站服务器，当您选用Apache搭建网站时，Apache日志是运维网站的重要信息。

日志服务支持通过数据接入向导一站式采集Apache日志，并为Apache日志创建索引和仪表盘。apache\_Apache访问日志仪表盘包括来源IP分布、请求状态占比、请求方法占比、访问PV/UV统计、流入流出流量统计、请求UA占比、前十访问来源、访问前十地址和请求时间前十地址等信息，全方位展示网站访问情况。

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在Project列表区域，单击目标Project。

3.  在**日志存储** \> **日志库**中，单击目标Logstore左侧的**\>**。

4.  在可视化仪表盘中，单击**apache\_Apache访问日志**。

    apache\_Apache访问日志仪表盘包括如下图表：

    -   **来源IP分布**图展示访问IP地址的来源情况，所关联的查询分析语句如下所示：

        ```
        * | select ip_to_province(remote_addr) as address, count(1) as c group by ip_to_province(remote_addr) limit 100
        ```

        ![来源IP分布](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9013469951/p9389.png)

    -   **请求状态占比**图展示最近一天各HTTP状态码的占比情况，所关联的查询分析语句如下所示：

        ```
        * | select status, count(1) as pv group by status
        ```

        ![请求状态占比](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9013469951/p9388.png)

    -   **请求方法占比**图展示最近一天各请求方法的占比情况，所关联的查询分析语句如下所示：

        ```
        * | select request_method, count(1) as pv group by request_method
        ```

        ![请求方法占比](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9013469951/p9387.png)

    -   **访问PV/UV统计**图展示最近一天内的PV数和UV数，所关联的查询分析语句如下所示：

        ```
        * | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i')  as time, count(1) as pv, approx_distinct(remote_addr) as uv group by date_format(date_trunc('hour', __time__), '%m-%d %H:%i') order by time limit 1000
        ```

        ![PV/UV统计](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9013469951/p9385.png)

    -   **流入流出流量统计**图展示流量的流入和流出情况，所关联的查询分析语句如下所示：

        ```
        * | select date_format(date_trunc('hour', __time__), '%m-%d %H:%i') as time, sum(bytes_sent) as net_out, sum(bytes_received) as net_in group by time order by time limit 10000
        ```

        ![出入流量统计](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0113469951/p9391.png)

    -   **请求UA占比**图展示最近一天各种浏览器的占比情况，所关联的查询分析语句如下所示：

        ```
        * | select case when http_user_agent like '%Chrome%' then 'Chrome' when http_user_agent like '%Firefox%' then 'Firefox' when http_user_agent like '%Safari%' then 'Safari' else 'unKnown' end as http_user_agent, count(1) as pv group by case when http_user_agent like '%Chrome%' then 'Chrome' when http_user_agent like '%Firefox%' then 'Firefox' when http_user_agent like '%Safari%' then 'Safari' else 'unKnown' end   order by pv desc limit 10
        ```

        ![请求UA占比](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0113469951/p9390.png)

    -   **前十访问来源**图展示最近一天PV数最多的前十个访问来源页面，所关联的查询分析语句如下所示：

        ```
        * | select  http_referer, count(1) as pv group by http_referer order by pv desc limit 10
        ```

        ![前十访问来源](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0113469951/p10098.png)

    -   **访问前十地址**展示最近一天PV数最多的前十个访问地址，所关联的查询分析语句如下所示：

        ```
        * | select split_part(request_uri,'?',1) as path,  count(1) as pv group by split_part(request_uri,'?',1) order by pv desc limit 10
        ```

        ![访问前十地址](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0113469951/p9386.png)

    -   **请求时间前十地址**图展示最近一天请求响应延时最长的前十个地址，所关联的查询分析语句如下所示：

        ```
        * | select request_uri as top_latency_request_uri,
                    request_time_sec 
                    order by request_time_sec desc limit 10 10
        ```

        ![请求时间前十地址](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0113469951/p10099.png)


