# 配置CLI

配置CLI后，您无需在每次执行命令时指定所需的访问密钥、服务入口和输出格式等全局参数。本文介绍配置CLI的操作方法。

已安装CLI。更多信息，请参见[安装CLI](/cn.zh-CN/开发指南/命令行工具CLI/安装CLI.md)。

## 配置默认账号的服务入口和访问密钥

日志服务CLI默认使用配置的main账号执行所有操作，您必须在使用CLI前配置该账号的信息。

1.  登录安装CLI的服务器。

2.  配置默认账号的访问密钥和服务入口。

    执行命令如下：

    ```
    aliyunlog configure AccessKey ID AccessKey Secret Endpoint
    ```

    其中AccessKey ID、AccessKey Secret和 Endpoint替换为拥有操作日志服务权限的AccessKey ID、AccessKey Secret和Endpoint。如何获取，请参见[访问密钥](/cn.zh-CN/开发指南/API参考/访问密钥.md)和[服务入口](/cn.zh-CN/开发指南/API参考/服务入口.md)。

3.  验证配置结果。

    编辑.aliyunlogcli文件，如果配置文件中显示如下类似结果，则说明配置默认账号成功。

    ```
    [main]
    access-id = LTAI******pLMZ
    access-key = XjAsP******eRqax
    region-endpoint = cn-hangzhou.log.aliyuncs.com
    sts-token =
    ```

    **说明：** 配置文件.aliyunlogcli在不同系统其所在位置不同，您可以参考如下路径找到配置文件。

    -   Linux：~/.aliyunlogcli
    -   Windows：C:\\Users\\UserName\\.aliyunlogcli
    如果配置不成功，请根据返回错误码提示进行处理。


## 配置多个账号的服务入口和访问密钥

如果您需要跨账号操作日志数据，则需要配置多个账号信息。

1.  登录安装CLI的服务器。

2.  配置多个账号的访问密钥和服务入口。

    执行命令如下：

    ```
    aliyunlog configure AccessKey ID AccessKey Secret Endpoint testName
    ```

    -   AccessKey ID、AccessKey Secret和 Endpoint替换为拥有操作日志服务权限的AccessKey ID、AccessKey Secret和Endpoint。如何获取，请参见[访问密钥](/cn.zh-CN/开发指南/API参考/访问密钥.md)和[服务入口](/cn.zh-CN/开发指南/API参考/服务入口.md)。
    -   testName为您配置的其他账号名称。
3.  验证配置结果。

    编辑~/.aliyunlogcli文件，如果配置文件中显示如下类似结果，则说明配置默认账号成功。

    ```
    [main]
    access-id = LTAI******pLMZ
    access-key = XjAsP******eRqax
    region-endpoint = cn-hangzhou.log.aliyuncs.com
    sts-token =
    
    [test]
    access-id = As******sPzvb
    access-key = FtagJeR******bQqax
    region-endpoint = cn-shanghai.log.aliyuncs.com
    sts-token =
    ```

    **说明：** 配置文件.aliyunlogcli在不同系统其所在位置不同，您可以参考如下路径找到配置文件。

    -   Linux：~/.aliyunlogcli
    -   Windows：C:\\Users\\UserName\\.aliyunlogcli
    如果配置不成功，请根据返回错误码提示进行处理。

    在使用CLI执行命令时，您可以通过`--client-name=testName`方式来使用指定的账号。例如`aliyunlog log create_project ..... --client-name=test`，表示使用test账号创建Project。


## 配置输出格式

日志服务CLI支持对输出结果进行格式化和字符转义处理。当您需要对输出结果格式化、设置转义字符时，可参考如下配置。

-   JSON格式化

    日志服务CLI返回结果默认以JSON形式输出，并且显示为一行，可读性差。为便于查看，您可以使用如下方法对输出JSON结果进行格式化。

    -   对特定命令的输出结果进行格式化。

        例如，`aliyunlog log get_log .... --format-output=json`表示对get\_log的输出结果进行JSON格式化。

    -   对所有命令的输出结果进行格式化。

        直接执行`aliyunlog configure --format-output=json`，则表示对所有输出结果进行JSON格式化。

-   转义字符

    日志服务CLI返回结果中，非英文字符默认都是转义字符串。如果您需要返回原始字符（例如中文字符串），可以在`--format-output`添加`no_escape`。

    直接执行`aliyunlog configure --format-output=no_escape`，则日志服务CLI所有命令的输出结果都不转义，按照原始字符返回。


