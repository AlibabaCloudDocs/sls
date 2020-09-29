# 采集及分析Nginx监控日志

Nginx中的自建状态页，可帮忙您监控Nginx状态。日志服务支持通过Logtail插件采集Nginx状态信息，并对监控日志进行查询分析、告警等操作，全方位监控您的Nginx集群。

已在服务器上安装Logtail，详情请参见[安装Logtail（Linux系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)或[安装Logtail（Windows系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md)。

**说明：** 目前支持Linux Logtail 0.16.0及以上版本，Window Logtail 1.0.0.8及以上版本。

## 步骤1 准备环境

请按照以下步骤，开启Nginx status插件。

1.  执行以下命令确认Nginx已具备[status功能](http://nginx.org/en/docs/http/ngx_http_stub_status_module.html)。

    ```
    nginx -V 2>&1 | grep -o with-http_stub_status_module
    with-http_stub_status_module 
    ```

    如果回显信息为`with-http_stub_status_module`，表示支持status功能。

2.  配置Nginx status。

    在Nginx配置文件（默认为/etc/nginx/nginx.conf）中开启status功能，配置示例如下所示，详情请参见[Nginx status](https://easyengine.io/tutorials/nginx/status-page/)。

    **说明：** allow 10.10.XX.XX表示只允许IP地址为10.10.XX.XX的服务器访问nginx status功能。

    ```
         location /private/nginx_status {
           stub_status on;
           access_log   off;
           allow 10.10.XX.XX;
           deny all;
         }                       
    ```

3.  执行如下命令验证安装Logtail的服务器具备nginx status访问权限。

    ```
    $curl http://10.10.XX.XX/private/nginx_status
    ```

    如果回显信息如下所示，则表示已完成Nginx status配置。

    ```
    Active connections: 1
    server accepts handled requests
    2507455 2507455 2512972
    Reading: 0 Writing: 1 Waiting: 0                       
    ```


## 步骤2 采集Nginx监控日志

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在接入数据区域，选择**自定义数据插件**。

3.  在选择日志空间页签中，选择目标Project和Logstore，单击**下一步**。

    您也可以单击**立即创建**，重新创建Project和Logstore，详情请参见[步骤1：创建Project和Logstore](/cn.zh-CN/快速入门/快速入门.md)。

4.  在创建机器组页签中，创建机器组。

    -   如果您已有可用的机器组，请单击**使用现有机器组**。
    -   如果您还没有可用的机器组，请执行以下操作（以ECS为例）。
        1.  选择ECS实例安装Logtail，详情请参见[安装Logtail（ECS实例）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（ECS实例）.md)。

            如果已在ECS上安装Logtail，可直接单击**确认安装完毕**。

            **说明：** 如果是自建集群、其他云厂商服务器，需要手动安装Logtail，详情请参见[安装Logtail（Linux系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md#)或[安装Logtail（Windows系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md)。

        2.  安装完成后，单击**确认安装完毕**。
        3.  创建机器组，详情请参见[创建IP地址机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建IP地址机器组.md)或[创建用户自定义标识机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建用户自定义标识机器组.md)。
5.  在机器组配置页签中，应用机器组。

    选择一个机器组，将该机器组从**源机器组**移动到**应用机器组**。

6.  在数据源设置页签中，配置**配置名称**和**插件配置**。

    -   inputs为Logtail采集配置，必选项，请根据您的数据源配置。

        **说明：** 一个inputs中只允许配置一个类型的数据源。

    -   processors为Logtail处理配置，可选项。您可以配置一种或多种处理方式，详情请参见[处理数据](/cn.zh-CN/数据采集/Logtail采集/使用Logtail插件采集数据/处理数据.md)。
    ```
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

    重要参数说明如下表所示：

    |参数|类型|是否必须|说明|
    |:-|:-|:---|:-|
    |type|string|是|数据源类型，固定为metric\_http。|
    |IntervalMs|int|是|每次请求的间隔，单位：ms。|
    |Addresses|string\[\]数组|是|配置为您需要监控的URL列表。|
    |IncludeBody|bool|否|是否采集请求的body，默认值：false。如果为true，则将请求body内容存放在名为content的字段中。|

7.  在**查询分析配置**页签中，设置索引。

    默认已设置索引，您也可以根据业务需求，重新设置索引，具体请参见[开启并配置索引](/cn.zh-CN/查询与分析/开启并配置索引.md)。

    **说明：**

    -   全文索引和字段索引属性必须至少启用一种。同时启用时，以字段索引属性为准。
    -   索引类型为long、double时，大小写敏感和分词符属性无效。

完成采集配置1分钟后，即可查看日志数据，样例如下所示。并且日志服务默认生成nginx\_status仪表盘，展示查询分析结果。

```
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

## 步骤3 查询分析日志

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在Project列表区域，单击目标Project。

3.  在**日志管理** \> **日志库**页签中，单击目标Logstore。

4.  在页面右上角，单击**15分钟（相对）**，设置查询的时间范围。

    您可以选择相对时间、整点时间和自定义时间范围。

    **说明：** 查询结果相对于指定的时间范围来说，有1min以内的误差。

5.  在搜索框中输入查询分析语句，单击**查询/分析**。

    查询分析语句详情请参见[实时分析简介](/cn.zh-CN/查询与分析/实时分析简介.md)，您还可以为查询结果设置告警，详情请参见[设置告警](/cn.zh-CN/可视化与告警/告警/设置告警.md)。

    -   查询日志
        -   查询某IP地址的请求状态。

            ```
            _address_ : 10.10.0.0
            ```

        -   查询响应时间超过100ms的请求。

            ```
            _response_time_ms_ > 100
            ```

        -   查询状态码非200的请求。

            ```
            not _http_response_code_ : 200
            ```

    -   分析日志
        -   每5分钟统计waiting、reading、writing、connection的平均值。

            ```
            *| select  avg(waiting) as waiting, avg(reading)  as reading,  avg(writing)  as writing,  avg(connection)  as connection,  from_unixtime( __time__ - __time__ % 300) as time group by __time__ - __time__ % 300 order by time limit 1440                       
            ```

        -   统计最大等待连接数排名前十的服务器。

            ```
            *| select  max(waiting) as max_waiting, address, from_unixtime(max(__time__)) as time group by address order by max_waiting desc limit 10                        
            ```

        -   统计请求IP的数量以及请求失败的IP数量。

            ```
            * | select  count(distinct(address)) as total                       
            ```

            ```
            not _result_ : success | select  count(distinct(address))                        
            ```

        -   统计最近十次访问失败的IP地址。

            ```
            not _result_ : success | select _address_ as address, from_unixtime(__time__) as time  order by __time__ desc limit 10                       
            ```

        -   每5分钟统计请求处理总数。

            ```
            *| select  avg(handled) * count(distinct(address)) as total_handled, avg(requests) * count(distinct(address)) as total_requests,  from_unixtime( __time__ - __time__ % 300) as time group by __time__ - __time__ % 300 order by time limit 1440                       
            ```

        -   每5分钟统计平均请求延迟。

            ```
            *| select  avg(_response_time_ms_) as avg_delay,  from_unixtime( __time__ - __time__ % 300) as time group by __time__ - __time__ % 300 order by time limit 1440                      
            ```

        -   统计请求成功的数量和失败的数量。

            ```
            not _http_response_code_ : 200  | select  count(1)                     
            ```

            ```
            _http_response_code_ : 200  | select  count(1)                       
            ```


