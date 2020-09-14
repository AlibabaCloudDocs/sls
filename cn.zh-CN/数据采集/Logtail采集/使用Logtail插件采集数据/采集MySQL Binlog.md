# 采集MySQL Binlog

本文介绍如何通过日志服务控制台创建Logtail采集配置来采集MySQL Binlog。

已在服务器上安装Logtail，详情请参见[安装Logtail（Linux系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)或[安装Logtail（Windows系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md)。

**说明：** 目前支持Linux Logtail 0.16.0及以上版本，Window Logtail 1.0.0.8及以上版本。

## 原理

Logtail内部实现了MySQL Slave节点的交互协议，具体流程如下所示。

1.  Logtail将自己伪装为MySQL Slave节点向MySQL master节点发送dump请求。
2.  MySQL master节点收到dump请求后，会将自身的Binlog实时发送给Logtail。
3.  Logtail对Binlog进行事件解析、过滤、数据解析等操作，并将解析好的数据上传到日志服务。

![实现原理](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0430559951/p2929.png)

## 功能特点

-   通过Binlog增量采集数据库的更新操作数据，性能优越。支持RDS等MySQL协议的数据库。
-   支持多种数据库过滤方式。
-   支持设置Binlog位点。
-   支持通过Checkpoint机制同步保存状态。

## 使用限制

-   不支持MySQL 8.0及以上版本。
-   MySQL必须开启Binlog，且Binlog必须为row模式（RDS默认已开启Binlog）。

    ```
    # 查看是否开启Binlog
    mysql> show variables like "log_bin";
    +---------------+-------+
    | Variable_name | Value |
    +---------------+-------+
    | log_bin       | ON    |
    +---------------+-------+
    1 row in set (0.02 sec)
    # 查看Binlog类型
    mysql> show variables like "binlog_format";
    +---------------+-------+
    | Variable_name | Value |
    +---------------+-------+
    | binlog_format | ROW   |
    +---------------+-------+
    1 row in set (0.03 sec)
    ```

-   ServerID唯一，即需要同步的MySQL的Slave ID唯一。
-   RDS限制
    -   无法直接在RDS服务器上安装Logtail，您需要将Logtail安装在能连通RDS实例的服务器上。
    -   RDS备库不支持Binlog采集，您需要配置RDS主库进行采集。

## 应用场景

适用于数据量较大且性能要求较高的数据同步场景。

-   增量订阅数据库改动进行实时查询与分析。
-   数据库操作审计。
-   使用日志服务对数据库更新信息进行自定义查询分析、可视化、对接下游流计算、导入MaxCompute离线计算、导入OSS长期存储等操作。

## 注意事项

建议您适当放开对Logtail的资源限制以应对流量突增等情况，避免Logtail因为资源超限被强制重启，对您的数据造成不必要的风险。

您可以通过/usr/local/ilogtail/ilogtail\_config.json文件修改相关参数，详情请参见[配置Logtail启动参数](/cn.zh-CN/数据采集/Logtail采集/安装/配置Logtail启动参数.md)。

如下示例表示将CPU的资源限制放宽到双核，将内存资源的限制放宽到2048MB。

```
{
    ...
    "cpu_usage_limit":2,
    "mem_usage_limit":2048,

    ...
}
```

## 数据可靠性

建议您启用MySQL服务器的全局事务ID（GTID）功能，并将Logtail升级到0.16.15及以上版本以保证数据可靠性，避免因主备切换造成的数据重复采集。

-   数据漏采集：Logtail与MySQL服务器之间的网络长时间中断时，可能会产生数据漏采集情况。

    如果Logtail和MySQL master节点之间的网络发生中断，MySQL master节点仍会不断地产生新的Binlog数据并且回收旧的Binlog数据。当网络恢复，Logtail与MySQL master节点重连成功后，Logtail会使用自身的checkpoint向MySQL master节点请求更多的Binlog数据。但由于长时间的网络中断，它所需要的数据很可能已经被回收，这时会触发Logtail的异常恢复机制。在异常恢复机制中，Logtail会从MySQL master节点获取最近的Binlog位置，以它为起点继续采集，这样就会跳过checkpoint和最近的Binlog位置之间的数据，导致数据漏采集。

-   数据重复采集：当MySQL master节点和slave节点之间的Binlog序号不同步时，发生了主备切换事件，可能会产生数据重复采集情况。

    在MySQL主备同步的设置下，MySQL master节点会将产生的Binlog同步给MySQL slave节点，MySQL slave节点收到后存储到本地的Binlog文件中。当MySQL master节点和slave节点之间的Binlog序号不同步时，发生了主备切换事件，以Binlog文件名和文件大小偏移量作为checkpoint的机制将导致数据重复采集。

    例如，有一段数据在MySQL master节点上位于`(binlog.100, 4)`到`(binlog.105, 4)`之间，而在MySQL slave节点上位于`(binlog.1000, 4)`到`(binlog.1005, 4)`之间，并且Logtail已经从MySQL master节点获取了这部分数据，将本地checkpoint更新到了`(binlog.105, 4)`。如果此时发生了主备切换且无任何异常发生，Logtail将会继续使用本地checkpoint`(binlog.105, 4)`去向新的MySQL master节点采集binlog。但是因为新的MySQL master上的`(binlog.1000, 4)`到`(binlog.1005, 4)`这部分数据的序号都大于Logtail所请求的序号，MySQL master将它们返回给Logtail，导致重复采集。


## 操作步骤

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在接入数据区域，选择**MySQL BinLog-插件**。

3.  在选择日志空间页签中，选择目标Project和Logstore，单击**下一步**。

    您也可以单击**立即创建**，重新创建Project和Logstore，详情请参见[步骤1：创建Project和Logstore](/cn.zh-CN/快速入门/快速入门.md)。

4.  在创建机器组页签中，创建机器组。

    -   如果您已有可用的机器组，请单击**使用现有机器组**。
    -   如果您还没有可用的机器组，请执行以下操作（以ECS为例）。
        1.  选择ECS实例安装Logtail，详情请参见[安装Logtail（ECS实例）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（ECS实例）.md)。

            如果已在ECS上安装Logtail，可直接单击**确认安装完毕**。

            **说明：** 如果是自建集群、其他云厂商服务器，需要手动安装Logtail，详情请参见[安装Logtail（Linux系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md#)或[安装Logtail（Windows系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md)。

        2.  安装完成后，单击**确认安装完毕**。
        3.  创建机器组，详情请参见[创建IP地址机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建IP地址机器组.md)或[创建用户自定义标识机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建用户自定义标识机器组.md)。
5.  在机器组配置页签中，应用机器组。

    选择一个机器组，将该机器组从**源机器组**移动到**应用机器组**。

6.  在数据源设置页签中，配置**配置名称**和**插件配置**。

    **插件配置**中已提供模板，包括inputs和processors，请根据您的需求替换配置参数。

    -   inputs为Logtail采集配置，必选项，请根据您的数据源配置。

        **说明：** 一个inputs中只允许配置一个类型的数据源。

    -   processors为Logtail处理配置，可选项。您可以配置一种或多种处理方式，详情请参见[处理数据](/cn.zh-CN/数据采集/Logtail采集/使用Logtail插件采集数据/处理数据.md)。
    ```
    {
     "inputs": [
         {
             "type": "service_canal",
             "detail": {
                 "Host": "************.mysql.rds.aliyuncs.com",
                 "Port": 3306,
                 "User" : "root",
                 "ServerID" : 56321,
                 "Password": "*******",
                 "IncludeTables": [
                     "user_info\\..*"
                 ],
                 "ExcludeTables": [
                     ".*\\.\\S+_inner"
                 ],
                 "TextToString" : true,
                 "EnableDDL" : true
             }
         }
     ]
    }
    ```

    |参数|类型|是否必须|说明|
    |:-|:-|:---|:-|
    |type|string|是|数据源类型，固定为service\_canal。|
    |Host|string|否|数据库主机。不配置时，默认为127.0.0.1。|
    |Port|int|否|数据库端口。不配置时，默认为3306。|
    |User|string|否|数据库用户名。不配置时，默认为root。 需保证配置的用户具有数据库读权限以及MySQL REPLICATION权限，示例如下。

    ```
CREATE USER canal IDENTIFIED BY 'canal';  
GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'canal'@'%';
-- GRANT ALL PRIVILEGES ON *.* TO 'canal'@'%' ;
FLUSH PRIVILEGES;
    ``` |
    |Password|string|否|数据库密码。不配置时，默认为空。 如果安全需求较高，建议将SQL访问用户名和密码配置为xxx，待配置同步至本地机器后，在本地文件/usr/local/ilogtail/user\_log\_config.json找到对应配置进行修改，详情请参见[修改本地配置](#section_h4m_4er_yl1)。

**说明：** 如果您在控制台上修改了此参数，同步至本地后会覆盖当前本地的配置。 |
    |ServerID|int|否|Logtail伪装成的Mysql Slave的ID。不配置时，默认为125。 **说明：** ServerID对于MySQL数据库必须唯一，否则会采集失败。 |
    |IncludeTables|string数组|是|包含的表名称（包括db，例如：test\_db.test\_table），可配置为正则表达式。如果某个表不符合IncludeTables中的任一条件则该表不会被采集。如果您希望采集所有表，请将此参数设置为.\*\\\\..\*。 **说明：** 如果需要完全匹配，请在前面加上^，后面加上$，例如：^test\_db\\\\.test\_table$。 |
    |ExcludeTables|string 数组|否|忽略的表名称（包括db，例如：test\_db.test\_table），可配置为正则表达式。如果某个表符合ExcludeTables中的任一条件则该表不会被采集。不设置时默认收集所有表。 **说明：** 如果需要完全匹配，请在前面加上^，后面加上$，例如：^test\_db\\\\.test\_table$。 |
    |StartBinName|string|否|首次采集的Binlog文件名。不设置时，默认从当前时间点开始采集。 如果想从指定位置开始采集，可以查看当前的Binlog文件以及文件大小偏移量，并将StartBinName、StartBinlogPos设置成对应的值，示例如下。

    ```
# StartBinName设置成 "mysql-bin.000063", StartBinlogPos设置成 0
mysql> show binary logs;
+------------------+-----------+
| Log_name         | File_size |
+------------------+-----------+
| mysql-bin.000063 |       241 |
| mysql-bin.000064 |       241 |
| mysql-bin.000065 |       241 |
| mysql-bin.000066 |     10778 |
+------------------+-----------+
4 rows in set (0.02 sec)
    ```

**说明：**

当指定StartBinName时，第一次采集会产生较大流量。 |
    |StartBinlogPos|int|否|首次采集的Binlog文件的偏移量，不设置时，默认为0。|
    |EnableGTID|bool|否|是否附加[全局事务ID](https://dev.mysql.com/doc/refman/5.7/en/replication-gtids-concepts.html)。不设置时，默认为true。设置为false时，表示上传的数据不附加全局事务ID。|
    |EnableInsert|bool|否|是否采集insert事件的数据。不设置时，默认为true。设置为false时，表示将不采集insert事件数据。|
    |EnableUpdate|bool|否|是否采集update事件的数据。不设置时，默认为true。设置为false时，表示不采集update事件数据。|
    |EnableDelete|bool|否|是否采集delete事件的数据。不设置时，默认为true。设置为false时，表示不采集delete事件数据。|
    |EnableDDL|bool|否|是否采集DDL（data definition language）事件数据。不设置时， 默认为false，表示不收集DDL事件数据。 **说明：** 该选项不支持IncludeTables和ExcludeTables过滤。 |
    |Charset|string|否|编码方式。不设置时，默认为utf-8。|
    |TextToString|bool|否|是否将text类型的数据转换成字符串。不设置时，默认为false，表示不转换。|
    |PackValues|bool|否|是否将事件数据打包成JSON格式。默认为false，表示不打包。如果设置为true，Logtail会将事件数据以JSON格式集中打包到data和old\_data两个字段中，其中old\_data仅在row\_update事件中有意义。 示例：假设数据表有三列数据c1，c2，c3，设置为false，row\_insert事件数据中会有c1，c2，c3三个字段，而设置为true时，c1，c2，c3会被统一打包为data字段，值为`{"c1":"...", "c2": "...", "c3": "..."}`。

**说明：** 该参数仅支持Logtail 0.16.19及以上版本。 |
    |EnableEventMeta|bool|否|是否采集事件的元数据，默认为false，表示不采集。 Binlog事件的元数据包括event\_time、event\_log\_position、event\_size和event\_server\_id。 **说明：** 该参数仅支持Logtail 0.16.21及以上版本。 |

7.  在**查询分析配置**页签中，设置索引。

    默认已设置索引，您也可以根据业务需求，重新设置索引，具体请参见[开启并配置索引](/cn.zh-CN/查询与分析/开启并配置索引.md)。

    **说明：**

    -   全文索引和字段索引属性必须至少启用一种。同时启用时，以字段索引属性为准。
    -   索引类型为long、double时，大小写敏感和分词符属性无效。

下发Logtail采集配置到服务器后，如果您的数据库存在相关的修改操作，Logtail会立即将修改的数据采集到日志服务。

**说明：** Logtail默认采集Binlog的增量数据。

## 修改本地配置

如果您没有在**插件配置**中输入真实的Host、User、Password等信息，可以在插件配置下发到本地后进行手动修改。

1.  登录Logtail所在服务器。

2.  打开/usr/local/ilogtail/user\_log\_config.json文件，找到service\_canal关键字，修改Host、User、Password等字段。

3.  执行以下命令重启Logtail。

    ```
    sudo /etc/init.d/ilogtaild stop; sudo /etc/init.d/ilogtaild start
    ```


Logtail采集Binlog到日志服务后，您可以在日志服务控制台上查看日志。例如：对`user_info`数据库下的`SpecialAlarm`表分别执行`INSERT`、`UPDATE`、`DELETE`操作，数据库表结构、数据库操作及日志样例如下所示。

-   表结构

    ```
    CREATE TABLE `SpecialAlarm` (
    `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
    `time` datetime NOT NULL,
    `alarmtype` varchar(64) NOT NULL,
    `ip` varchar(16) NOT NULL,
    `count` int(11) unsigned NOT NULL,
    PRIMARY KEY (`id`),
    KEY `time` (`time`) USING BTREE,
    KEY `alarmtype` (`alarmtype`) USING BTREE
    ) ENGINE=MyISAM AUTO_INCREMENT=1;
    ```

-   数据库操作

    执行INSERT、DELETE和UPDATE三种操作。

    ```
    insert into specialalarm (`time`, `alarmType`, `ip`, `count`) values(now(), "NO_ALARM", "10.10.**.***", 55);
    delete from specialalarm where id = 4829235  ;
    update specialalarm set ip = "10.11.***.**" where id = "4829234";
    ```

    为`zc.specialalarm`创建一个索引。

    ```
    ALTER TABLE `zc`.`specialalarm` 
    ADD INDEX `time_index` (`time` ASC);
    ```

-   日志样例

    在查询分析页面，查看每种操作对应的日志，日志样例如下所示。

    -   INSERT语句

        ```
        __source__:  10.30.**.**  
        __tag__:__hostname__:  iZbp145dd9fccu*****  
        __topic__:    
        _db_:  zc  
        _event_:  row_insert  
        _gtid_:  7d2ea78d-b631-11e7-8afb-00163e0eef52:536  
        _host_:  *********.mysql.rds.aliyuncs.com  
        _id_:  113  
        _table_:  specialalarm  
        alarmtype:  NO_ALARM  
        count:  55  
        id:  4829235  
        ip:  10.10.***.***  
        time:  2017-11-01 12:31:41
        ```

    -   DELETE语句

        ```
        __source__:  10.30.**.**  
        __tag__:__hostname__:  iZbp145dd9fccu****
        __topic__:    
        _db_:  zc  
        _event_:  row_delete  
        _gtid_:  7d2ea78d-b631-11e7-8afb-00163e0eef52:537  
        _host_:  *********.mysql.rds.aliyuncs.com  
        _id_:  114  
        _table_:  specialalarm  
        alarmtype:  NO_ALARM  
        count:  55  
        id:  4829235  
        ip:  10.10.**.***
        time:  2017-11-01 12:31:41
        ```

    -   UPDATE语句

        ```
        __source__:  10.30.**.**  
        __tag__:__hostname__:  iZbp145dd9fccu****  
        __topic__:    
        _db_:  zc  
        _event_:  row_update  
        _gtid_:  7d2ea78d-b631-11e7-8afb-00163e0eef52:538  
        _host_:  *********.mysql.rds.aliyuncs.com  
        _id_:  115  
        _old_alarmtype:  NO_ALARM  
        _old_count:  55  
        _old_id:  4829234  
        _old_ip:  10.10.22.133  
        _old_time:  2017-10-31 12:04:54  
        _table_:  specialalarm  
        alarmtype:  NO_ALARM  
        count:  55  
        id:  4829234  
        ip:  10.11.***.***
        time:  2017-10-31 12:04:54
        ```

    -   DDL（data definition language）语句

        ```
        __source__:  10.30.**.**  
        __tag__:__hostname__:  iZbp145dd9fccu****  
        __topic__:    
        _db_:  zc  
        _event_:  row_update  
        _gtid_:  7d2ea78d-b631-11e7-8afb-00163e0eef52:539  
        _host_:  *********.mysql.rds.aliyuncs.com  
        ErrorCode:  0
        ExecutionTime:  0
        Query:  ALTER TABLE `zc`.`specialalarm` 
        ADD INDEX `time_index` (`time` ASC)
        StatusVars:
        ```

    |字段|说明|
    |:-|:-|
    |`_host_`|数据库host名称。|
    |`_db_`|数据库名称。|
    |`_table_`|表的名称。|
    |`_event_`|事件类型。|
    |`_id_`|本次采集的自增ID，从0开始，每次采集一个binlog事件后加1。|
    |`_gtid_`|全局事务ID。|
    |`_filename_`|Binlog文件名。|
    |`_offset_`|Binlog文件大小偏移量，该值只会在每次commit后更新。|


