# update\_machine\_group

调用CLI命令修改机器组信息。

## 请求语法

```
aliyunlog log update_machine_group --project_name=<value> --group_detail=<value> [--access-id=<value>] [--access-key=<value>] [--sts-token=<value>] [--region-endpoint=<value>] [--client-name=<value>] [--jmes-filter=<value>] [--format-output=<value>] [--decode-output=<value>]
```

## 请求参数

该命令的必选和特有参数描述如下。

|参数名称|数值类型|是否必选|示例值|描述|
|----|----|----|---|--|
|--project\_name|String|是|aliyun-test-project|Project名称。|
|--group\_detail|JSON Object|是|file://./machinegroup.json|修改后的机器组配置文件。|

关于该命令的全局参数，请参见[全局参数](/cn.zh-CN/开发指南/CLI参考/全局参数.md)。

## 示例

1.  修改machinegroup.json文件，其中机器组名称为group\_name2。其内容示例如下：

    ```
    {
      "machine_list": [
        "machine1",
        "machine2"
      ],
      "machine_type": "userdefined",
      "group_name": "group_name2",
      "group_type": "",
      "group_attribute": {
        "externalName": "ex name",
        "groupTopic": "topic x"
      }
    }
    ```

    各参数说明如下：

    |参数名称|说明|
    |----|--|
    |machine\_list|机器组的标识信息。    -   如果machine\_type配置为ip，则此处填写服务器的IP地址。
    -   如果machine\_type配置为userdefined，则此处填写自定义的标识。 |
    |machine\_type|机器标识类型。    -   ip：IP地址机器组。
    -   userdefined：用户自定义标识机器组。 |
    |group\_name|机器组名称。其命名规则如下：    -   同一个Project下，不可重复。
    -   只能包含小写字母、数字、短划线（-）和下划线（\_）。
    -   必须以小写字母或者数字开头和结尾。
    -   长度为3~128字符。 |
    |group\_type|机器组类型，可选值为空或者Armory。|
    |group\_attribute|机器组的属性。详细请参考下表group\_attribute参数说明。|

    其中group\_attribute参数说明如下表所示：

    |参数名称|说明|
    |----|--|
    |externalName|机器组所依赖的外部管理系统（Armory）标识。|
    |groupTopic|机器组的日志主题。|

2.  使用默认账号修改名称为group\_name2的机器组。

    ```
    aliyunlog log update_machine_group --project_name="aliyun-test-project" --group_detail="file://./machinegroup.json"
    ```

3.  查询已修改的机器组。命令示例如下：

    ```
    aliyunlog log get_machine_group --project_name="aliyun-test-project" --group_name="group_name2"
    ```

    返回结果如下：

    ```
    {
      "createTime": 1622105480,
      "groupAttribute": {
        "externalName": "ex name",
        "groupTopic": "topic x"
      },
      "groupName": "group_name2",
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

如果返回报错信息，请参见具体接口的错误码处理。更多信息，请参见[UpdateMachineGroup错误码处理](/cn.zh-CN/开发指南/API参考/Logtail机器组相关接口/UpdateMachineGroup.md)。

## API参考

[UpdateMachineGroup](/cn.zh-CN/开发指南/API参考/Logtail机器组相关接口/UpdateMachineGroup.md)

