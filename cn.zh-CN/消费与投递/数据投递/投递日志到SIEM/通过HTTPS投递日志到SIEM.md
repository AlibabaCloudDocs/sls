# 通过HTTPS投递日志到SIEM

本文通过以Splunk HEC为例来展示如何通过HEC将阿里云相关日志投递到SIEM。

例如当前SIEM（如Splunk）位于组织内部环境（on-premise），而不是云端。为了安全考虑，没有开放任何端口让外界环境来访问此SIEM。

**说明：** 本章节中的配置代码仅为示例，最新的代码示例请参见[Github](https://github.com/aliyun/aliyun-log-python-sdk/blob/master/tests/consumer_group_examples/sync_data_to_splunk.py)或[Github（多源日志库时）](https://github.com/aliyun/aliyun-log-python-sdk/blob/master/tests/consumer_group_examples/sync_data_to_splunk_multiple_logstores.py)。

## 投递流程

推荐使用日志服务消费组构建程序进行实时消费，然后通过Splunk API（HEC）发送日志给Splunk。

![投递流程](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6684041261/p274888.png)

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

-   配置内容
    -   程序日志文件：以便后续测试或者诊断问题。
    -   基本配置项：包括日志服务连接配置和消费组配置。
    -   消费组的高级选项：性能调参，不推荐修改。
    -   SIEM（Splunk）相关参数与选项。
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
    
        # 从环境变量中加载日志服务参数与选项。
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
    
        # 一般不建议修改消费者名称，尤其是需要进行并发消费时。
        consumer_name = "{0}-{1}".format(consumer_group, current_process().pid)
    
        # 心跳时长，当服务器在2倍时间内没有收到特定Shard的心跳报告时，服务器会认为对应消费者离线并重新调配任务。
        # 所以当网络环境不佳时，不建议将时长设置的比较小。
        heartbeat_interval = 20
    
        # 消费数据的最大间隔，如果数据生成的速度很快，不需要调整这个参数。
        data_fetch_interval = 1
    
        # 构建一个消费组和消费者
        option = LogHubConfig(endpoint, accessKeyId, accessKey, project, logstore, consumer_group, consumer_name,
                              cursor_position=CursorPosition.SPECIAL_TIMER_CURSOR,
                              cursor_start_time=cursor_start_time,
                              heartbeat_interval=heartbeat_interval,
                              data_fetch_interval=data_fetch_interval)
    
        # Splunk选项
        settings = {
                    "host": "10.1.2.3",
                    "port": 80,
                    "token": "a023nsdu123123123",
                    'https': False,             # 可选, bool
                    'timeout': 120,             # 可选, int
                    'ssl_verify': True,         # 可选, bool
                    "sourcetype": "",           # 可选, sourcetype
                    "index": "",                # 可选, index
                    "source": "",               # 可选, source
                }
    
        return option, settings
    ```


## 消费与投递示例

如下代码展示如何从日志服务获取数据并投递到Splunk，请仔细阅读代码中相关注释并根据需求调整格式。

```
from aliyun.log.consumer import *
from aliyun.log.pulllog_response import PullLogResponse
from multiprocessing import current_process
import time
import json
import socket
import requests

class SyncData(ConsumerProcessorBase):
    """
    这个消费者从日志服务消费数据并发送给Splunk。
    """
    def __init__(self, splunk_setting):
      """初始化并验证Splunk连通性"""
        super(SyncData, self).__init__()

        assert splunk_setting, ValueError("You need to configure settings of remote target")
        assert isinstance(splunk_setting, dict), ValueError("The settings should be dict to include necessary address and confidentials.")

        self.option = splunk_setting
        self.timeout = self.option.get("timeout", 120)

        # 测试Splunk连通性
        s = socket.socket()
        s.settimeout(self.timeout)
        s.connect((self.option["host"], self.option['port']))

        self.r = requests.session()
        self.r.max_redirects = 1
        self.r.verify = self.option.get("ssl_verify", True)
        self.r.headers['Authorization'] = "Splunk {}".format(self.option['token'])
        self.url = "{0}://{1}:{2}/services/collector/event".format("http" if not self.option.get('https') else "https", self.option['host'], self.option['port'])

        self.default_fields = {}
        if self.option.get("sourcetype"):
            self.default_fields['sourcetype'] = self.option.get("sourcetype")
        if self.option.get("source"):
            self.default_fields['source'] = self.option.get("source")
        if self.option.get("index"):
            self.default_fields['index'] = self.option.get("index")

    def process(self, log_groups, check_point_tracker):
        logs = PullLogResponse.loggroups_to_flattern_list(log_groups, time_as_str=True, decode_bytes=True)
        logger.info("Get data from shard {0}, log count: {1}".format(self.shard_id, len(logs)))
        for log in logs:
            # 发送数据到Splunk
            event = {}
            event.update(self.default_fields)
            event['time'] = log[u'__time__']
            del log['__time__']

            json_topic = {"actiontrail_audit_event": ["event"] }
            topic = log.get("__topic__", "")
            if topic in json_topic:
                try:
                    for field in json_topic[topic]:
                        log[field] = json.loads(log[field])
                except Exception as ex:
                    pass
            event['event'] = json.dumps(log)

            data = json.dumps(event, sort_keys=True)

                try:
                    req = self.r.post(self.url, data=data, timeout=self.timeout)
                    req.raise_for_status()
                except Exception as err:
                    logger.debug("Failed to connect to remote Splunk server ({0}). Exception: {1}", self.url, err)

                    # 根据需要，添加一些重试或者报告。

        logger.info("Complete send data to remote")

        self.save_checkpoint(check_point_tracker)
```

## 启动程序示例

例如程序命名为sync\_data.py，启动程序示例如下所示。

```
export SLS_ENDPOINT=<Endpoint of your region>
export SLS_AK_ID=<YOUR AK ID>
export SLS_AK_KEY=<YOUR AK KEY>
export SLS_PROJECT=<SLS Project Name>
export SLS_LOGSTORE=<SLS Logstore Name>
export SLS_CG=<消费组名，可以简单命名为"syc_data">

python3 sync_data.py
```

## 多源日志库示例

针对多源日志库，需要共用一个executor以避免进程过多。更多信息，请参见[多源日志库时发送日志到Splunk](https://github.com/aliyun/aliyun-log-python-sdk/blob/master/tests/consumer_group_examples/sync_data_to_splunk_multiple_logstores.py)。核心的变化是主函数，示例如下所示。

```
exeuctor, options, settings = get_option()

    logger.info("*** start to consume data...")
    workers = []

    for option in options:
        worker = ConsumerWorker(SyncData, option, args=(settings,) )
        workers.append(worker)
        worker.start()

    try:
        for i, worker in enumerate(workers):
            while worker.is_alive():
                worker.join(timeout=60)
            logger.info("worker project: {0} logstore: {1} exit unexpected, try to shutdown it".format(
                options[i].project, options[i].logstore))
            worker.shutdown()
    except KeyboardInterrupt:
        logger.info("*** try to exit **** ")
        for worker in workers:
            worker.shutdown()

        # wait for all workers to shutdown before shutting down executor
        for worker in workers:
            while worker.is_alive():
                worker.join(timeout=60)

    exeuctor.shutdown()
```

## 限制与约束

每一个日志库（logstore）最多可以配置30个消费组，如果遇到`ConsumerGroupQuotaExceed`则表示超出限制，建议在控制台删除一些不再使用的消费组。

## 消费状态与监控

-   在控制台查看消费组状态，详情请参见[查看消费组状态](/cn.zh-CN/消费与投递/实时消费/消费组消费/通过消费组消费日志数据.md)。
-   通过云监控查看消费组延迟情况并配置告警，详情请参见[消费组延迟](/cn.zh-CN/消费与投递/实时消费/消费组消费/消费组监控与告警.md)。

## 并发消费

基于消费组的程序，可以直接启动多次程序以实现并发效果。

```
nohup python3 sync_data.py &
nohup python3 sync_data.py &
nohup python3 sync_data.py &
...
```

**说明：** 所有消费者的名称均不相同（消费者名以进程ID为后缀），且属于同一个消费组。因为一个分区（Shard）只能被一个消费者消费，例如一个日志库有10个分区，那么最多有10个消费组同时消费。

## 吞吐量

基于测试，在没有带宽、接收端速率限制（如Splunk端）的情况下，用python3运行上述样例，单个消费者大约占用20%的单核CPU资源，此时消费可以达到10 MB/s原始日志的速率。因此10个消费者理论上可以达到100 MB/s原始日志，即每个CPU核每天可以消费0.9 TB原始日志。

## 高可用

消费组将检测点（check-point）保存在服务器端，当一个消费者停止，另外一个消费者将自动接管并从断点继续消费。可以在不同机器上启动消费者，这样在一台机器停止或者损坏的情况下，其他机器上的消费者可以自动接管并从断点进行消费。为了备用，也可以通过不同机器启动大于Shard数量的消费者。

## HTTPS

如果服务入口（Endpoint）配置为`https://`前缀，例如`https://cn-beijing.log.aliyuncs.com`，则程序自动使用HTTPS加密与日志服务连接。

服务器证书\*.aliyuncs.com是由GlobalSign签发，默认大多数Linux和Windows机器自动信任此证书。如果有机器不信任此证书，请参见[Certificate installation](https://success.outsystems.com/Support/Enterprise_Customers/Installation/Install_a_trusted_root_CA__or_self-signed_certificate)下载并安装此证书。

