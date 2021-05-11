# PolarDB MySQL云原生数据库

本文介绍PolarDB MySQL云原生数据库审计日志、慢日志和性能日志的字段详情。

## 审计日志

|日志字段|说明|
|----|--|
|\_\_topic\_\_|日志主题，固定为polardb\_audit\_log。|
|owner\_id|阿里云账号ID|
|region|PolarDB MySQL集群所在地域|
|cluster\_id|PolarDB MySQL集群ID|
|node\_id|PolarDB MySQL节点ID|
|check\_rows|扫描的行数|
|db|数据库名称|
|fail|SQL执行是否出错。包括：-   0：成功
-   1：失败 |
|client\_ip|访问PolarDB MySQL集群的客户端IP地址|
|threat\_client\_ip|访问PolarDB MySQL集群的客户端IP地址的威胁情报。更多信息，请参见[威胁情报字段](/cn.zh-CN/应用中心（App）/日志审计服务/生成威胁情报.md)。|
|latency|执行SQL操作后，多久返回结果，单位：微秒。|
|origin\_time|执行操作的时间|
|return\_rows|返回的行数|
|sql|执行的SQL语句|
|thread\_id|线程ID|
|user|执行SQL的用户名|
|update\_rows|更新行数|

## 慢日志

|日志字段|说明|
|----|--|
|\_\_topic\_\_|日志主题，固定为rds\_slow\_log。|
|owner\_id|阿里云账号ID|
|region|PolarDB MySQL集群所在地域|
|cluster\_id|PolarDB MySQL集群ID|
|node\_id|PolarDB MySQL节点ID|
|db\_type|PolarDB数据库类型|
|db\_name|PolarDB数据库名|
|version|PolarDB数据库版本号|
|rows\_examined|扫描的行数|
|rows\_sent|返回的行数|
|start\_time|开始执行的时间|
|query\_time|执行的耗时，单位：秒。|
|lock\_time|锁等待的耗时，单位：秒。|
|user\_host|客户端信息|
|query\_sql|慢日志SQL语句|

## 性能日志

|指标名称|说明|
|----|--|
|mysql\_perf\_active\_session|每秒活跃连接数|
|mysql\_perf\_binlog\_size|本地Binlog使用量，单位：MB。|
|mysql\_perf\_com\_delete|每秒DELETE语句执行次数|
|mysql\_perf\_com\_delete\_multi|每秒Multi-DELETE语句执行次数|
|mysql\_perf\_com\_insert|每秒INSERT语句执行次数|
|mysql\_perf\_com\_insert\_select|每秒INSERT-SELECT语句执行次数|
|mysql\_perf\_com\_replace|每秒REPLACE语句执行次数|
|mysql\_perf\_com\_replace\_select|每秒REPLACE-SELECT语句执行次数|
|mysql\_perf\_com\_select|每秒SELECT语句执行次数|
|mysql\_perf\_com\_update|每秒UPDATE语句执行次数|
|mysql\_perf\_com\_update\_multi|每秒Multi-UPDATE语句执行次数|
|mysql\_perf\_cpu\_ratio|CPU使用率，单位：百分比。|
|mysql\_perf\_created\_tmp\_disk\_tables|每秒创建临时表个数|
|mysql\_perf\_data\_size|数据空间使用量，单位：MB。|
|mysql\_perf\_innodb\_buffer\_dirty\_ratio|缓冲池脏块率，单位：百分比。|
|mysql\_perf\_innodb\_buffer\_read\_hit|缓冲池读命中率，单位：百分比。|
|mysql\_perf\_innodb\_buffer\_use\_ratio|缓冲池使用率，单位：百分比。|
|mysql\_perf\_innodb\_data\_read|每秒从存储引擎读取数据量，单位：Byte。|
|mysql\_perf\_innodb\_data\_reads|每秒缓冲池读取次数|
|mysql\_perf\_innodb\_data\_writes|每秒缓冲池写次数|
|mysql\_perf\_innodb\_data\_written|每秒往存储引起写入数据量，单位：Byte。|
|mysql\_perf\_innodb\_log\_write\_requests|每秒日志写请求数|
|mysql\_perf\_innodb\_os\_log\_fsyncs|每秒向日志文件完成的fsync\(\)写数量|
|mysql\_perf\_innodb\_rows\_deleted|每秒删除的行数|
|mysql\_perf\_innodb\_rows\_inserted|每秒插入的行数|
|mysql\_perf\_innodb\_rows\_read|每秒读取的行数|
|mysql\_perf\_iops|每秒IOPS|
|mysql\_perf\_iops\_r|每秒读IOPS|
|mysql\_perf\_iops\_throughput|每秒总IO吞吐量，单位：MB。|
|mysql\_perf\_iops\_throughput\_r|每秒读IO吞吐量，单位：MB。|
|mysql\_perf\_iops\_throughput\_w|每秒写IO吞吐量，单位：MB。|
|mysql\_perf\_iops\_w|每秒写IOPS|
|mysql\_perf\_kbytes\_received|每秒输入流量，单位：KB。|
|mysql\_perf\_kbytes\_sent|每秒输出流量，单位：KB。|
|mysql\_perf\_mem\_ratio|内存使用率，单位：百分比。|
|mysql\_perf\_mps|每秒数据操作数|
|mysql\_perf\_other\_log\_size|其他日志使用量，单位：MB。|
|mysql\_perf\_qps|每秒请求数|
|mysql\_perf\_redolog\_size|本地Redolog使用量，单位：MB。|
|mysql\_perf\_slow\_queries|平均每秒慢查询数量|
|mysql\_perf\_sys\_dir\_size|系统空间使用量，单位：MB。|
|mysql\_perf\_tmp\_dir\_size|临时空间使用量，单位：MB。|
|mysql\_perf\_total\_session|当前平均总连接数|
|mysql\_perf\_tps|每秒事务数|

