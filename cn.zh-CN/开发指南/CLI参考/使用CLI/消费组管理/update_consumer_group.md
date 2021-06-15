# update\_consumer\_group

调用CLI命令修改指定消费组信息。

## 请求语法

```
aliyunlog log update_consumer_group --project=<value> --logstore=<value> --consumer_group=<value> [--timeout=<value>] [--in_order=<value>] [--access-id=<value>] [--access-key=<value>] [--sts-token=<value>] [--region-endpoint=<value>] [--client-name=<value>] [--jmes-filter=<value>] [--format-output=<value>] [--decode-output=<value>
```

## 请求参数

该命令的必选和特有参数描述如下。

|参数名称|数值类型|是否必选|示例值|描述|
|----|----|----|---|--|
|--project|String|是|aliyun-test-project|Project名称。|
|--logstore|String|是|logstore-a|Logstore名称。|
|--consumer\_group|String|是|consumer-group-1|消费组名称。|
|--timeout|Integer|否|360|超时时间。单位：秒。消费者定期向日志服务的服务端发送心跳消息，用于建立联系。若服务端在超时时间段内没有收到消费者发送的心跳消息，则服务端删除该消费者占用相关资源。 |
|--in\_order|Boolean|否|true|在单个Shard中是否按顺序消费。 -   true：表示在单个Shard中按顺序消费。Shard分裂后，先消费原Shard数据，然后并列消费两个新Shard的数据。
-   false：表示不按顺序消费。 |

关于该命令的全局参数，请参见[全局参数](/cn.zh-CN/开发指南/CLI参考/全局参数.md)。

## 示例

-   请求示例

    使用默认账号修改消费组consumer-group-1的超时时间。

    ```
    aliyunlog log update_consumer_group --project="aliyun-test-project" --logstore="logstore-a" --consumer_group="consumer-group-1" --timeout="360" --in_order=true
    ```

-   返回示例

    命令执行成功后，无响应消息。


## 错误码

如果返回报错信息，请参见具体接口的错误码处理。更多信息，请参见[UpdateConsumerGroup错误码处理](/cn.zh-CN/开发指南/API参考/消费组接口/UpdateConsumerGroup.mdsection_z0z_c0n_4gb)。

## API参考

[UpdateConsumerGroup](/cn.zh-CN/开发指南/API参考/消费组接口/UpdateConsumerGroup.md)

