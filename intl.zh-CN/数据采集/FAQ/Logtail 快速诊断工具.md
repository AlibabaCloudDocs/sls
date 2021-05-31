# Logtail 快速诊断工具

当日志采集发生异常时，您可以通过Logtail自助检测工具查看客户端是否存在异常情况，根据工具提示快速定位并解决问题。

**说明：** 本工具目前仅支持Linux系统的服务器。

## 准备工作

1.  下载检测工具脚本。

    ```
    wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/checkingtool.sh -O checkingtool.sh
    ```

    **说明：** 如果无法正常下载，请通过以下备用地址重试。

    ```
    wget http://logtail-corp.oss-cn-hangzhou-zmf.aliyuncs.com/linux64/checkingtool.sh -O checkingtool.sh
    ```

2.  安装curl工具。

    检查工具需要使用curl进行网络连通性检查，请确保机器已安装curl工具。


## 运行诊断工具

1.  执行以下命令运行诊断工具：

    ```
    chmod 744 ./checkingtool.sh
    ./checkingtool.sh
    sh checkingtool.sh
    ```

    回显信息：

    ```
    [Info]:     Logtail checking tool version : 0.3.0
    [Input]:  please choose which item you want to check :
                 1. MachineGroup heartbeat fail.
                 2. MachineGroup heartbeat is ok, but log files have not been collected.
         Item :
    ```

2.  请根据提示输入`1`或`2`，脚本会根据您的选择执行不同检查流程。

    其中：

    -   `1`表示执行机器组心跳检查，机器组心跳失败时请选择此项。
    -   `2`表示执行日志采集检查，机器组心跳成功，但日志文件没有被采集时，请选择此项。
    选择检查项目后，诊断工具会自动执行对应检查流程。


## 诊断流程

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7044982951/p30599.png)

## 机器组心跳检查

选择机器组心跳检查流程后会进行下述一系列的检查：

1.  基础环境检查。

    -   是否安装Logtail。

    -   是否运行Logtail。

    -   SSL状态是否正常。

    -   与日志服务之间是否有网络联通。

    ```
    [Info]:     Logtail checking tool version : 0.3.0
    [Input]:  please choose which item you want to check :
                    1. MachineGroup heartbeat fail.
                    2. MachineGroup heartbeat is ok, but log files have not been collected.
            Item :1
    [Info]:     Check logtail install files
    [Info]:     Install file: ilogtail_config.json exists.                          [ OK ]
    [Info]:     Install file: /etc/init.d/ilogtaild exists.                         [ OK ]
    [Info]:     Install file: ilogtail exists.                                      [ OK ]
    [Info]:     Bin file: /usr/local/ilogtail/ilogtail_0.14.2 exists.               [ OK ]
    [Info]:     Logtail version :                                                   [ OK ]
    [Info]:     Check logtail running status
    [Info]:     Logtail is runnings.                                                [ OK ]
    [Info]:     Check network status
    [Info]:     Logtail is using ip: 11.XX.XX.187
    [Info]:     Logtail is using UUID: 0DF18E97-0F2D-486F-B77F-XXXXXXXXXXXX
    [Info]:     Check SSL status
    [Info]:     SSL status OK.                                                      [ OK ]
    [Info]:     Check logtail config server
    [Info]:     config server address: http://config.sls.aliyun-inc.com
    [Info]:     Logtail config server OK
    ```

    若其中检查出现`Error`信息，请参考提示进行处理。

2.  确认是否非本人ECS。

    基础环境检查通过后，请确认您的服务器是否为ECS、是否由本账号购买。

    若此服务器不是ECS或者ECS购买账号和日志服务账号不同，输入`y`，否则输入`N`。

    ```
    [Input]: Is your server non-Alibaba Cloud ECS or not belong to the same account with the current Project of Log Service ? (y/N)
    ```

    当输入`y`后，检查工具会输出本地配置的AliUid信息，请确认其中是否包含了您的AliUid，若未包含请参考文档[创建AliUid标识](/intl.zh-CN/数据采集/Logtail采集/机器组/配置用户标识.md)。

    ```
    [Input]:  Is your server non-Alibaba Cloud ECS or not belong to the same account with the current Project of Log Service ? (y/N)y
    [Info]:     Check aliyun user id(s)
    [Info]:     aliyun user id : 126XXXXXXXXXX79 .                                 [ OK ]
    [Info]:     aliyun user id : 165XXXXXXXXXX50 .                                 [ OK ]
    [Info]:     aliyun user id : 189XXXXXXXXXX57 .                                 [ OK ]
    [Input]:  Is your project owner account ID is the above IDs ? (y/N)
    ```

3.  检查Region。

    请确认您的Project所在区域是否和Logtail安装时所选区域一致，若不一致请[重新安装Logtail](/intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)。

    ```
    [Input]: please make sure your project is in this region : { cn-hangzhou } (y/N) :
    ```

4.  检查IP配置。

    请确认您机器组配置的IP和Logtail工作IP一致，若不一致请参考[IP地址机器组](/intl.zh-CN/数据采集/Logtail采集/机器组/创建IP地址机器组.md)修改。

    若您配置的是自定义标识机器组，请确认本地配置的标识与服务端配置一致，若不一致请参考[自定义标识机器组](/intl.zh-CN/数据采集/Logtail采集/机器组/创建用户自定义标识机器组.md)修改。

    ```
    [Input]:  please make sure your machine group's ip is same with : { 11.XX.XX.187 } or your machine group's userdefined-id is in : { XX-XXXXX } (y/N) :
    ```


## 日志采集检查

选择日志未采集检查流程后会进行下述一系列的检查：

1.  确认IP配置。

    请确认您机器组配置的ip和Logtail工作ip一致且心跳正常，若不一致请[修改机器组](/intl.zh-CN/数据采集/Logtail采集/机器组/管理机器组.md)。

    ```
    [Input]:  please make sure your machine group's ip is same with : { 11.XX.XX.187 } (y/N) :
    ```

2.  确认采集配置应用。

    请确认您的采集配置已经成功应用到该机器组中，如何查看机器组应用配置参见[管理机器组](/intl.zh-CN/数据采集/Logtail采集/机器组/管理机器组.md)。

    ```
    [Input]: please make sure you have applied collection config to the machine group (y/N) :Y
    ```

3.  检查日志文件。

    检查时请输入您需要检查的日志文件全路径，若未找到匹配项，请确认配置的路径信息可以匹配给定的日志文件。

    若配置错误请重新修改采集配置并保存，1分钟后再次执行此脚本重新检查。

    ```
    [Input]:  please input your log file's full path (eg. /var/log/nginx/access.log) :/disk2/logs/access.log
    [Info]:     Check specific log file
    [Info]:     Check if specific log file [ /disk2/logs/access.log ] is included by user config.
    [Warning]:  Specific log file doesnt exist.                                     [ Warning ]
    [Info]:     Matched config found:                                               [ OK ]
    [Info]:     [Project] -> sls-zc-xxxxxx
    [Info]:     [Logstore] -> release-xxxxxxx
    [Info]:     [LogPath] -> /disk2/logs
    [Info]:     [FilePattern] -> *.log
    ```


## 检查通过但采集依然异常

若所有的检查全部通过，但采集依然出现异常，请在脚本最后的选择中输入`y`并回车确认。

请您将检查脚本输出的信息作为附件，提交工单给我们的售后工程师。

```
[Input]: please make sure all the check items above have passed. If the problem persists, please copy all the outputs and submit a ticket in the ticket system. : (y/N)y
```

## 快速检查

快速检查运行时无需确认，可用于二次封装自定义检查脚本。

**说明：** 快速检查运行时会输出客户端配置的**阿里云ID**和**动态机器组/自定义标识**，不存在时并不会给出告警，如果客户端需要阿里云ID或动态机器组/自定义标识的配置，请查看工具的输出和您配置的是否一致，不一致时按照以下方法重新配置：[创建AliUid标识](/intl.zh-CN/数据采集/Logtail采集/机器组/配置用户标识.md)、[自定义标识机器组](/intl.zh-CN/数据采集/Logtail采集/机器组/创建用户自定义标识机器组.md)。

**操作步骤**

请运行脚本`./checkingtool.sh --logFile [LogFileFullPath]`进行检查。 检测脚本发现异常时，请根据脚本提示进行处理。

**说明：** 若指定日志文件检查通过且Logtail运行环境正常，建议进入阿里云控制台中查看该日志服务配置项的异常日志，参见[诊断采集错误](/intl.zh-CN/数据采集/FAQ/诊断采集错误.md)。

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7044982951/p30600.png)

## Logtail采集异常的常见问题

运行Logtail快速诊断工具后，可以诊断出Logtail采集异常的原因，您可以根据具体原因查找对应的解决方案。常见Logtail采集问题原因及解决方案如下。

|常见问题|解决方法|
|:---|:---|
|安装文件丢失|重装Logtail。|
|Logtail未运行|使用命令`/etc/init.d/ilogtaild start`开启Logtail。|
|多个Logtail进程|使用命令`/etc/init.d/ilogtaild stop`关闭Logtail，然后执行命令`/etc/init.d/ilogtaild start`开启。|
|443端口被禁用|防火墙开放443端口。|
|无法找到配置服务器|确认是否已正确[安装 Linux Logtail](/intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)，若安装错误，重新执行安装命令。|
|不存在用户配置|确认是否已执行以下操作： 1.  控制台已经创建好Logtail配置。
2.  机器组中包含该服务器。
3.  已经将配置应用到机器组。 |
|没有匹配指定日志文件|确认是否正确配置了Logtail。|
|指定日志文件匹配多次|匹配多次时Logtail会随机选择一个配置，建议去重。|

## 检测工具常用参数

|常用参数|说明|
|:---|:-|
|`--help`|查看帮助文档。|
|`--logFile [LogFileFullPath]`|检测Logtail是否收集路径为`LogFileFullPath`的日志，同时检查基本的Logtail运行环境（安装文件完整性、运行状态、阿里云userID、网络连通性等）。|
|`--logFileOnly [LogFileFullPath]`|只检测Logtail是否收集路径为`LogFileFullPath`的日志。|
|`--envOnly`|只检测Logtail运行环境。|

