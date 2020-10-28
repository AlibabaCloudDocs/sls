# 配置Logtail启动参数

本文介绍Logtail启动参数的配置场景及详细说明。

## 应用场景

遇到以下场景时，可修改Logtail启动参数。

-   需要采集的日志文件数目大，占用大量内存。
-   日志数据流量大导致CPU占用率高。
-   日志数据量大导致发送到日志服务的网络流量大。

## 启动参数说明

-   文件路径

    /usr/local/ilogtail/ilogtail\_config.json

-   文件格式

    JSON

-   文件示例（只展示部分参数）

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


## 配置启动参数

1.  打开ilogtail\_config.json文件。

2.  根据需求配置启动参数。

    -   下表中只列出您需要关注的常用启动参数，未列出的启动参数，保持默认配置即可。
    -   请根据需要新增或修改指定启动参数，不必要的参数不需要增加到ilogtail\_config.json文件中。
    |参数|类型|说明|
    |:-|--|:-|
    |cpu\_usage\_limit|double|CPU使用阈值，以单核计算，最小值为0.1，最大值为当前机器的CPU核心数，默认值：2。 例如：设置为0.4，表示限制Logtail的CPU使用为CPU单核的40%，超出后Logtail自动重启。

一般情况下，通过极简模式采集日志时，单核处理能力约24MB/s；通过完整正则模式采集日志时，单核处理能力约12MB/s 。 |
    |mem\_usage\_limit|int|内存使用阈值，单位：MB，最小值为128，最大值为当前机器有效内存值，默认值：2048。 例如：设置为100，表示Logtail的内存为100MB，超出后Logtail自动重启。

如果需要同时采集的文件数目超过1000，可根据需求修改此参数。 |
    |max\_bytes\_per\_sec|int|每秒钟Logtail发送原始数据的流量限制，单位为Byte/s，取值范围：1024~52428800，默认值：20971520。 例如：设置为2097152，表示Logtail发送数据的速率为2MB/s。

**说明：** 配置超过20971520Byte/s（20MB/s），表示不限速。 |
    |process\_thread\_count|int|Logtail处理数据的线程数，单位：个， 取值范围：1~64，默认值：1。 一般情况下，可以处理极简模式下24MB/s的数据写入或完整正则模式12MB/s的数据写入。默认情况下无需调整该参数取值。 |
    |send\_request\_concurrency|int|异步并发的个数，单位：个，取值范围：1~1000，默认值：20。Logtail默认异步发送数据包。 如果写入TPS很高，可以配置更高的异步并发个数。可以按照一个并发支持0.5MB/s~1MB/s网络吞吐来计算，具体依据网络延时而定。

**说明：** 设置异步并发个数过高容易导致网络端口占用过多，需调整TCP相关参数，详情请参见[调整TCP相关参数](https://yq.aliyun.com/articles/52884)。 |
    |buffer\_file\_num|int|限制缓存文件的最大数目，单位：个，取值范围：1~100，默认值：25。 遇到网络异常、写入配额超限等情况时，Logtail将实时解析后的日志写入本地文件（安装目录下）缓存起来，等待恢复后尝试重新发送。 |
    |buffer\_file\_size|int|单个缓存文件允许的最大字节数，单位：Byte，取值范围：1048576~104857600，默认值：20971520Byte，即20MB。 buffer\_file\_size\*buffer\_file\_num是缓存文件可以实际使用的最大磁盘空间。 |
    |buffer\_file\_path|String|缓存文件存放目录。 默认值为空，即缓存文件存放于logtail安装目录/usr/local/ilogtail下。 如果您要配置此参数，配置后，需手动将原目录下名为 logtail\\\_buffer\\\_file\_\*的文件移动到此目录，以保证logtail可以读取到该缓存文件并在发送后进行删除。 |
    |bind\_interface|String|本机绑定的网卡名。默认值为空，自动绑定可用的网卡。 如果配置该参数（例如：配置为eth1），则Logtail将强制使用该网卡上传日志。

**说明：** 只支持Linux版本。 |
    |check\_point\_filename|String|Logtail的checkpoint文件的保存路径， 默认情况下为/tmp/logtail\_check\_point。 建议Docker用户修改checkpoint文件保存路径，并将宿主机挂载此路径，否则容器释放时会因丢失checkpoint信息而造成重复采集。例如：在Docker中配置check\_point\_filename为`/data/logtail/check_point.dat`，在Docker启动命令中增加`-v /data/docker1/logtail:/data/logtail`， 将宿主机/data/docker1/logtail目录挂载到Docker中的/data/logtail目录。 |
    |user\_config\_file\_path|String|Logtail配置文件的保存路径，默认情况下为进程binary所在目录，文件名为user\_log\_config.json。 建议Docker用户修改Logtail采集配置文件保存路径，并将宿主机挂载此路径，否则容器释放时会因丢失checkpoint信息而造成重复采集。例如：Docker中配置user\_config\_file\_path为/data/logtail/user\_log\_config.json，在Docker启动命令中增加`-v /data/docker1/logtail:/data/logtail`， 将宿主机/data/docker1/logtail目录挂载到Docker中的/data/logtail目录。 |
    |discard\_old\_data|bool|是否丢弃历史日志。默认值：true，表示丢弃距离当前时间超过12小时的日志。|
    |working\_ip|String|Logtail上报本服务器的IP地址。默认值为空，表示自动从本服务器获取IP地址。|
    |working\_hostname|String|Logtail上报的本服务器的主机名。默认值为空，表示自动从本服务器获取主机名。|
    |max\_read\_buffer\_size|long|每条日志读取的最大值，默认值：524288Byte（512KB）。如果您的单条日志超过512KB，可修改此配置。|
    |oas\_connect\_timeout|long|Logtail发起获取Logtail配置、访问密钥等请求时，连接阶段的超时时间。单位：秒，默认值：5秒。 网络条件较差，建立连接时间过长时可修改此配置。 |
    |oas\_request\_timeout|long|Logtail发起获取Logtail配置、访问密钥等请求时，整个请求阶段的超时时间。 单位：秒，默认值：10秒。 网络条件较差，建立连接时间过长时可修改此配置。 |

3.  重启Logtail使配置生效。

    ```
    /etc/init.d/ilogtaild stop
    /etc/init.d/ilogtaild start
    /etc/init.d/ilogtaild status
    ```


