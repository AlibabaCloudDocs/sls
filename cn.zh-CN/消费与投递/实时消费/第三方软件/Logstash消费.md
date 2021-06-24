# Logstash消费

日志服务支持通过Logstash消费日志数据，您可以通过配置日志服务的Input插件对接Logstash获取日志服务中的日志数据并写入到其他系统中，例如Kafka、HDFS等。

## 功能特性

-   分布式协同消费：可配置多台服务器同时消费某一个Logstore。
-   高性能：基于Java ConsumerGroup实现，单核消费速度可达20 MB/s。
-   高可靠性：消费进度保存到服务端，异常恢复后会从上一次消费的Checkpoint处自动恢复消费。
-   自动负载均衡：根据消费者数量自动分配Shard，消费者增加或减少后会自动负载均衡。

## 操作步骤

1.  安装Logstash。

    1.  [下载安装包](https://www.elastic.co/cn/downloads/logstash)。

    2.  解压安装包到指定目录。

2.  安装input插件。

    1.  下载input插件。下载地址为[logstash-input-sls](https://github.com/aliyun/logstash-input-logservice)。

    2.  安装input插件。

        ```
        logstash-plugin install logstash-input-sls
        ```

        **说明：** 插件安装失败原因及解决方案，请参见[插件安装配置](https://gems.ruby-china.com/)。

3.  启动Logstash。

    ```
    logstash -f logstash.conf
    ```

    配置参数如下表所示。

    |参数|类型|是否必须|说明|
    |--|--|----|--|
    |endpoint|string|是|日志服务项目所在的endpoint，详情请参见[服务入口](/cn.zh-CN/开发指南/API参考/服务入口.md)。|
    |access\_id|string|是|阿里云Access Key ID，需要具备消费组相关权限，详情请参见[指定Logstore的消费权限](/cn.zh-CN/开发指南/访问控制RAM/RAM自定义授权场景.md)。|
    |access\_key|string|是|阿里云Access Key Secret，需要具备消费组相关权限，详情请参见[指定Logstore的消费权限](/cn.zh-CN/开发指南/访问控制RAM/RAM自定义授权场景.md)。|
    |project|string|是|日志服务Project名称。|
    |logstore|string|是|日志服务Logstore名称。|
    |consumer\_group|string|是|消费组名称。|
    |consumer\_name|string|是|消费者名称，同一个消费组中的消费者名称不能重复。|
    |position|string|是|消费开始位置。     -   **begin**：从Logstore写入的第一条数据开始消费。
    -   **end**：从当前时间点开始消费。
    -   **yyyy-MM-dd HH:mm:ss**：从指定时间点开始消费。 |
    |checkpoint\_second|number|否|每隔几秒Checkpoint一次，建议10秒~60秒，不能低于10秒，默认为30秒。|
    |include\_meta|boolean|否|日志数据是否包含Meta，Meta包括日志source、time、tag、topic，默认为true。|
    |consumer\_name\_with\_ip|boolean|否|消费者名称是否包含IP地址，默认为true，分布式协同消费下必须设置为true。|


## 示例

配置Logstash消费某一个Logstore并将日志打印到标准输出，示例如下：

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

