# 采集MySQL监控数据

您可使用Telegraf采集MySQL监控数据，再通过日志服务Logtail将Telegraf数据上传到MetricStore中，搭建MySQL可视化监控方案。本文介绍如何通过日志服务来完成MySQL监控数据的采集和可视化。

-   服务器可连通待监控的MySQL数据库。
-   已在服务器上安装Logtail（Linux Logtail 0.16.44及以上版本），详情请参见[安装Logtail（Linux系统）](https://help.aliyun.com/document_detail/28982.html#t13056.html)。

## 使用限制

-   仅支持MySQL 5.5及以上版本。
-   不支持Windows版本。

## 步骤1：创建Logtail采集配置

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在接入数据区域，选择**MySQL监控**。

3.  在选择日志空间页签中，选择目标Project和MetricStore，单击**下一步**。

    您也可以单击**立即创建**，重新创建Project和MetricStore，详情请参见[创建Project](/cn.zh-CN/数据采集/准备工作/管理Project.md)和[创建MetricStore](/cn.zh-CN/时序存储/管理MetricStore.md)。

4.  在创建机器组页签中，创建机器组。

    -   如果您已有可用的机器组，请单击**使用现有机器组**。
    -   如果您还没有可用的机器组，请执行以下操作（以ECS为例）：
        1.  选择ECS实例安装Logtail，详情请参见[安装Logtail（ECS实例）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（ECS实例）.md)。

            如果已在ECS上安装Logtail，请直接单击**确认安装完毕**。

            **说明：** 如果是自建集群、其他云厂商服务器，需要手动安装Logtail，详情请参见[安装Logtail（Linux系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)。

        2.  安装完成后，单击**确认安装完毕**。
        3.  创建机器组，详情请参见[创建IP地址机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建IP地址机器组.md)或[创建用户自定义标识机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建用户自定义标识机器组.md)。
5.  在机器组配置页签中，应用机器组。

    选择一个机器组，将该机器组从**源机器组**移动到**应用机器组**。

6.  在Logtail配置页签中，配置**配置名称**和**插件配置**。

    inputs为Logtail采集配置，必选项，请根据您的数据源配置。

    **说明：** 一个inputs中只允许配置一个类型的数据源。

    ```
    {
        "inputs": [
            {
                "detail": {
                    "Format": "influx",
                    "Address": ":8476"
                },
                "type": "service_http_server"
            }
        ],
        "global": {
            "AlwaysOnline": true,
            "DelayStopSec": 500
        }
    }
    ```

    |参数|类型|是否必选|参数说明|
    |--|--|----|----|
    |type|string|是|数据源类型，固定为service\_http\_server。|
    |Format|string|是|数据类型，固定为influx。|
    |Address|string|是|监听地址与端口，格式为ip:port。|

7.  单击**下一步**，完成配置。


## 步骤2：安装Telegraf

1.  登录服务器。

2.  安装Telegraf。

    -   如果您的服务器是CentOS或AliOS系统，请执行如下命令安装Telegraf。

        ```
        wget https://dl.influxdata.com/telegraf/releases/telegraf-1.15.2-1.x86_64.rpm
        yum localinstall telegraf-1.15.2-1.x86_64.rpm
        systemctl enable --now telegraf
        systemctl status telegraf
        ```

    -   如果您的服务器是Ubuntu系统，请执行如下命令安装Telegraf。

        ```
        wget https://dl.influxdata.com/telegraf/releases/telegraf_1.15.2-1_amd64.deb
        dpkg -i telegraf_1.15.2-1_amd64.deb
        systemctl enable --now telegraf
        systemctl status telegraf
        ```


## 步骤3：配置Telegraf

1.  登录服务器。

2.  配置Logtail端口。

    1.  打开/etc/telegraf/telegraf.conf文件。

    2.  将/etc/telegraf/telegraf.conf文件中的内容替换下如下脚本。

        其中 \[\[outputs.influxdb\]\]下的urls为您在[步骤6](#step_ukh_596_6ak)中配置的Address地址，默认为"http://127.0.0.1:8476"。

        ```
        # Global tags can be specified here in key="value" format.
        [global_tags]
          # dc = "us-east-1" # will tag all metrics with dc=us-east-1
          # rack = "1a"
          ## Environment variables can be used as tags, and throughout the config file
          # user = "$USER"
        
        # Configuration for telegraf agent
        [agent]
          ## Default data collection interval for all inputs
          interval = "10s"
          ## Rounds collection interval to 'interval'
          ## ie, if interval="10s" then always collect on :00, :10, :20, etc.
          round_interval = true
        
          ## Telegraf will send metrics to outputs in batches of at most
          ## metric_batch_size metrics.
          ## This controls the size of writes that Telegraf sends to output plugins.
          metric_batch_size = 1000
        
          ## Maximum number of unwritten metrics per output.  Increasing this value
          ## allows for longer periods of output downtime without dropping metrics at the
          ## cost of higher maximum memory usage.
          metric_buffer_limit = 10000
        
          ## Collection jitter is used to jitter the collection by a random amount.
          ## Each plugin will sleep for a random time within jitter before collecting.
          ## This can be used to avoid many plugins querying things like sysfs at the
          ## same time, which can have a measurable effect on the system.
          collection_jitter = "0s"
        
          ## Default flushing interval for all outputs. Maximum flush_interval will be
          ## flush_interval + flush_jitter
          flush_interval = "10s"
          ## Jitter the flush interval by a random amount. This is primarily to avoid
          ## large write spikes for users running a large number of telegraf instances.
          ## ie, a jitter of 5s and interval 10s means flushes will happen every 10-15s
          flush_jitter = "0s"
        
          ## By default or when set to "0s", precision will be set to the same
          ## timestamp order as the collection interval, with the maximum being 1s.
          ##   ie, when interval = "10s", precision will be "1s"
          ##       when interval = "250ms", precision will be "1ms"
          ## Precision will NOT be used for service inputs. It is up to each individual
          ## service input to set the timestamp at the appropriate precision.
          ## Valid time units are "ns", "us" (or "µs"), "ms", "s".
          precision = ""
        
        
          ## Maximum number of rotated archives to keep, any older logs are deleted.
          ## If set to -1, no archives are removed.
          # logfile_rotation_max_archives = 5
        
          ## Override default hostname, if empty use os.Hostname()
          hostname = ""
          ## If set to true, do no set the "host" tag in the telegraf agent.
          omit_hostname = false
              
        ###############################################################################
        #                            OUTPUT PLUGINS                                   #
        ###############################################################################
        
        
        # Configuration for sending metrics to Logtail's InfluxDB receiver
        [[outputs.influxdb]]
          ## The full HTTP Logtail listen address
          urls = ["http://127.0.0.1:8476"]
          ## Always be true
          skip_database_creation = true
        ```

3.  增加MySQL监控配置。

    1.  在/etc/telegraf/telegraf.d目录下创建mysql.conf配置文件。

    2.  在mysql.conf配置文件中填入如下MySQL监控相关信息，并根据实际情况替换。

        ```
        [[inputs.mysql]]
          ## specify servers via a url matching:
          ##  [username[:password]@][protocol[(address)]]/[?tls=[true|false|skip-verify|custom]]
          ##  see https://github.com/go-sql-driver/mysql#dsn-data-source-name
          ##  e.g.
          ##    servers = ["user:passwd@tcp(127.0.0.1:3306)/?tls=false"]
          ##    servers = ["user@tcp(127.0.0.1:3306)/?tls=false"]
          #
          ## If no servers are specified, then localhost is used as the host.
          servers = ["user:passwd@tcp(127.0.0.1:3306)/?tls=false"]
          metric_version = 2
          ## if the list is empty, then metrics are gathered from all databasee tables
          table_schema_databases = []
          ## gather metrics from INFORMATION_SCHEMA.TABLES for databases provided above list
          gather_table_schema = false
          ## gather thread state counts from INFORMATION_SCHEMA.PROCESSLIST
          gather_process_list = false
          ## gather user statistics from INFORMATION_SCHEMA.USER_STATISTICS
          gather_user_statistics = false
          ## gather auto_increment columns and max values from information schema
          gather_info_schema_auto_inc = false
          ## gather metrics from INFORMATION_SCHEMA.INNODB_METRICS
          gather_innodb_metrics = true
          ## gather metrics from SHOW SLAVE STATUS command output
          gather_slave_status = false
          ## gather metrics from SHOW BINARY LOGS command output
          gather_binary_logs = false
          ## gather metrics from SHOW GLOBAL VARIABLES command output
          gather_global_variables = true
          ## gather metrics from PERFORMANCE_SCHEMA.TABLE_IO_WAITS_SUMMARY_BY_TABLE
          gather_table_io_waits = false
          ## gather metrics from PERFORMANCE_SCHEMA.TABLE_LOCK_WAITS
          gather_table_lock_waits = false
          ## gather metrics from PERFORMANCE_SCHEMA.TABLE_IO_WAITS_SUMMARY_BY_INDEX_USAGE
          gather_index_io_waits = false
          ## gather metrics from PERFORMANCE_SCHEMA.EVENT_WAITS
          gather_event_waits = false
          ## gather metrics from PERFORMANCE_SCHEMA.FILE_SUMMARY_BY_EVENT_NAME
          gather_file_events_stats = false
          ## gather metrics from PERFORMANCE_SCHEMA.EVENTS_STATEMENTS_SUMMARY_BY_DIGEST
          gather_perf_events_statements = false
          ## the limits for metrics form perf_events_statements
          perf_events_statements_digest_text_limit = 120
          perf_events_statements_limit = 250
          perf_events_statements_time_limit = 86400
          ## Some queries we may want to run less often (such as SHOW GLOBAL VARIABLES)
          ##   example: interval_slow = "30m"
          interval_slow = ""
          ## Optional TLS Config (will be used if tls=custom parameter specified in server uri)
          # tls_ca = "/etc/telegraf/ca.pem"
          # tls_cert = "/etc/telegraf/cert.pem"
          # tls_key = "/etc/telegraf/key.pem"
          ## Use TLS but skip chain & host verification
          # insecure_skip_verify = false omit_hostname = false
        
        [[processors.strings]]
          namepass = ["mysql", "mysql_innodb"]
          [[processors.strings.replace]]
            tag = "server"
            old = "127.0.0.1:3306"
            new = "mysql-dev"
          [[processors.strings.replace]]
            tag = "server"
            old = "192.168.1.98:3306"
            new = "mysql-prod"
        ```

        重要参数说明如下表所示。

        |参数|类型|是否必选|说明|
        |--|--|----|--|
        |servers|数组|是|监听的MySQL列表，格式为user:passwd@tcp\(host:port\)/?tls=true/false。|
        |processors.strings.replace|对象|否|默认情况下，上报的数据中Server字段为host:port格式。您可以通过processors.strings.replace，将host:port替换成易识别的值，例如mysql-prod。该对象支持设置多个。|

4.  执行如下命令触发Telegraf重新加载配置。

    ```
    systemctl reload telegraf
    ```


-   查询分析

    配置完成后，Telegraf将采集到的监控数据通过Logtail上传到日志服务MetricStore中。您可以在MetricStore查询分析页面进行查询分析操作，详情请参见[查询分析时序数据](/cn.zh-CN/时序存储/查询与分析/查询分析时序数据.md)。

-   日志服务可视化

    日志服务自动在对应Project中生成MySQL监控仪表盘，您可以直接使用该仪表盘，及进行告警设置等相关操作。

    ![MySQL监控](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1683129951/p144039.png)

-   Grafana可视化

    日志服务为MySQL监控数据提供Grafana模板，您可以使用Grafana仪表盘展示查询分析结果，详情请参见[时序数据对接Grafana](/cn.zh-CN/时序存储/可视化/时序数据对接Grafana.md)，Grafana模板详情请参见[《1 SLS MySQL监控 v2020.08.13》](https://grafana.com/grafana/dashboards/12826)。

    ![Grafana可视化](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2683129951/p144066.png)


