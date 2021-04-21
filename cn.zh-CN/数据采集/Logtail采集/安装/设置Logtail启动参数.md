# 设置Logtail启动参数

为防止Logtail消耗过多服务器资源，影响其他服务运行，日志服务对Logtail采集性能做了限制。当您需要提升Logtail采集性能时，可修改Logtail启动参数。

## 设置场景

遇到以下场景时，可修改Logtail启动参数。

-   需要采集的日志文件数目大（同时采集的文件数超过100个或所监控的目录下的文件数超过5000个），占用大量内存。
-   日志数据流量大（例如极简模式下超过2 MB/s，正则模式下超过1 MB/s），导致CPU占用率高。
-   Logtail发送数据到日志服务的速率超过20 MB/s。

## 设置启动参数

1.  在安装Logtail的服务器上，打开/usr/local/ilogtail/ilogtail\_config.json文件。

2.  根据需求设置启动参数。

    启动参数示例如下：

    ```
    {
        ...
        "cpu_usage_limit" : 0.4,
        "mem_usage_limit" : 100,
        "max_bytes_per_sec" : 2097152,
        "process_thread_count" : 1,
        "send_request_concurrency" : 4,
        "buffer_file_num" : 25,
        "buffer_file_size" : 20971520,
        "buffer_file_path" : "",
        ...
    }
    ```

    **说明：**

    -   下表中只列出您需要关注的常用启动参数，未列出的启动参数，保持默认配置即可。
    -   您可以根据需要新增或修改指定启动参数。
    Logtail启动参数说明如下表所示：

    |参数|类型|说明|示例|
    |:-|--|:-|--|
    |cpu\_usage\_limit|double|CPU使用阈值，以单核计算。取值如下：    -   取值范围：0.1~当前机器的CPU核心数
    -   默认值：2
例如设置为0.4，表示日志服务将尽可能限制Logtail的CPU使用为CPU单核的40%，超出后Logtail自动重启。

一般情况下，通过极简模式采集日志时，单核处理能力约24 MB/s；通过完整正则模式采集日志时，单核处理能力约12 MB/s 。

|"cpu\_usage\_limit" : 0.4|
    |mem\_usage\_limit|int|内存使用阈值。取值如下：    -   取值范围：128（MB）~当前机器有效内存值
    -   默认值：2048（MB）
例如设置为100，表示日志服务将尽可能限制Logtail的内存为100 MB，超出后Logtail自动重启。

如果需要同时采集的文件数目超过1000，可根据需求修改此参数。

|"mem\_usage\_limit" : 100|
    |max\_bytes\_per\_sec|int|每秒钟Logtail发送原始数据的流量限制。取值如下：    -   取值范围：1024（Byte/s）~52428800（Byte/s）
    -   默认值：20971520（Byte/s）
例如设置为2097152，表示Logtail发送数据的速率为2 MB/s。

**说明：** 设置的值超过20971520 Byte/s（20MB/s），表示不限速。

|"max\_bytes\_per\_sec" : 2097152|
    |process\_thread\_count|int|Logtail处理数据的线程数。 取值如下：    -   取值范围：1~64
    -   默认值：1
一般情况下，可以处理极简模式下24 MB/s的数据写入或完整正则模式12 MB/s的数据写入。默认情况下无需调整该参数取值。

|"process\_thread\_count" : 1|
    |send\_request\_concurrency|int|异步并发的个数。取值如下：    -   取值范围：1~1000
    -   默认值：20
如果写入TPS很高，可以设置更高的异步并发个数。可以按照一个并发支持0.5 MB/s~1 MB/s网络吞吐来计算，实际根据网络延时而定。

**说明：** 设置异步并发个数过高容易导致网络端口占用过多，需调整TCP相关参数。更多信息，请参见[调整TCP相关参数](https://yq.aliyun.com/articles/52884)。

|"send\_request\_concurrency" : 4|
    |buffer\_file\_num|int|限制缓存文件的最大数目。取值如下：    -   取值范围：1~100
    -   默认值：25
遇到网络异常、写入配额超限等情况时，Logtail将实时解析后的日志写入本地文件（安装目录下）缓存起来，等待恢复后尝试重新发送。

|buffer\_file\_num" : 25|
    |buffer\_file\_size|int|单个缓存文件允许的最大字节数。取值如下：    -   取值范围：1048576（Byte）~104857600（Byte）
    -   默认值：20971520（Byte）
buffer\_file\_size\*buffer\_file\_num是缓存文件可以实际使用的最大磁盘空间。

|"buffer\_file\_size" : 20971520|
    |buffer\_file\_path|String|缓存文件存放目录。 默认值为空，即缓存文件存放于logtail安装目录/usr/local/ilogtail下。 当您设置此参数后，需手动将原目录下名为 logtail\\\_buffer\\\_file\_\*的文件移动到此目录，以保证Logtail可以读取到该缓存文件并在发送后进行删除。

|"buffer\_file\_path" : ""|
    |bind\_interface|String|本机绑定的网卡名。默认值为空，自动绑定可用的网卡。 如果设置为指定的网卡（例如eth1），则表示Logtail将强制使用该网卡上传日志。

**说明：** 只支持Linux版本。

|"bind\_interface" : ""|
    |check\_point\_filename|String|Logtail的checkpoint文件的保存路径， 默认为/tmp/logtail\_check\_point。 建议Docker用户修改checkpoint文件保存路径，并将宿主机挂载此路径，否则容器释放时会因丢失checkpoint信息而造成重复采集。例如在Docker中设置check\_point\_filename为`/data/logtail/check_point.dat`，在Docker启动命令中增加`-v /data/docker1/logtail:/data/logtail`， 将宿主机/data/docker1/logtail目录挂载到Docker中的/data/logtail目录。

|"check\_point\_filename" : /tmp/logtail\_check\_point|
    |user\_config\_file\_path|String|Logtail配置文件的保存路径，默认为进程binary所在目录，文件名为user\_log\_config.json。 建议Docker用户修改Logtail配置文件保存路径，并将宿主机挂载此路径，否则容器释放时会因丢失checkpoint信息而造成重复采集。例如Docker中设置user\_config\_file\_path为/data/logtail/user\_log\_config.json，在Docker启动命令中增加`-v /data/docker1/logtail:/data/logtail`， 将宿主机/data/docker1/logtail目录挂载到Docker中的/data/logtail目录。

|"user\_config\_file\_path" : user\_log\_config.json|
    |discard\_old\_data|Boolean|是否丢弃历史日志。默认值：true，表示丢弃距离当前时间超过12小时的日志。|"discard\_old\_data" : true|
    |working\_ip|String|Logtail上报本服务器的IP地址。默认值为空，表示自动从本服务器获取IP地址。|"working\_ip" : ""|
    |working\_hostname|String|Logtail上报的本服务器的主机名。默认值为空，表示自动从本服务器获取主机名。|"working\_hostname" : ""|
    |max\_read\_buffer\_size|long|每条日志读取的最大值。默认值：524288，单位：Byte。如果您的单条日志超过524288 Byte（512 KB），可修改此参数。

|"max\_read\_buffer\_size" : 524288|
    |oas\_connect\_timeout|long|Logtail发起获取Logtail配置、访问密钥等请求时，连接阶段的超时时间。默认值：5，单位：秒。 网络条件较差，建立连接时间过长时可修改此参数。

|"oas\_connect\_timeout" : 5|
    |oas\_request\_timeout|long|Logtail发起获取Logtail配置、访问密钥等请求时，整个请求阶段的超时时间。默认值：10，单位：秒。 网络条件较差，建立连接时间过长时可修改此参数。

|"" : 10|
    |data\_server\_port|long|设置data\_server\_port为true后，Logtail将通过HTTPS协议传输数据到日志服务。仅支持Logtail 1.0.10及以上版本。

|"data\_server\_port": 443|
    |enable\_log\_time\_auto\_adjust|Boolean|设置enable\_log\_time\_auto\_adjust为true后，日志时间可自适应服务器本地时间。出于数据安全考虑，日志服务会对请求（包括Logtail发起的请求）所携带的时间进行校验，拒绝与日志服务端时间相差超过15分钟的请求。Logtail发起请求时所携带的时间为服务器本地时间，当服务器本地时间被修改后（例如某些测试场景下需要调整本地时间为未来时间），Logtail请求将被拒绝，导致写入数据失败。您可以使用该参数实现日志时间自适应服务器本地时间。

仅支持Logtail 1.0.19及以上版本。

**说明：**

    -   开启该功能后，日志时间将被加上日志服务端的时间与服务器本地时间的偏移量。由于偏移量只在请求被日志服务端拒绝时更新，因此可能出现日志服务端所查询到的日志的时间和日志实际的写入时间不一致的情况。
    -   Logtail的部分逻辑依赖于系统时间的递增，建议在每次机器时间调整后重启Logtail。
|"enable\_log\_time\_auto\_adjust": true|
    |accept\_multi\_config|Boolean|是否允许多个Logtail配置采集同一个文件。默认值：false，表示不允许。默认情况下，一个文件只能被一个Logtai配置采集，您可以通过该参数消除限制。每个Logtail配置的处理过程是独立的，当允许多个Logtai配置采集同一个文件时，需要消耗多倍的CPU、内存开销。

仅支持Logtail 0.16.26 及以上版本。

|"accept\_multi\_config": true|

3.  重启Logtail使配置生效。

    ```
    /etc/init.d/ilogtaild stop && /etc/init.d/ilogtaild start                        
    ```

    重启后，您可以执行`/etc/init.d/ilogtaild status`命令检查Logtail状态。


## 附录：环境变量说明

环境变量与Logtail启动参数的对应关系如下，具体的参数说明请参见[表 1](#table_r2i_oh4_os6)。

|参数|环境变量|优先级|支持版本|
|--|----|---|----|
|cpu\_usage\_limit|cpu\_usage\_limit|如果您通过环境变量和配置文件修改了Logtail启动参数，以环境变量为准。|Logtail 0.16.32及以上版本|
|mem\_usage\_limit|mem\_usage\_limit|
|max\_bytes\_per\_sec|max\_bytes\_per\_sec|
|process\_thread\_count|process\_thread\_count|
|send\_request\_concurrency|send\_request\_concurrency|
|check\_point\_filename|ALIYUN\_LOGTAIL\_CHECK\_POINT\_PATH|如果您通过环境变量和配置文件修改了Logtail启动参数，以环境变量为准。|Logtail 0.16.36及以上版本|
|user\_config\_file\_path|user\_config\_file\_path|如果您通过环境变量和配置文件修改了Logtail启动参数，以配置文件为准。|Logtail 0.16.56及以上版本|
|discard\_old\_data|discard\_old\_data|
|working\_ip|working\_ip|
|working\_hostname|working\_hostname|
|max\_read\_buffer\_size|max\_read\_buffer\_size|
|oas\_connect\_timeout|oas\_connect\_timeout|
|oas\_request\_timeout|oas\_request\_timeout|
|data\_server\_port|data\_server\_port|
|accept\_multi\_config|accept\_multi\_config|
|enable\_log\_time\_auto\_adjust|enable\_log\_time\_auto\_adjust|如果您通过环境变量和配置文件修改了Logtail启动参数，以配置文件为准。|Logtail 1.0.19及以上版本|

