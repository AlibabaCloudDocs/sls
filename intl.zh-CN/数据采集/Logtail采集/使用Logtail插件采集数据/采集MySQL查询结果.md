# 采集MySQL查询结果

本文介绍如何通过日志服务控制台创建Logtail采集配置来采集MySQL查询结果。

已在服务器上安装Logtail，详情请参见[安装Logtail（Linux系统）](/intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)或[安装Logtail（Windows系统）](/intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md)。

**说明：** 目前支持Linux Logtail 0.16.0及以上版本，Window Logtail 1.0.0.8及以上版本。

## 原理

Logtail根据Logtail采集配置定期执行指定的SELECT语句，将返回结果作为数据上传到日志服务。

Logtail获取到执行结果时，会将结果中配置的CheckPoint字段保存在本地，当下次执行SELECT语句时，会将上一次保存的CheckPoint带入到SELECT语句中，以此实现增量数据采集。

![实现原理](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2430559951/p2930.png)

## 功能

-   支持MySQL类型的数据库。
-   支持分页设置。
-   支持时区设置。
-   支持超时设置。
-   支持checkpoint状态保存。
-   支持SSL。
-   支持限制每次最大采集数量。

## 应用场景

-   根据自增ID或时间等标志采集增量数据。
-   根据筛选条件自定义同步。

## 操作步骤

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在接入数据区域，选择**MySQL查询结果-插件**。

3.  在选择日志空间页签中，选择目标Project和Logstore，单击**下一步**。

    您也可以单击**立即创建**，重新创建Project和Logstore，详情请参见[步骤1：创建Project和Logstore](/intl.zh-CN/快速入门/快速入门.md)。

4.  在创建机器组页签中，创建机器组。

    -   如果您已有可用的机器组，请单击**使用现有机器组**。
    -   如果您还没有可用的机器组，请执行以下操作（以ECS为例）。
        1.  选择ECS实例安装Logtail，详情请参见[安装Logtail（ECS实例）](/intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（ECS实例）.md)。

            如果已在ECS上安装Logtail，可直接单击**确认安装完毕**。

            **说明：** 如果是自建集群、其他云厂商服务器，需要手动安装Logtail，详情请参见[安装Logtail（Linux系统）](/intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md#)或[安装Logtail（Windows系统）](/intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md)。

        2.  安装完成后，单击**确认安装完毕**。
        3.  创建机器组，详情请参见[创建IP地址机器组](/intl.zh-CN/数据采集/Logtail采集/机器组/创建IP地址机器组.md)或[创建用户自定义标识机器组](/intl.zh-CN/数据采集/Logtail采集/机器组/创建用户自定义标识机器组.md)。
5.  在机器组配置页签中，应用机器组。

    选择一个机器组，将该机器组从**源机器组**移动到**应用机器组**。

6.  在数据源设置页签中，配置**配置名称**和**插件配置**。

    -   inputs为Logtail采集配置，必选项，请根据您的数据源配置。

        **说明：** 一个inputs中只允许配置一个类型的数据源。

    -   processors为Logtail处理配置，可选项。您可以配置一种或多种处理方式，详情请参见[处理数据](/intl.zh-CN/数据采集/Logtail采集/使用Logtail插件采集数据/处理数据.md)。
    ```
    {
      "inputs": [
        {
          "type": "service_mysql",
          "detail": {
            "Address": "************.mysql.rds.aliyuncs.com",
            "User": "****",
            "Password": "*******",
            "DataBase": "****",
            "Limit": true,
            "PageSize": 100,
            "StateMent": "select * from db.VersionOs where time > ?",
            "CheckPoint": true,
            "CheckPointColumn": "time",
            "CheckPointStart": "2018-01-01 00:00:00",
            "CheckPointSavePerPage": true,
            "CheckPointColumnType": "time",
            "IntervalMs": 60000
          }
        }
      ]
    }
    ```

    |参数|类型|是否必选|说明|
    |:-|:-|:---|:-|
    |type|string|是|数据源类型，固定为service\_mysql。|
    |Address|string|否|MySQL地址。不配置时，默认为127.0.0.1:3306。|
    |User|string|否|数据库用户名。不配置时，默认为root。|
    |Password|string|否|数据库密码。不配置时，默认为空。 如果安全需求较高，建议将SQL访问用户名和密码配置为xxx，待配置同步至本地机器后，在本地文件/usr/local/ilogtail/user\_log\_config.json找到对应配置进行修改，详情请参见[修改本地配置](#section_62v_7yx_0sh)。

**说明：** 如果您在控制台上修改了此参数，同步至本地后会覆盖当前本地的配置。 |
    |DialTimeOutMs|int|否|数据库连接超时时间，单位：ms。不配置时，默认为5000ms。|
    |ReadTimeOutMs|int|否|数据库读取超时时间，单位：ms。不配置时，默认为5000ms。|
    |StateMent|string|否|SQL语句。 设置CheckPoint为true时，StateMent中SQL语句的where条件中必须包含CheckPointColumn，并将该列的值配置为?。例如：CheckPointColumn配置为id，则StateMent配置为`SELECT * from ... where id > ?`。 |
    |Limit|bool|否|是否使用Limit分页。不配置时，默认为false，表示不使用Limit分页。 建议使用Limit进行分页。设置Limit为true后，进行SQL查询时，会自动在StateMent中追加LIMIT语句。 |
    |PageSize|int|否|分页大小，Limit为true时必须配置。|
    |MaxSyncSize|int|否|每次同步最大记录数。不配置时，默认为0，表示无限制。|
    |CheckPoint|bool|否|是否使用checkpoint。不配置时，默认为false，表示不使用checkpoint。|
    |CheckPointColumn|string|否|checkpoint列名称。 CheckPoint为true时必须配置。 |
    |CheckPointColumnType|string|否|checkpoint列类型，支持int和time两种类型。int类型的内部存储为int64，time类型支持MySQL的date、datetime、time。 CheckPoint为true时必须配置。 |
    |CheckPointStart|string|否|checkpoint初始值。 CheckPoint为true时必须配置。 |
    |CheckPointSavePerPage|bool|否|设置为true，则每次分页时保存一次checkpoint；设置为false，则每次同步完后保存checkpoint。|
    |IntervalMs|int|是|同步间隔，单位：ms。|

7.  在**查询分析配置**页签中，设置索引。

    默认已设置索引，您也可以根据业务需求，重新设置索引，具体请参见[开启并配置索引](/intl.zh-CN/查询与分析/开启并配置索引.md)。

    **说明：**

    -   全文索引和字段索引属性必须至少启用一种。同时启用时，以字段索引属性为准。
    -   索引类型为long、double时，大小写敏感和分词符属性无效。

## 修改本地配置

如果您没有在**插件配置**中输入真实的Address、User、Password等信息，可以在插件配置下发到本地后进行手动修改。

1.  登录Logtail所在服务器。

2.  打开/usr/local/ilogtail/user\_log\_config.json文件，找到service\_mysql关键字，修改Address、User、Password等字段。

3.  执行以下命令重启Logtail。

    ```
    sudo /etc/init.d/ilogtaild stop; sudo /etc/init.d/ilogtaild start
    ```


Logtail采集MySQL查询结果到日志服务后，您可以在日志服务控制台上查看日志。数据库表结构和Logtail采集到的日志样例如下所示。

-   表结构

    ```
    CREATE TABLE `VersionOs` (
      `id` int(11) unsigned NOT NULL AUTO_INCREMENT COMMENT 'id',
      `time` datetime NOT NULL,
      `version` varchar(10) NOT NULL DEFAULT '',
      `os` varchar(10) NOT NULL,
      `count` int(11) unsigned NOT NULL,
      PRIMARY KEY (`id`),
      KEY `timeindex` (`time`)
    )
    ```

-   日志样例

    ```
    "count":  "4"  
    "id:  "721097"  
    "os:  "Windows"  
    "time:  "2017-08-25 13:00:00"  
    "version":  "1.3.0"
    ```


