# Use the Kafka protocol to upload logs

You can use Kafka Producer SDKs or collection agents to collect logs, and then upload the logs to Log Service by using the Kafka protocol. This topic describes how to upload logs to Log Service by using the Kafka protocol.

## Limits

-   Only Kafka 0.8.0 to Kafka 2.1.1 \(message format version 2\) are supported.
-   You must use the SASL\_SSL protocol to ensure the security of log transfer.
-   If your Logstore contains multiple shards, you must upload log data in load balancing mode.
-   You can use only the Kafka protocol to upload logs that are collected by using Kafka Producer SDKs or collection agents.

## Configurations

You must specify relevant parameters when you use the Kafka protocol to upload logs to Log Service. The following table describes the required parameters.

|Parameter|Description|
|---------|-----------|
|Connection protocol|Set the value to SASL\_SSL.|
|hosts|The endpoint of the Log Service project. The format is `project name.endpoint`. Specify an endpoint based on the region where your Log Service project resides. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). -   Alibaba Cloud internal network: The port number is 10011. Example: test-project-1.cn-hangzhou-intranet.log.aliyuncs.com:10011.
-   Internet: The port number is 10012. Example: test-project-1.cn-hangzhou.log.aliyuncs.com:10012. |
|topic|The name of the Logstore.|
|username|The name of the project.|
|password|The AccessKey pair. The format is $\{access-key-id\}\#$\{access-key-secret\}. Enter your AccessKey ID in $\{access-key-id\} and your AccessKey secret in $\{access-key-secret\}. We recommend that you use the AccessKey pair of your RAM user. For more information, see [Authorize a RAM user to connect to Log Service](/intl.en-US/Developer Guide/Access control RAM/Create a RAM user and authorize the RAM user to access Log Service.md).|
|Certificate file|The domain name of each Log Service project has a CA certificate. You only need to download the root certificate and save the certificate to a directory, for example, /etc/ssl/certs/ca-bundle.crt.|

## Example 1: Use Beats to upload logs to Log Service

You can use Beats such as MetricBeat, PacketBeat, Winlogbeat, Auditbeat, Filebeat, and Heartbeat to collect logs. After the logs are collected, you can use the Kafka protocol to upload the logs to Log Service. For more information, visit [Beats-Kafka-Output](https://www.elastic.co/guide/en/beats/filebeat/master/kafka-output.html).

-   Configuration example

    ```
    output.kafka: 
      # initial brokers for reading cluster metadata 
      hosts: ["test-project-1.cn-hangzhou.log.aliyuncs.com:10012"] 
      username: "<yourusername>" 
      password: "<yourpassword>" 
      ssl.certificate_authorities: 
      # message topic selection + partitioning 
      topic: 'test-logstore-1' 
      partition.round_robin: 
        reachable_only: false 
    
      required_acks: 1 
      compression: gzip 
      max_message_bytes: 1000000
    ```

-   Sample log entry

    By default, Beats provide JSON-formatted logs in the content field. You can create a JSON index for the content field. For more information, visit [JSON](/intl.en-US/Index and query/Data types of indexes.md).

    ![Beats](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4407549951/p41999.png)


## Example 2: Use collectd to upload logs to Log Service

[collectd](https://collectd.org/) is a daemon that is used to collect the performance metrics of systems and applications at a regular interval. The collectd daemon allows you to upload logs to Log Service by using the Kafka protocol. For more information, visit [Write Kafka Plug-in](https://collectd.org/wiki/index.php/Plugin:Write_Kafka).

Before you upload logs to Log Service, you must install the collectd-write\_kafka plug-in and relevant dependencies. If you are using CentOS, you can run the `sudo yum install collectd-write_kafka` command to install the collectd-write\_kafka plug-in. For more information, visit [RPM resource collectd-write\_kafka](https://rpmfind.net/linux/rpm2html/search.php?query=collectd-write_kafka).

-   Configuration example

    In the following configuration example, the output format of logs is set to JSON. You can also set the output format to Command and Graphite. For more information, visit [Manpage collectd.conf](https://collectd.org/documentation/manpages/collectd.conf.5.shtml).

    ```
    <Plugin write_kafka>
      Property "metadata.broker.list" "test-project-1.cn-hangzhou.log.aliyuncs.com:10012" 
      Property "security.protocol" "sasl_ssl" 
      Property "sasl.mechanism" "PLAIN" 
      Property "sasl.username" "<yourusername>" 
      Property "sasl.password" "<yourpassword>" 
      Property "broker.address.family" "v4"  
      <Topic "test-logstore-1">
        Format JSON 
        Key "content"  
      </Topic>
    </Plugin>
                        
    ```

-   Sample log entry

    Logs are sent to the Log Service console in the JSON format. The log content is included in the content field. You can create a JSON index for the content field. For more information, see [JSON](/intl.en-US/Index and query/Data types of indexes.md).

    ![Collectd](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4407549951/p42000.png)


## Example 3: Use Telegraf to upload logs to Log Service

[Telegraf](https://github.com/influxdata/telegraf) is an agent in the Go programming language and is used to collect, process, and analyze metrics. It consumes only a small number of memory resources. In addition, Telegraf supports integration with multiple plug-ins. You can use Telegraf to retrieve metrics from the system where it runs, or from third-party APIs. You can also use Telegraf to monitor metrics by using StatsD and Kafka consumers.

Before you use the Kafka protocol to upload collected logs to Log Service, you must modify the configuration file of Telegraf.

-   Configuration example

    In the following configuration example, the output format of logs is set to JSON. You can also set the output format to Graphite and Carbon2. For more information, visit [Telegraf output formats](https://github.com/influxdata/telegraf/blob/master/docs/DATA_FORMATS_OUTPUT.md).

    **Note:** You must set a valid tls\_ca path for Telegraf. You can use the default directory of the root certificate of the server. In a Linux-based server, the default directory of the root certificate is /etc/ssl/certs/ca-bundle.crt.

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
      sasl_username = "<yourusername>" 
      sasl_password = "<yourpassword>" 
      ## Data format to output. 
      ## https://github.com/influxdata/telegraf/blob/master/docs/DATA_FORMATS_OUTPUT.md 
      data_format = "json"
    ```

-   Sample log entry

    Logs are sent to the Log Service console in the JSON format. The log content is included in the content field. You can create a JSON index for the content field. For more information, see [JSON](/intl.en-US/Index and query/Data types of indexes.md).

    ![Telegraf](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4407549951/p42180.png)


## Example 4: Use Fluentd to upload logs to Log Service

[Fluentd](https://www.fluentd.org/) is an open source log data collector. It is a Cloud Native Computing Foundation \(CNCF\) project and complies with the Apache 2 License protocol.

Fluentd is compatible with multiple input plug-ins, processing plug-ins, and output plug-ins. You can use the fluent-plugin-kafka plug-in to upload logs to Log Service. For more information about how to install and configure this plug-in, visit [fluent-plugin-kafka](https://github.com/fluent/fluent-plugin-kafka).

-   Configuration example

    In the following configuration example, the output format of logs is set to JSON. Fluentd also supports other output formats. For more information, visit [Fluentd Formatter](https://docs.fluentd.org).

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
      username <yourusername> 
      password <yourpassword> 
      ssl_ca_certs_from_system true 
      # ruby-kafka producer options 
      max_send_retries 10000 
      required_acks 1 
      compression_codec gzip 
    </match>
    ```

-   Sample log entry

    Logs are sent to the Log Service console in the JSON format. The log content is included in the content field. You can create a JSON index for the content field. For more information, see [JSON](/intl.en-US/Index and query/Data types of indexes.md).

    ![Fluentd](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4407549951/p42204.png)


## Example 5: Use Logstash to upload logs to Log Service

[Logstash](https://www.elastic.co/products/logstash) is an open source log collection engine that provides real-time processing capabilities. You can use Logstash to dynamically collect logs from different sources.

Logstash provides a built-in Kafka output plug-in. You can configure Logstash to upload logs to Log Service by using the Kafka protocol. Log Service uses the SASL\_SSL protocol during data transfer. You must configure the SSL certificate and Java Authentication and Authorization Service \(JAAS\) file.

-   Configuration example
    1.  Create a JAAS file and save the file to a directory, for example, /etc/kafka/kafka\_client\_jaas.conf.

        Add the following content to the JAAS file:

        ```
        KafkaClient { 
          org.apache.kafka.common.security.plain.PlainLoginModule required 
          username="<yourusername>" 
          password="<yourpassword>"; 
        };
        ```

    2.  Configure the SSL certificate and save the certificate to a directory, for example, /etc/kafka/client-root.truststore.jks.

        The domain name of each Log Service project has a CA certificate. You only need to download the root certificate [GlobalSign Root CA](https://www.getssl.cn/support/globalsign-root-certificates/), encode the certificate in Base64, and save the certificate to a directory, for example, /etc/kafka/ca-root. Then, run the following [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) command to generate a JKS file. You must set a password when a JKS file is generated for the first time.

        ```
        keytool -keystore client.truststore.jks -alias root -import -file /etc/kafka/ca-root
        ```

    3.  Configure Logstash.

        In the following configuration example, the output format of logs is set to JSON. Logstash also supports other output formats. For more information, visit [Logstash Codec](https://www.elastic.co/guide/en/logstash/current/codec-plugins.html).

        **Note:** The following configurations are used for a connectivity test. In a production environment, we recommend that you remove the stdout field.

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

-   Sample log entry

    Logs are sent to the Log Service console in the JSON format. The log content is included in the content field. You can create a JSON index for the content field. For more information, see [JSON](/intl.en-US/Index and query/Data types of indexes.md).

    ![Logstash](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5407549951/p42205.png)


## Error messages

If a log fails to be uploaded by using the Kafka protocol, an error message is returned. The following table describes the error messages. For more information, see [Exception summary](https://kafka.apache.org/20/javadoc/org/apache/kafka/common/errors/package-summary.html).

|Error message|Description|Solution|
|-------------|-----------|--------|
|NetworkException|The error message is returned because a network exception has occurred.|Try again after 1 second.|
|TopicAuthorizationException|The error message is returned because the authentication has failed.|This is because your AccessKey pair is invalid or you are not authorized to write data to the destination project or Logstore. Enter a valid AccessKey pair and make sure that it has the required write permissions.|
|UnknownTopicOrPartitionException|The error message is returned because one of the following errors has occurred: -   The destination project or Logstore does not exist.
-   The region of the project is different from the region of the specified endpoint.

|Create a project and a Logstore in advance. Make sure that the region of the project is the same as the region of the specified endpoint. |
|KafkaStorageException|The error message is returned because a server error has occurred.|Try again after 1 second.|

