# 通过Syslog投递日志到SIEM

Syslog是一个常见的日志通道，几乎所有的SIEM（例如IBM Qradar, HP Arcsight）都支持通过Syslog渠道接收日志。本文主要介绍如何通过Syslog将日志服务中的日志投递到SIEM。

## 背景信息

-   Syslog主要是基于[RFC5424](https://datatracker.ietf.org/doc/rfc5424/)和[RFC3164](https://tools.ietf.org/html/rfc3164)定义相关格式规范，RFC3164协议是2001年发布的，RFC5424协议是2009年发布的升级版本。因为新版兼容旧版，且新版本解决了很多问题，因此推荐使用RFC5424协议。
-   Syslog over TCP/TLS：Syslog只规定日志格式，理论上TCP和UDP都支持Syslog，可以较好的保证数据传输稳定性。[RFC5425](https://tools.ietf.org/html/rfc5425)协议也定义了TLS的安全传输层，如果您的SIEM支持TCP通道或者TLS通道，则建议优先使用。
-   Syslog facility：早期Unix定义的[程序组件](https://tools.ietf.org/html/rfc3164#section-4.1.1)，此处选择`user`作为默认组件。
-   Syslog severity：定义[日志级别](https://tools.ietf.org/html/rfc3164#section-4.1.1)，您可以根据需求设置指定内容的日志为较高的级别。默认一般用`info`。

**说明：** 本文中的配置代码仅为示例，最新最全的代码示例请参考[Github](https://github.com/aliyun/aliyun-log-python-sdk/blob/master/tests/consumer_group_examples/sync_data_to_syslog.py)。

## 投递流程

推荐使用日志服务消费组构建程序来进行实时消费，然后通过Syslog over TCP/TLS来发送日志给SIEM。

![投递流程](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3877688951/p47433.png)

## 主程序示例

如下代码展示主程序控制逻辑。

```
def main():
    option, settings = get_monitor_option()

    logger.info("*** start to consume data...")
    worker = ConsumerWorker(SyncData, option, args=(settings,) )
    worker.start(join=True)

if __name__ == '__main__':
    main()
```

## 程序配置示例

-   配置内容：
    -   程序日志文件：以便后续测试或者诊断问题。
    -   基本配置项：包括日志服务连接配置和消费组配置。
    -   消费组的高级选项：性能调参，不推荐修改。
    -   SIEM的Syslog server相关参数与选项。

        **说明：** 如果SIEM支持基于TCP或者TLS的Syslog通道，则需要配置proto为TLS及配置正确的SSL的证书。

-   代码示例

    请仔细阅读代码中相关注释并根据业务需求调整选项。

    ```
    #encoding: utf8
    import os
    import logging
    from logging.handlers import RotatingFileHandler
    
    root = logging.getLogger()
    handler = RotatingFileHandler("{0}_{1}.log".format(os.path.basename(__file__), current_process().pid), maxBytes=100*1024*1024, backupCount=5)
    handler.setFormatter(logging.Formatter(fmt='[%(asctime)s] - [%(threadName)s] - {%(module)s:%(funcName)s:%(lineno)d} %(levelname)s - %(message)s', datefmt='%Y-%m-%d %H:%M:%S'))
    root.setLevel(logging.INFO)
    root.addHandler(handler)
    root.addHandler(logging.StreamHandler())
    
    logger = logging.getLogger(__name__)
    
    def get_option():
        ##########################
        # 基本选项
        ##########################
    
        #从环境变量中加载日志服务参数与选项。
        endpoint = os.environ.get('SLS_ENDPOINT', '')
        accessKeyId = os.environ.get('SLS_AK_ID', '')
        accessKey = os.environ.get('SLS_AK_KEY', '')
        project = os.environ.get('SLS_PROJECT', '')
        logstore = os.environ.get('SLS_LOGSTORE', '')
        consumer_group = os.environ.get('SLS_CG', '')
    
        # 消费的起点。这个参数在首次运行程序的时候有效，后续再次运行时将从上一次消费的保存点继续消费。
        # 可以使用“begin”、“end”，或者特定的ISO时间格式。
        cursor_start_time = "2018-12-26 0:0:0"
    
        ##########################
        # 一些高级选项
        ##########################
    
        # 一般不要修改消费者名称，尤其是需要并发消费时
        consumer_name = "{0}-{1}".format(consumer_group, current_process().pid)
    
        # 心跳时长，当服务器在2倍时间内没有收到特定Shard的心跳报告时，服务器会认为对应消费者离线并重新调配任务。
        # 所以当网络环境不佳的时候，不建议将时长设置的比较小。
        heartbeat_interval = 20
    
        # 消费数据的最大间隔，如果数据生成的速度很快，不需要调整这个参数。
        data_fetch_interval = 1
    
        # 构建一个消费组和消费者
        option = LogHubConfig(endpoint, accessKeyId, accessKey, project, logstore, consumer_group, consumer_name,
                              cursor_position=CursorPosition.SPECIAL_TIMER_CURSOR,
                              cursor_start_time=cursor_start_time,
                              heartbeat_interval=heartbeat_interval,
                              data_fetch_interval=data_fetch_interval)
    
        # syslog options
        settings = {
                    "host": "1.2.3.4", # 必选
                    "port": 514,       # 必选，端口
                    "protocol": "tcp", # 必选，tcp、udp或tls（仅Python3）。
                    "sep": "||",       # 必选，key=value键值对的分隔符，这里用||分隔。
                    "cert_path": None, # 可选，TLS的证书位置。
                    "timeout": 120,    # 可选，超时时间，默认120秒。
                    "facility": syslogclient.FAC_USER,  #可选，可以参考其他syslogclient.FAC_*的值。
                    "severity": syslogclient.SEV_INFO,  #可选，可以参考其他syslogclient.SEV_*的值。
                    "hostname": None,  # 可选，机器名，默认选择本机机器名。
                    "tag": None        # 可选，标签，默认是短划线（-）。
                }
    
        return option, settings
    ```


## 消费与投递示例

如下代码展示如何从日志服务获取数据投递到SIEM Syslog服务器。请仔细阅读代码中相关注释并根据需求调整格式。

```
from syslogclient import SyslogClientRFC5424 as SyslogClient

class SyncData(ConsumerProcessorBase):
    """
    消费者从日志服务消费数据并发送给Syslog server
    """
    def __init__(self, splunk_setting):
      """初始化并验证Syslog server连通性"""
        super(SyncData, self).__init__()   # remember to call base's init

        assert target_setting, ValueError("You need to configure settings of remote target")
        assert isinstance(target_setting, dict), ValueError("The settings should be dict to include necessary address and confidentials.")

        self.option = target_setting
        self.protocol = self.option['protocol']
        self.timeout = int(self.option.get('timeout', 120))
        self.sep = self.option.get('sep', "||")
        self.host = self.option["host"]
        self.port = int(self.option.get('port', 514))
        self.cert_path=self.option.get('cert_path', None)

        # try connection
        with SyslogClient(self.host, self.port, proto=self.protocol, timeout=self.timeout, cert_path=self.cert_path) as client:
            pass

    def process(self, log_groups, check_point_tracker):
        logs = PullLogResponse.loggroups_to_flattern_list(log_groups, time_as_str=True, decode_bytes=True)
        logger.info("Get data from shard {0}, log count: {1}".format(self.shard_id, len(logs)))
        try:
            with SyslogClient(self.host, self.port, proto=self.protocol, timeout=self.timeout, cert_path=self.cert_path) as client:
                for log in logs:
                    # Put your sync code here to send to remote.
                    # the format of log is just a dict with example as below (Note, all strings are unicode):
                    #    Python2: {"__time__": "12312312", "__topic__": "topic", u"field1": u"value1", u"field2": u"value2"}
                    #    Python3: {"__time__": "12312312", "__topic__": "topic", "field1": "value1", "field2": "value2"}
                    # suppose we only care about audit log
                    timestamp = datetime.fromtimestamp(int(log[u'__time__']))
                    del log['__time__']

                    io = six.StringIO()
                    first = True
          # TODO：这里可以根据需要修改格式化内容，这里使用Key=Value传输，并使用默认的||进行分割。
                    for k, v in six.iteritems(log):
                        io.write("{0}{1}={2}".format(self.sep, k, v))

                    data = io.getvalue()

          # TODO：这里可以根据需要修改facility或者severity。
                    client.log(data, facility=self.option.get("facility", None), severity=self.option.get("severity", None), timestamp=timestamp, program=self.option.get("tag", None), hostname=self.option.get("hostname", None))

        except Exception as err:
            logger.debug("Failed to connect to remote syslog server ({0}). Exception: {1}".format(self.option, err))

            # TODO: 需要添加一些错误处理的代码，例如重试或者通知等。
            raise err

        logger.info("Complete send data to remote")

        self.save_checkpoint(check_point_tracker)
```

## 启动程序示例

假设程序命名为sync\_data.py，启动程序示例如下所示。

```
export SLS_ENDPOINT=<Endpoint of your region>
export SLS_AK_ID=<YOUR AK ID>
export SLS_AK_KEY=<YOUR AK KEY>
export SLS_PROJECT=<SLS Project Name>
export SLS_LOGSTORE=<SLS Logstore Name>
export SLS_CG=<消费组名，可以简单命名为"syc_data">

python3 sync_data.py
```

## 限制与约束

每一个日志库（logstore）最多可以配置10个消费组，如果遇到`ConsumerGroupQuotaExceed`则表示限制，建议在控制台删除一些不再使用的消费组。

## 消费状态与监控

在控制台查看消费组状态，详情请参见[查看消费组状态](/intl.zh-CN/消费与投递/实时消费/消费组消费/通过消费组消费日志数据.md)。

## 并发消费

基于消费组的程序，可以直接启动多次程序以实现并发效果。

```
nohup python3 sync_data.py &
nohup python3 sync_data.py &
nohup python3 sync_data.py &
...
```

**说明：** 所有消费者的名称均不相同（消费者名以进程ID为后缀），且属于同一个消费组。因为一个分区（Shard）只能被一个消费者消费，假设一个日志库有10个分区，那么最多有10个消费组同时消费。

## 吞吐量

基于测试，在没有带宽、接收端速率限制（如Splunk端）的情况下，用python3运行上述样例，单个消费者大约占用20%的单核CPU资源，此时消费可以达到10 MB/s原始日志的速率。因此10个消费者理论上可以达到100 MB/s原始日志每个CPU核，即每个CPU核每天可以消费0.9 TB原始日志。

## 高可用

消费组将检测点（check-point）保存在服务器端，当一个消费者停止，另外一个消费者将自动接管并从断点继续消费。可以在不同机器上启动消费者，这样在一台机器停止或者损坏的情况下，其他机器上的消费者可以自动接管并从断点进行消费。理论上，为了备用，也可以通过不同机器启动大于Shard数量的消费者。

