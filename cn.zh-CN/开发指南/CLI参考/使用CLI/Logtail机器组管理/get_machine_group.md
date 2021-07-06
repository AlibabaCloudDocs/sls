# get\_machine\_group

调用CLI命令获取指定机器组信息。

## 请求语法

```
aliyunlog log get_machine_group --project_name=<value> --group_name=<value> [--access-id=<value>] [--access-key=<value>] [--sts-token=<value>] [--region-endpoint=<value>] [--client-name=<value>] [--jmes-filter=<value>] [--format-output=<value>] [--decode-output=<value>]
```

## 请求参数

该命令的必选和特有参数描述如下。

|参数名称|数值类型|是否必选|示例值|描述|
|----|----|----|---|--|
|--project\_name|String|是|aliyun-test-project|Project名称。|
|--group\_name|String|是|group\_name|机器组名称。|

关于该命令的全局参数，请参见[全局参数](/cn.zh-CN/开发指南/CLI参考/全局参数.md)。

## 示例

-   请求示例

    使用默认账号获取名称为group\_name的机器组信息。

    ```
    aliyunlog log get_machine_group --project_name="aliyun-test-project" --group_name="group_name"
    ```

-   返回示例

    ```
    {
      "createTime": 1622105480,
      "groupAttribute": {
        "externalName": "ex name",
        "groupTopic": "topic x"
      },
      "groupName": "group_name",
      "groupType": "",
      "lastModifyTime": 1622105480,
      "machineIdentifyType": "userdefined",
      "machineList": [
        "machine1",
        "machine2"
      ]
    }
    ```


## 错误码

如果返回报错信息，请参见具体接口的错误码处理。更多信息，请参见[GetMachineGroup错误码处理](/cn.zh-CN/开发指南/API参考/Logtail机器组相关接口/GetMachineGroup.md)。

## API参考

[GetMachineGroup](/cn.zh-CN/开发指南/API参考/Logtail机器组相关接口/GetMachineGroup.md)

