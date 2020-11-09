# 使用Kafka协议上传日志

您可以使用各类Kafka Producer SDK或采集工具来采集日志，并通过Kafka协议上传到日志服务。本文介绍通过Kafka协议将日志上传到日志服务的操作步骤。

## 相关限制

-   支持的Kafka协议版本为：0.8.0到2.1.1V2。
-   为保证日志传输安全性，必须使用SASL\_SSL连接协议。
-   如果您的Logstore有多个Shard，请使用负载均衡模式上传日志。
-   目前只支持将Kafka Producer SDK或采集Agent采集到的日志使用Kafka协议上传日志到日志服务。

## 配置方式

使用Kafka协议上传日志时，您需要配置以下参数。

|参数|说明|
|--|--|
|连接类型|为保证日志传输安全性，连接协议必须为SASL\_SSL。|
|hosts|初始连接的集群地址，格式为`project名称.Endpoint`，请根据Project所在的Endpoint进行配置。更多信息，请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。 -   阿里云内网：端口号为10011，例如test-project-1.cn-hangzhou-intranet.log.aliyuncs.com:10011。
-   公网：端口号为10012，例如test-project-1.cn-hangzhou.log.aliyuncs.com:10012。 |
|topic|配置为日志服务Logstore名称。|
|username|配置为日志服务Project名称。|
|password|配置为阿里云AK，格式为$\{access-key-id\}\#$\{access-key-secret\}。请根据实际情况，将$\{access-key-id\}替换为您的AccessKey ID，将$\{access-key-secret\}替换为您的AccessKey Secret。建议使用RAM用户的AK。更多信息，请参见[授权](/intl.zh-CN/开发指南/访问控制RAM/创建RAM用户及授权.md)。|
|证书文件|日志服务的域名均具备可信任证书，您只需使用服务器自带的根证书即可，例如：/etc/ssl/certs/ca-bundle.crt。|

## 示例1：通过Beats系列软件采集日志到日志服务

Beats系列软件（MetricBeat、PacketBeat、Winlogbeat、Auditbeat、Filebeat、Heartbeat等）采集到日志后，支持通过Kafka协议将日志上传到日志服务。更多信息，请参见[Beats-Kafka-Output](https://www.elastic.co/guide/en/beats/filebeat/master/kafka-output.html)。

-   配置示例

    ```
    output.kafka: 
      # initial brokers for reading cluster metadata 
      hosts: ["test-project-1.cn-hangzhou.log.aliyuncs.com:10012"] 
      username: "yourusername" 
      password: "yourpassword" 
      ssl.certificate_authorities: 
      # message topic selection + partitioning 
      topic: 'test-logstore-1' 
      partition.round_robin: 
        reachable_only: false 
    
      required_acks: 1 
      compression: gzip 
      max_message_bytes: 1000000
    ```

-   日志样例

    Beats系列软件默认输出的日志为JSON类型，您可以给content字段创建JSON类型的索引。更多信息，请参见[JSON类型](/intl.zh-CN/查询与分析/索引数据类型.md)。

    ![Beats系列软件](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3140559951/p41999.png)


## 示例2：通过Collectd采集日志到日志服务

[Collectd](https://collectd.org/)是一个守护（daemon）进程，用于定期采集系统和应用程序的性能指标，并支持通过Kafka协议上传到日志服务。更多信息，请参见[Write Kafka Plugin](https://collectd.org/wiki/index.php/Plugin:Write_Kafka)。

将Collectd采集到日志上传到日志服务时，还需安装Kafka插件以及相关依赖。例如：在linux Centos中，可以使用yum安装Kafka插件，命令为`sudo yum install collectd-write_kafka`，安装RPM请参见[Collectd-write\_kafka](https://rpmfind.net/linux/rpm2html/search.php?query=collectd-write_kafka)。

-   配置示例

    示例中将日志输出格式（Format）设置为JSON，除此之外，还支持Command、Graphite类型。更多信息，请参见[Collectd配置文档](https://collectd.org/documentation/manpages/collectd.conf.5.shtml)。

    ```
    <Plugin write_kafka>
      Property "metadata.broker.list" "test-project-1.cn-hangzhou.log.aliyuncs.com:10012" 
      Property "security.protocol" "sasl_ssl" 
      Property "sasl.mechanism" "PLAIN" 
      Property "sasl.username" "yourusername" 
      Property "sasl.password" "yourpassword" 
      Property "broker.address.family" "v4"  
      <Topic "test-logstore-1">
        Format JSON 
        Key "content"  
      </Topic>
    </Plugin>
                        
    ```

-   日志样例

    使用JSON模式输出日志后，您可以给content字段创建JSON类型的索引。更多信息，请参见[JSON类型](/intl.zh-CN/查询与分析/索引数据类型.md)。

    ![Collectd](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3140559951/p42000.png)


## 使用Telegraf采集日志到日志服务

[Telegraf](https://github.com/influxdata/telegraf)是由Go语言编写的代理程序，内存占用小，用于收集、处理、汇总数据指标。Telegraf具有丰富的插件及具备集成功能，可从其运行的系统中获取各种指标、从第三方API中获取指标以及通过statsd和Kafka消费者服务监听指标。

将Telegraf采集到的日志通过kafka协议上传到日志服务前，您需要先修改配置文件。

-   配置示例

    示例中将日志输出格式（Format）设置为JSON，除此之外还支持Graphite、Carbon2等类型。更多信息，请参见[Telegraf输出格式](https://github.com/influxdata/telegraf/blob/master/docs/DATA_FORMATS_OUTPUT.md)。

    **说明：** Telegraf必须配置一个合法的tls\_ca路径，使用服务器自带的根证书的路径即可。Linux环境中，根证书CA路径一般为/etc/ssl/certs/ca-bundle.crt。

    ```
    [[outputs.kafka]] 
      ## URLs of kafka brokers 
      brokers = ["test-project-1.cn-hangzhou.log.aliyuncs.com:10012"] 
      ## Kafka topic for producer messages 
      topic = "test-logstore-1" 
      routing_key = "content" 
      ## CompressionCodec represents the various compression codecs recognized by 
      ## Kafka in messages. 
      ## 0 : No compression 
      ## 1 : Gzip compression 
      ## 2 : Snappy compression 
      ## 3 : LZ4 compression 
      compression_codec = 1 
      ## Optional TLS Config tls_ca = "/etc/ssl/certs/ca-bundle.crt" 
      # tls_cert = "/etc/telegraf/cert.pem" # tls_key = "/etc/telegraf/key.pem" 
      ## Use TLS but skip chain & host verification 
      # insecure_skip_verify = false 
      ## Optional SASL Config 
      sasl_username = "yourusername" 
      sasl_password = "yourpassword" 
      ## Data format to output. 
      ## https://github.com/influxdata/telegraf/blob/master/docs/DATA_FORMATS_OUTPUT.md 
      data_format = "json"
    ```

-   日志样例

    使用JSON模式输出日志后，您可以给content字段创建JSON类型的索引。更多信息，请参见[JSON类型](/intl.zh-CN/查询与分析/索引数据类型.md)。

    ![Telegraf](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3140559951/p42180.png)


## 使用Fluentd采集日志到日志服务

[Fluentd](https://www.fluentd.org/)是一个开源的日志收集器，是云端原生计算基金会（CNCF）的成员项目之一，遵循Apache 2 License协议。

Fluentd支持众多输入、处理、输出插件，支持通过Kafka插件将日志上传到日志服务，您只需安装并配置Kafka插件即可。更多信息，请参见[fluent-plugin-kafka](https://github.com/fluent/fluent-plugin-kafka)。

-   配置示例

    示例中将日志输出格式（Format）设置为JSON，除此之外还支持数十种Format类型。更多信息，请参见[Fluentd Formatter](https://docs.fluentd.org)。

    ```
    <match **>
      @type kafka 
      # Brokers: you can choose either brokers or zookeeper. 
      brokers      test-project-1.cn-hangzhou.log.aliyuncs.com:10012 
      default_topic test-logstore-1 
      default_message_key content 
      output_data_type json 
      output_include_tag true 
      output_include_time true 
      sasl_over_ssl true 
      username yourusername  //请根据真实值，替换yourusername。
      password yourpassword   //请根据真实值，替换yourpassword。
      ssl_ca_certs_from_system true 
      # ruby-kafka producer options 
      max_send_retries 10000 
      required_acks 1 
      compression_codec gzip 
    </match>
    ```

-   日志样例

    使用JSON模式输出日志后，您可以给content字段创建JSON类型的索引。更多信息，请参见[JSON类型](/intl.zh-CN/查询与分析/索引数据类型.md)。

    ![Fluentd](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3140559951/p42204.png)


## 示例5：使用Logstash采集日志到日志服务

[Logstash](https://www.elastic.co/products/logstash)是一个具备实时处理能力、开源的日志采集引擎，可以动态采集不同来源的日志。

Logstash内置Kafka输出插件，您可以配置Logstash实现日志通过kafka协议上传到日志服务。由于日志服务使用SASL\_SSL连接协议，因此还需要配置SSL证书和jaas文件。

-   配置示例
    1.  创建jaas文件，并保存到任意路径（例如/etc/kafka/kafka\_client\_jaas.conf）。

        将如下内容添加到jaas文件中。

        ```
        KafkaClient { 
          org.apache.kafka.common.security.plain.PlainLoginModule required 
          username="yourusername" 
          password="yourpassword"; 
        };
        ```

    2.  配置SSL信任证书，保存到任意路径（例如：/etc/kafka/client-root.truststore.jks）。

        日志服务的域名均为可信任证书，您只需下载[GlobalSign Root CA](https://www.getssl.cn/support/globalsign-root-certificates/)根证书，保存base64编码的根证书到任意路径（例如/etc/kafka/ca-root）即可。然后输入[keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html)命令生成.jks格式的文件（首次生成时，需要配置密码）。

        ```
        keytool -keystore client.truststore.jks -alias root -import -file /etc/kafka/ca-root
        ```

    3.  配置Logstash。

        示例中将日志输出格式（Format）设置为JSON，除此之外还支持数十种Format类型。更多信息，请参见[Logstash Codec](https://www.elastic.co/guide/en/logstash/current/codec-plugins.html)。

        **说明：** 本示例为连通性测试的配置，您的生产环境中建议删除stdout的输出配置。

        ```
        input { stdin { } } 
        output { 
          stdout { codec => rubydebug } 
          kafka { 
            topic_id => "test-logstore-1" 
            bootstrap_servers => "test-project-1.cn-hangzhou.log.aliyuncs.com:10012" 
            security_protocol => "SASL_SSL" 
            ssl_truststore_location => "/etc/client-root.truststore.jks" 
            ssl_truststore_password => "123456" 
            jaas_path => "/etc/kafka_client_jaas.conf" 
            sasl_mechanism => "PLAIN" 
            codec => "json" 
            client_id => "kafka-logstash" 
          } 
        }
        ```

-   日志样例

    使用JSON模式输出日志后，您可以给content字段创建JSON类型的索引。更多信息，请参见[JSON类型](/intl.zh-CN/查询与分析/索引数据类型.md)。

    ![Logstash](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3140559951/p42205.png)


## 错误信息

使用Kafka协议上传日志失败时，会按照Kafka的错误信息返回对应的错误信息，如下表所示，Kafka协议错误信息详情请参见[error list](https://kafka.apache.org/20/javadoc/org/apache/kafka/common/errors/package-summary.html)。

|错误信息|说明|推荐解决方式|
|----|--|------|
|NetworkException|出现网络错误时返回该错误信息。|一般等待1秒后重试即可。|
|TopicAuthorizationException|鉴权失败时返回该错误信息。|一般是您提供的AccessKey错误或没有写入对应Project、Logstore的权限。请填写正确的且具备写入权限的AccessKey。|
|UnknownTopicOrPartitionException|出现该错误可能有两种原因： -   不存在对应的Project或Logstore。
-   Project所在地域与填入的Endpoint不一致。

|请确保已创建对应的Project和Logstore。如果已创建还是提示该错误，请检查Project所在地域是否和填入的Endpoint一致。 |
|KafkaStorageException|服务端出现异常时返回该错误信息。|一般等待1秒后重试即可。|

