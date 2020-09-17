# Send logs to an SIEM system over Syslog

Syslog is a widely used logging standard. Almost all security information and event management \(SIEM\) systems, such as IBM Qradar and HP Arcsight, can receive logs over Syslog. This topic describes how to send logs on Alibaba Cloud to an SIEM system over Syslog.

## Background information

-   **Syslog** is defined in [RFC 5424](https://datatracker.ietf.org/doc/rfc5424/) and [RFC 3164](https://tools.ietf.org/html/rfc3164). RFC 3164 was published in 2001, and RFC 5424 was an upgraded edition published in 2009. We recommend that you use RFC 5424 because this edition is compatible with the earlier edition and solves many issues.
-   **Syslog over TCP/TLS**: Syslog defines the standard format of log messages. Both TCP and UDP support Syslog to ensure the stability of data transmission. [RFC 5425](https://tools.ietf.org/html/rfc5425) defines the use of Transport Layer Security \(TLS\) to provide a secure connection for the transport of Syslog messages. We recommend that you send Syslog messages over TCP or TLS if your SIEM system supports TCP or TLS.
-   **Syslog facility**: the [program component](https://tools.ietf.org/html/rfc3164#section-4.1.1) defined by earlier versions of Unix. You can select `user` as the default facility.
-   **Syslog severity**: the [severity](https://tools.ietf.org/html/rfc3164#section-4.1.1) defined for Syslog messages. You can set the log of the specified content to a higher severity level based on your business needs. The default value is `info`.

**Note:** Code examples in this topic are used for reference only. For more information about the latest code examples, see [GitHub](https://github.com/aliyun/aliyun-log-python-sdk/blob/master/tests/consumer_group_examples/sync_data_to_syslog.py).

## Workflow

We recommend that you configure the required program based on consumer groups in Log Service. Then, you can use the program to send Syslog messages over TCP or TLS to the SIEM system. We recommend that you send Syslog messages over TCP or TLS if your SIEM system supports TCP or TLS.

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0357549951/p47433.png)

## Example: Write a main program

The following code shows the control logic of a main program:

```
def main():
    option, settings = get_monitor_option()

    logger.info("*** start to consume data...")
    worker = ConsumerWorker(SyncData, option, args=(settings,) )
    worker.start(join=True)

if __name__ == '__main__':
    main()
```

## Example: Configure the program

-   Configure the following information:
    -   Log file of the program: facilitates subsequent testing or diagnosis of potential issues.
    -   Basic configuration items: includes consumer group settings and connection to Log Service.
    -   Advanced options for consumer groups: adjusts performance. We do not recommend that you change these settings.
    -   Parameters and options for the Syslog server of the SIEM system.

        **Note:** If the SIEM system supports sending Syslog messages over TCP or TLS, you must set proto to TLS and configure a valid SSL certificate.

-   Code example:

    Read the code comments in the following example and change parameter settings based on your business needs:

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
        # Basic configuration items
        ##########################
    
        # Obtain parameters and options for Log Service from environment variables.
        endpoint = os.environ.get('SLS_ENDPOINT', '')
        accessKeyId = os.environ.get('SLS_AK_ID', '')
        accessKey = os.environ.get('SLS_AK_KEY', '')
        project = os.environ.get('SLS_PROJECT', '')
        logstore = os.environ.get('SLS_LOGSTORE', '')
        consumer_group = os.environ.get('SLS_CG', '')
    
        # The starting point of data consumption. This parameter is valid when you run the program for the first time. When you run the program the next time, the consumption will continue from the latest consumption checkpoint.
        # You can use the BEGIN...END statement or a specific ISO time.
        cursor_start_time = "2018-12-26 0:0:0"
    
        ##########################
        # Advanced options
        ##########################
    
        # We do not recommend that you modify the consumer name, especially when concurrent consumption is required.
        consumer_name = "{0}-{1}".format(consumer_group, current_process().pid)
    
        # The heartbeat interval. If the server does not receive a heartbeat report for a specific shard within twice the specified interval, it indicates that the consumer is offline. In this case, the server will allocate the task to another consumer.
        # We recommend that you set a greater interval when the network performance is poor.
        heartbeat_interval = 20
    
        # The maximum interval between two data consumption processes. If data is generated at a fast speed, you do not need to adjust the setting of this parameter.
        data_fetch_interval = 1
    
        # Create a consumer group that contains the consumer.
        option = LogHubConfig(endpoint, accessKeyId, accessKey, project, logstore, consumer_group, consumer_name,
                              cursor_position=CursorPosition.SPECIAL_TIMER_CURSOR,
                              cursor_start_time=cursor_start_time,
                              heartbeat_interval=heartbeat_interval,
                              data_fetch_interval=data_fetch_interval)
    
        # Syslog options
        settings = {
                    "host": "1.2.3.4", # Required.
                    "Port": 514, # Required. The port number.
                    "protocol": "tcp", # Required. Valid values: tcp, udp, and tls. The tls value is only applicable to Python 3.
                    "sep": "||",       # Required. The separator that separates key-value pairs. In this example, the separator is two consecutive vertical bars (||).
                    "cert_path": None, # Optional. The location where the TLS certificate is stored.
                    "timeout": 120,    # Optional. The timeout period. The default value is 120 seconds.
                    "facility": syslogclient.FAC_USER,  # Optional. You can refer to values of the syslogclient.FAC_* parameter in other examples.
                    "severity": syslogclient.SEV_INFO,  # Optional. You can refer to values of other syslogclient.SEV_*.
                    "hostname": None,  # Optional. The hostname. The default value is the name of the local host.
                    "tag": None        # Optional. The tag. The default value is a hyphen (-).
                }
    
        return option, settings
    ```


## Example: Consume and send data

The following example shows how to collect data from Log Service and send the data to the Syslog server in the SIEM system. Read the code comments in the following example and configure parameters as required:

```
from syslogclient import SyslogClientRFC5424 as SyslogClient

class SyncData(ConsumerProcessorBase):
    """
    The consumer consumes data from Log Service and sends it to the Syslog server.
    """
    def __init__(self, splunk_setting):
      """Initiate the Syslog server and check its network connectivity."""
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
          # TODO: Modify the formatted content based on your business needs. The data is transmitted by using key-value pairs that are separated with two consecutive vertical bars (||).
                    for k, v in six.iteritems(log):
                        io.write("{0}{1}={2}".format(self.sep, k, v))

                    data = io.getvalue()

          # TODO: Modify the facility and severity settings based on your business needs.
                    client.log(data, facility=self.option.get("facility", None), severity=self.option.get("severity", None), timestamp=timestamp, program=self.option.get("tag", None), hostname=self.option.get("hostname", None))

        except Exception as err:
            logger.debug("Failed to connect to remote syslog server ({0}). Exception: {1}".format(self.option, err))

            # TODO: Add code to handle errors. For example, you can add the code to retry or send a notification in response to an error.
            raise err

        logger.info("Complete send data to remote")

        self.save_checkpoint(check_point_tracker)
```

## Example: Start the program

Assume that the program is named sync\_data.py. The following code shows how to start the program:

```
export SLS_ENDPOINT=<Endpoint of your region>
export SLS_AK_ID=<YOUR AK ID>
export SLS_AK_KEY=<YOUR AK KEY>
export SLS_PROJECT=<SLS Project Name>
export SLS_LOGSTORE=<SLS Logstore Name>
export SLS_CG=<Consumer group name. You can set it to "syc_data".>

python3 sync_data.py
```

## Limits

You can configure up to 10 consumer groups for each Logstore in Log Service. If the system displays the `ConsumerGroupQuotaExceed` error message, we recommend that you log on to the Log Service console to delete consumer groups that you no longer need.

## View and monitor data consumption

You can log on to the Log Service console to view the status of a consumer group. For more information, see [View consumer group status](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Consumption by consumer groups/Use consumer groups to consume log data.md).

## Concurrent consumption

You can start multiple consumer group-based programs for multiple consumers to consume data at the same time.

```
nohup python3 sync_data.py &
nohup python3 sync_data.py &
nohup python3 sync_data.py &
...
```

**Note:** The names of all consumers are unique within a consumer group because these names are suffixed with process IDs. The data of one shard can be consumed by only one consumer. If a Logstore contains 10 shards and each consumer group contains only one consumer, up to 10 consumer groups can consume the data of all shards at the same time.

## Throughput

In preceding examples, Python 3 is used to run the program without limits on the bandwidth or receiving speed, such as the receiving speed on Splunk. A single consumer consumes about 20% of single-core CPU resources. In this case, the consumption speed of raw logs can reach 10 MB/s. Therefore, if 10 consumers consume data at the same time, the consumption speed of raw logs can reach 100 MB/s per CPU core. Each CPU core can consume up to 0.9 TB of raw logs every day.

## High availability

A consumer group stores checkpoints on the server. When the data consumption process of one consumer stops, another consumer automatically takes over the process and continues the process from the checkpoint of the last consumption. You can start consumers on different servers. If a server stops or is damaged, a consumer on another server can take over the consumption process and continue the process from the checkpoint. To have sufficient consumers, you can start more consumers than the number of shards on different servers.

