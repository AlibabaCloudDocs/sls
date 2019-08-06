# 分析-Nginx监控日志 {#concept_65673_zh .concept}

Nginx和php-fpm、Docker、Apache等很多软件一样内建了一个状态页，对于Nginx的状态查看以及监控提供了很大帮助。本文档主要介绍通过日志服务Logtail采集Nginx status信息，并对采集的status信息进行查询、统计、搭建仪表盘、建立自定义报警，对您的Nginx集群进行全方位的监控。

## 环境准备 {#section_zlj_mmb_wfb .section}

请按照以下步骤，开启Nginx status插件。

1.  确认Nginx是否具备[status功能](http://nginx.org/en/docs/http/ngx_http_stub_status_module.html)。

    执行以下命令查看Nginx是否具备status功能：

    ``` {#codeblock_npp_li8_hjk}
    nginx -V 2>&1 | grep -o with-http_stub_status_module
    with-http_stub_status_module
    					
    ```

    如果回显信息为`with-http_stub_status_module`，表示支持status功能。

2.  配置[Nginx status](https://easyengine.io/tutorials/nginx/status-page/)。

    在Nginx的配置文件（默认为/etc/nginx/nginx.conf）中开启status功能，样例配置如下：

    ``` {#codeblock_eoh_knx_713}
         location /private/nginx_status {
           stub_status on;
           access_log   off;
           allow 10.10.XX.XX;
           deny all;
         }
    					
    ```

    **说明：** 该配置只允许ip为`10.10.XX.XX`的机器访问`nginx status`功能。

3.  验证Logtail安装的机器具有`nginx status`访问权限。

    可通过如下命令测试：

    ``` {#codeblock_8b7_d28_8w3}
    $curl http://10.10.XX.XX/private/nginx_status
    Active connections: 1
    server accepts handled requests
    2507455 2507455 2512972
    Reading: 0 Writing: 1 Waiting: 0
    					
    ```


## 数据采集 {#section_zl2_fcd_c67 .section}

1.  安装Logtail

    [安装logtail](../../../../intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)，确认版本号在0.16.0及以上。若低于0.16.0版本请根据文档提示升级到最新版本。

2.  填写采集配置

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13214/156507236432468_zh-CN.png)

    1.  在日志服务控制台创建一个新的Logstore，采集向导中选择自建软件中的Nginx监控。
    2.  根据提示配置Nginx监控的URL以及相关参数（基于http采集功能实现）。

        **说明：** 

        -   将样例配置中`Addresses`字段内容修改为您需要监控的url列表。
        -   如果您的Nginx status返回的信息和默认的不同，请修改`processors`用以支持http的body解析。
        样例配置如下：

        ``` {#codeblock_rxm_0rk_one}
        {
        "inputs": [
         {
              "type": "metric_http",
              "detail": {
                  "IntervalMs": 60000,
                  "Addresses": [
                      "http://10.10.XX.XX/private/nginx_status",
                      "http://10.10.XX.XX/private/nginx_status",
                      "http://10.10.XX.XX/private/nginx_status"
                  ],
                  "IncludeBody": true
              }
         }
        ],
        "processors": [
         {
              "type": "processor_regex",
              "detail": {
                  "SourceKey": "content",
                  "Regex": "Active connections: (\\d+)\\s+server accepts handled requests\\s+(\\d+)\\s+(\\d+)\\s+(\\d+)\\s+Reading: (\\d+) Writing: (\\d+) Waiting: (\\d+)[\\s\\S]*",
                  "Keys": [
                      "connection",
                      "accepts",
                      "handled",
                      "requests",
                      "reading",
                      "writing",
                      "waiting"
                  ],
                  "FullMatch": true,
                  "NoKeyError": true,
                  "NoMatchError": true,
                  "KeepSource": false
              }
         }
        ]
        }
        							
        ```


## 数据预览 {#section_tm5_y1e_4yw .section}

应用配置1分钟后，点击预览可以看到状态数据已经成功采集。Logtail的http采集除了将body解析上传，还会将url、状态码、方法名、响应时间、是否请求成功一并上传。

**说明：** 若无数据，请先检查配置是否为合法json。

``` {#codeblock_f6f_7gm_nk6}
_address_:http://10.10.XX.XX/private/nginx_status  
_http_response_code_:200  
_method_:GET  
_response_time_ms_:1.83716261897  
_result_:success  
accepts:33591200  
connection:450  
handled:33599550  
reading:626  
requests:39149290  
waiting:68  
writing:145  
			
```

## 查询分析 {#section_cuf_rq5_bzc .section}

要对数据进行查询和分析，请先[开启并配置索引](../../../../intl.zh-CN/查询与分析/开启并配置索引.md#)。

## 自定义查询 {#section_sem_39s_k06 .section}

查询相关帮助文档参见[简介](../../../../intl.zh-CN/查询与分析/简介.md)。

1.  查询某一ip的status信息： `_address_ : 10.168.0.0`
2.  查询响应时间超过100ms的请求: `_response_time_ms_ > 100`
3.  查看状态码非200的请求: `not _http_response_code_ : 200`

## 统计分析 {#section_1ul_elp_ei1 .section}

统计分析语法参见[实时分析简介](../../../../intl.zh-CN/查询与分析/实时分析简介.md)。

-   每5分钟统计 waiting reading writing connection 平均值：

    ``` {#codeblock_i1h_uz1_0a1}
    *| select  avg(waiting) as waiting, avg(reading)  as reading,  avg(writing)  as writing,  avg(connection)  as connection,  from_unixtime( __time__ - __time__ % 300) as time group by __time__ - __time__ % 300 order by time limit 1440
    					
    ```

-   统计top 10的 waiting：

    ``` {#codeblock_lx2_jip_8ue}
    *| select  max(waiting) as max_waiting, address, from_unixtime(max(__time__)) as time group by address order by max_waiting desc limit 10
    					
    ```

-   目前Nginx总数以及invalid数量：

    ``` {#codeblock_rdz_g9s_7dj}
    * | select  count(distinct(address)) as total
    					
    ```

    ``` {#codeblock_4fd_xfr_mcy}
    not _result_ : success | select  count(distinct(address))
    					
    ```

-   最近 top 10 失败的请求：

    ``` {#codeblock_j3r_bjf_ypz}
    not _result_ : success | select _address_ as address, from_unixtime(__time__) as time  order by __time__ desc limit 10
    					
    ```

-   每5分钟统计统计请求处理总数：

    ``` {#codeblock_ueq_aps_y0s}
    *| select  avg(handled) * count(distinct(address)) as total_handled, avg(requests) * count(distinct(address)) as total_requests,  from_unixtime( __time__ - __time__ % 300) as time group by __time__ - __time__ % 300 order by time limit 1440
    					
    ```

-   每5分钟统计平均请求延迟：

    ``` {#codeblock_3a1_sfl_gj5}
    *| select  avg(_response_time_ms_) as avg_delay,  from_unixtime( __time__ - __time__ % 300) as time group by __time__ - __time__ % 300 order by time limit 1440
    					
    ```

-   请求有效数/无效数：

    ``` {#codeblock_5q7_3dk_any}
    not _http_response_code_ : 200  | select  count(1)
    					
    ```

    ``` {#codeblock_0sf_fiy_nln}
    _http_response_code_ : 200  | select  count(1)
    					
    ```


## 仪表盘 {#section_t1q_9ed_kkm .section}

日志服务默认对于Nginx监控数据提供了仪表盘，您可以在Nginx status的仪表盘，仪表盘搭建参见[创建和删除仪表盘](../../../../intl.zh-CN/查询与分析/可视化分析/仪表盘/创建和删除仪表盘.md)。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13214/156507236432469_zh-CN.png)

## 设置报警 {#section_92n_fi0_ymu .section}

1.  将以下查询另存为快速查询，名称为`invalid_nginx_status` : `not _http_response_code_ : 200 | select count(1) as invalid_count`。
2.  根据该快速查询[设置告警](../../../../intl.zh-CN/告警/设置告警任务/设置告警.md)，样例如下：

    |配置|取值|
    |--|--|
    |告警名称|invalid\_nginx\_alarm|
    |添加到仪表盘|nginx\_status|
    |图表名称|total|
    |查询语句|\* | select count\(distinct\(\_\_source\_\_\)\) as total|
    |查询区间|15分钟|
    |执行间隔|15分钟|
    |触发条件|total\>100|
    |触发通知阈值|1|
    |通知类型|通知中心|
    |通知内容|nginx status 获取异常，请前往日志服务查看具体异常信息，project : xxxxxxxx, logstore : nginx\_status|


