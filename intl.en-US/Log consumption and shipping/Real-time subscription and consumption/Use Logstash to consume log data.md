# Use Logstash to consume log data

Log Service allows you to use Logstash to consume log data. You can configure the Logstash input plug-in to read log data from Log Service and write the data to other systems, such as Kafka and Hadoop Distributed File System \(HDFS\).

## Features

-   Distributed collaborative consumption: Multiple servers can be configured to consume log data from a Logstore at the same time.
-   High performance: If you use a Java consumer group, the consumption speed of a single-core CPU can reach up to 20 MB/s.
-   High reliability: Log Service saves consumption checkpoints. This mechanism resumes log consumption from the last checkpoint after a consumption exception is resolved.
-   Automatic load balancing: Shards are automatically allocated based on the number of consumers in a consumer group. If a consumer joins or leaves the consumer group, shards are automatically reallocated.

## Procedure

1.  Install Logstash.

    1.  [Download the installation package](https://www.elastic.co/downloads/logstash) of Logstash.

    2.  Decompress the package to a specified directory.

2.  Install the Logstash input plug-in.

    1.  Download the Logstash input plug-in. To download the plug-in, visit [logstash-input-sls](https://github.com/aliyun/logstash-input-logservice).

    2.  Install the Logstash input plug-in.

        ```
        logstash-plugin install logstash-input-sls
        ```

        **Note:** For information about the causes of installation failures and solutions, see [Plug-in installation and configuration](https://gems.ruby-china.com/).

3.  Start Logstash.

    ```
    logstash -f logstash.conf
    ```

    The following table describes the configuration parameters of the plug-in.

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |endpoint|String|Yes|The endpoint of the region where the Log Service project resides. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|
    |access\_id|String|Yes|The AccessKey ID of the Alibaba Cloud account or RAM user that is used to access the project. The Alibaba Cloud account or RAM user must be granted permissions to consume log data by using consumer groups. For more information, see [Permission to consume data of a specified Logstore](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md).|
    |access\_key|String|Yes|The AccessKey secret of the Alibaba Cloud account or RAM user that is used to access the project. The Alibaba Cloud account or RAM user must be granted permissions to consume log data by using consumer groups. For more information, see [Permission to consume data of a specified Logstore](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md).|
    |project|String|Yes|The name of the project in Log Service.|
    |logstore|String|Yes|The name of the Logstore in Log Service.|
    |consumer\_group|String|Yes|The name of the consumer group.|
    |consumer\_name|String|Yes|The name of the consumer, which must be unique in a consumer group.|
    |position|String|Yes|The position where data consumption starts. Valid values:     -   **begin**: consumes data starting from the first log entry that is written to the Logstore.
    -   **end**: consumes data starting from the current point in time.
    -   **yyyy-MM-dd HH:mm:ss**: consumes data starting from a specified point in time. |
    |checkpoint\_second|Number|No|The interval at which checkpoints are recorded. We recommend that you set the interval to a value between 10 seconds and 60 seconds. Minimum value: 10 seconds. Default value: 30 seconds.|
    |include\_meta|Boolean|No|Specifies whether input logs contain metadata, such as the log source, time, tags, and topic fields. Default value: true.|
    |consumer\_name\_with\_ip|Boolean|No|Specifies whether to include an IP address in a consumer name. Default value: true. You must set this parameter to true if distributed collaborative consumption is applied.|


## Examples

The following script shows how to configure Logstash to consume log data from a Logstore and print the data into stdout logs:

```
input {
  logservice{
  endpoint => "your project endpoint"
  access_id => "your access id"
  access_key => "your access key"
  project => "your project name"
  logstore => "your logstore name"
  consumer_group => "consumer group name"
  consumer_name => "consumer name"
  position => "end"
  checkpoint_second => 30
  include_meta => true
  consumer_name_with_ip => true
  }
}

output {
  stdout {}
}
```

