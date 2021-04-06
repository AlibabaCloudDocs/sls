# 通过DaemonSet-CRD方式采集日志

在Kubernetes容器中以DaemonSet模式安装Logtail后，可通过CRD方式创建采集配置采集Kubernetes集群日志。本文介绍如何通过CRD方式创建采集配置。

已安装alibaba-log-controller Helm。更多信息，请参见[安装Logtail日志组件](/cn.zh-CN/数据采集/Logtail采集/采集容器日志/安装Logtail日志组件.md)。

## 实现原理

![kub-CRD实现原理](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4330559951/p2686.png)

CRD配置的内部工作流程如下：

1.  使用`kubectl`或其他工具应用aliyunlogconfigs CRD配置。
2.  alibaba-log-controller监听到CRD配置更新。
3.  alibaba-log-controller根据CRD内容以及服务端状态，自动向日志服务提交创建Logstore、创建采集配置以及应用机器组的请求。
4.  Logtail定期请求配置服务器，获取新的或已更新的配置并进行热加载。
5.  Logtail根据配置信息采集各个容器上的标准输出或文本文件。
6.  Logtail将采集到的数据发送到日志服务。

## 创建采集配置

您只需要定义AliyunLogConfig CRD即可创建采集配置，CRD配置格式如下所示。如果您要删除对应的采集配置只需删除对应的CRD资源即可。

```
apiVersion: log.alibabacloud.com/v1alpha1      ## 默认值，无需修改。
kind: AliyunLogConfig                          ## 默认值，无需修改。
metadata:
  name: simple-stdout-example                  ## 资源名，在集群内唯一。
spec:
  project: k8s-my-project                      ## [可选]Project名称，默认为安装时设置的Project，若指定Project请确保该Project未被使用。
  logstore: k8s-stdout                         ## Logstore名称，不存在时自动创建。
  shardCount: 2                                ## [可选]Shard数量，默认为2，支持1~10。
  lifeCycle: 90                                ## [可选]Logstore中数据的存储时间，默认为90，支持1-7300，7300天为永久存储。
  logtailConfig:                               ## 详细配置
    inputType: plugin                          ## 采集的数据源类型，file（文本文件）或plugin（标准输出）。
    configName: simple-stdout-example          ## 采集配置的名称，与资源名(metadata.name)保持一致。
    inputDetail:                               ## 采集配置的详细信息，具体配置请参见本文下方的示例。
      ...
```

`logtailConfig`字段的详细说明请参见[Logtail配置](/cn.zh-CN/开发指南/API 参考/公共资源说明/Logtail配置.md)。`logtailConfig`的配置示例请参见[示例（标准输出）](#section_c3g_wm1_f2b)和[示例（文本文件）](#section_qlv_zm1_f2b)。

创建完成后，自动应用该采集配置。

## 查看采集配置

您可以通过CRD或控制台查看采集配置，其中控制台方式请参见[管理Logtail采集配置](/cn.zh-CN/数据采集/Logtail采集/机器组/管理Logtail采集配置.md)。

**说明：** 如果您使用的是CRD方式，但又通过控制台更改了配置，则下一次使用CRD方式更新配置时，会覆盖控制台的更改内容。

-   使用`kubectl get aliyunlogconfigs`查看当前所有的采集配置。

    ```
    [root@iZbp1dsbiaZ ~]# kubectl get aliyunlogconfigs
    NAME                   AGE
    regex-file-example     10s
    regex-stdout-example   4h
    simple-file-example    5s
    ```

-   执行`kubectl get aliyunlogconfigs ${config_name} -o yaml`查看采集配置的详细信息和状态。其中， `{config_name}`为采集配置的名称，请根据实际情况替换。

    执行结果中的`status`字段表示配置执行的结果。如果`statusCode`为200，表示配置应用成功；如果`statusCode`非200，表示配置应用失败。

    ```
    [root@iZbp1dsbiaZ ~]# kubectl get aliyunlogconfigs simple-file-example -o yaml
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
    annotations:
      kubectl.kubernetes.io/last-applied-configuration: |
        {"apiVersion":"log.alibabacloud.com/v1alpha1","kind":"AliyunLogConfig","metadata":{"annotations":{},"name":"simple-file-example","namespace":"default"},"spec":{"logstore":"k8s-file","logtailConfig":{"configName":"simple-file-example","inputDetail":{"dockerFile":true,"dockerIncludeEnv":{"ALIYUN_LOGTAIL_USER_DEFINED_ID":""},"filePattern":"simple.LOG","logPath":"/usr/local/ilogtail","logType":"common_reg_log"},"inputType":"file"}}}
    clusterName: ""
    creationTimestamp: 2018-05-17T08:44:46Z
    generation: 0
    name: simple-file-example
    namespace: default
    resourceVersion: "21790443"
    selfLink: /apis/log.alibabacloud.com/v1alpha1/namespaces/default/aliyunlogconfigs/simple-file-example
    uid: 8d3a09c4-59ae-11e8-851d-00163f008685
    spec:
    lifeCycle: null
    logstore: k8s-file
    logtailConfig:
      configName: simple-file-example
      inputDetail:
        dockerFile: true
        dockerIncludeEnv:
          ALIYUN_LOGTAIL_USER_DEFINED_ID: ""
        filePattern: simple.LOG
        logPath: /usr/local/ilogtail
        logType: common_reg_log
      inputType: file
    machineGroups: null
    project: ""
    shardCount: null
    status:
    status: OK
    statusCode: 200
    ```


## 示例（标准输出）

采集K8s标准输出时，请将`inputType`设置为`plugin`，并将具体信息填写到`inputDetail`下的`plugin`字段，具体字段及其说明请参见[通过DaemonSet-控制台方式采集Kubernetes标准输出](/cn.zh-CN/数据采集/Logtail采集/采集容器日志/通过DaemonSet-控制台方式采集Kubernetes标准输出.md)。

-   极简采集方式

    举例：采集除了环境变量中配置为`COLLECT_STDOUT_FLAG=false`之外的所有容器的标准输出（stdout和stderr），参考示例如下所示。

    ```
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in you k8s cluster
      name: simple-stdout-example
    spec:
      # logstore name to upload log
      logstore: k8s-stdout
      # logtail config detail
      logtailConfig:
        # docker stdout's input type is 'plugin'
        inputType: plugin
        # logtail config name, should be same with [metadata.name]
        configName: simple-stdout-example
        inputDetail:
          plugin:
            inputs:
              -
                # input type
                type: service_docker_stdout
                detail:
                  # collect stdout and stderr
                  Stdout: true
                  Stderr: true
                  # collect all container's stdout except containers with "COLLECT_STDOUT_FLAG:false" in docker env config
                  ExcludeEnv:
                    COLLECT_STDOUT_FLAG: "false"
    ```

-   自定义处理采集方式

    举例：采集grafana的访问日志，并将访问日志解析成结构化数据。其中，grafana的容器配置中包含的环境变量为`GF_INSTALL_PLUGINS=grafana-piechart-....`，通过配置`IncludeEnv`为`GF_INSTALL_PLUGINS: ''`指定Logtail只采集该容器的标准输出。

    ![自定义处理采集方式](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4330559951/p2687.png)

    grafana访问日志的格式如下所示。

    ```
    t=2018-03-09T07:14:03+0000 lvl=info msg="Request Completed" logger=context userId=0 orgId=0 uname= method=GET path=/ status=302 remote_addr=172.16.64.154 time_ms=0 size=29 referer=
    ```

    使用正则表达式解析访问日志，参考示例如下所示。

    ```
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in you k8s cluster
      name: regex-stdout-example
    spec:
      # logstore name to upload log
      logstore: k8s-stdout-regex
      # logtail config detail
      logtailConfig:
        # docker stdout's input type is 'plugin'
        inputType: plugin
        # logtail config name, should be same with [metadata.name]
        configName: regex-stdout-example
        inputDetail:
          plugin:
            inputs:
              -
                # input type
                type: service_docker_stdout
                detail:
                  # 只采集stdout，不采集stderr
                  Stdout: true
                  Stderr: false
                  # 只采集容器环境变量中配置key为"GF_INSTALL_PLUGINS"的stdout。
                  IncludeEnv:
                    GF_INSTALL_PLUGINS: ''
            processors:
              -
                # 使用正则表达式处理
                type: processor_regex
                detail:
                  # 从docker采集的数据默认key为"content"。
                  SourceKey: content
                  # 正则表达式提取。
                  Regex: 't=(\d+-\d+-\w+:\d+:\d+\+\d+) lvl=(\w+) msg="([^"]+)" logger=(\w+) userId=(\w+) orgId=(\w+) uname=(\S*) method=(\w+) path=(\S+) status=(\d+) remote_addr=(\S+) time_ms=(\d+) size=(\d+) referer=(\S*).*'
                  # 提取出的key。
                  Keys: ['time', 'level', 'message', 'logger', 'userId', 'orgId', 'uname', 'method', 'path', 'status', 'remote_addr', 'time_ms', 'size', 'referer']
                  # 保留原始字段。
                  KeepSource: true
                  NoKeyError: true
                  NoMatchError: true
    ```

    应用配置后，采集到日志服务的数据如下所示。

    ![采集到的日志数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4330559951/p2689.png)


## 示例（文本文件）

采集K8s文本文件时，请将`inputType`设置为`file`，并将具体信息填写到`inputDetail`内，具体字段及说明请参见[通过DaemonSet-控制台方式采集Kubernetes文件](/cn.zh-CN/数据采集/Logtail采集/采集容器日志/通过DaemonSet-控制台方式采集Kubernetes文件.md)。

-   极简模式的文件

    举例：采集环境变量配置中含有key为`ALIYUN_LOGTAIL_USER_DEFINED_ID`的容器日志文件，文件所处的路径为`/data/logs/app_1`，文件名为`simple.LOG`，参考示例如下所示。

    ```
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in you k8s cluster
      name: simple-file-example
    spec:
      # logstore name to upload log
      logstore: k8s-file
      # logtail config detail
      logtailConfig:
        # log file's input type is 'file'
        inputType: file
        # logtail config name, should be same with [metadata.name]
        configName: simple-file-example
        inputDetail:
          # 极简模式日志，logType设置为common_reg_log。
          logType: common_reg_log
          # 日志文件夹
          logPath: /data/logs/app_1
          # 文件名, 支持通配符，例如log_*.log。
          filePattern: simple.LOG
          # 采集容器内的文件，dockerFile设置为true。
          dockerFile: true
          # only collect container with "ALIYUN_LOGTAIL_USER_DEFINED_ID" in docker env config
          dockerIncludeEnv:
            ALIYUN_LOGTAIL_USER_DEFINED_ID: ""
    ```

-   正则模式的文件

    举例：某Java程序日志样例如下所示，日志中由于包含错误堆栈信息，可能一条日志会被分解成多行，因此需要设置行首正则表达式。

    ```
    [2018-05-11T20:10:16,000] [INFO] [SessionTracker] [SessionTrackerImpl.java:148] Expiring sessions
    java.sql.SQLException: Incorrect string value: '\xF0\x9F\x8E\x8F",...' for column 'data' at row 1
    at org.springframework.jdbc.support.AbstractFallbackSQLExceptionTranslator.translate(AbstractFallbackSQLExceptionTranslator.java:84)
    at org.springframework.jdbc.support.AbstractFallbackSQLException
    ```

    参考示例如下所示。

    ```
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in you k8s cluster
      name: regex-file-example
    spec:
      # logstore name to upload log
      logstore: k8s-file
      logtailConfig:
        # log file's input type is 'file'
        inputType: file
        # logtail config name, should be same with [metadata.name]
        configName: regex-file-example
        inputDetail:
          # 对于正则类型的日志，将logType设置为common_reg_log。
          logType: common_reg_log
          # 日志文件夹
          logPath: /app/logs
          # 文件名, 支持通配符，例如log_*.log。
          filePattern: error.LOG
          # 行首正则表达式
          logBeginRegex: '\[\d+-\d+-\w+:\d+:\d+,\d+]\s\[\w+]\s.*'
          # 解析正则
          regex: '\[([^]]+)]\s\[(\w+)]\s\[(\w+)]\s\[([^:]+):(\d+)]\s(.*)'
          # 提取出的key列表
          key : ["time", "level", "method", "file", "line", "message"]
          # 正则模式日志，时间解析默认从日志中的time字段中提取。如果无需提取时间，可不设置该字段。当您设置了timeFormat字段后，默认按照logtail容器的时区（0时区）解析数据。您东八区的日志时间将往后延迟8小时。例如11:00产生的日志上传到日志服务后，日志时间为19:00。
          timeFormat: '%Y-%m-%dT%H:%M:%S'
          # 采集容器内的文件，dockerFile设置为true。
          dockerFile: true
          # only collect container with "ALIYUN_LOGTAIL_USER_DEFINED_ID" in docker env config
          dockerIncludeEnv:
            ALIYUN_LOGTAIL_USER_DEFINED_ID: ""
    ```

    应用配置后，采集到日志服务的数据如下所示。

    ![正则模式](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4330559951/p5237.png)

-   分隔符模式的日志文件

    Logtail支持分隔符模式的日志解析，参考示例如下所示。

    ```
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in you k8s cluster
      name: delimiter-file-example
    spec:
      # logstore name to upload log
      logstore: k8s-file
      logtailConfig:
        # log file's input type is 'file'
        inputType: file
        configName: delimiter-file-example
        # logtail config name, should be same with [metadata.name]
        inputDetail:
          # 对于分隔符类型的日志，logType设置为delimiter_log。
          logType: delimiter_log
          # 日志文件夹
          logPath: /usr/local/ilogtail
          # 文件名, 支持通配符，例如log_*.log。
          filePattern: delimiter_log.LOG
          # 使用多字符分隔符
          separator: '|&|'
          # 提取的key列表
          key : ['time', 'level', 'method', 'file', 'line', 'message']
          # 用作解析时间的key，如无需求则填为''。
          timeKey: 'time'
          # 时间解析方式，如无需求则填为''。
          timeFormat: '%Y-%m-%dT%H:%M:%S'
          # 采集容器内的文件，dockerFile flag设置为true。
          dockerFile: true
          # only collect container with "ALIYUN_LOGTAIL_USER_DEFINED_ID" in docker env config
          dockerIncludeEnv:
            ALIYUN_LOGTAIL_USER_DEFINED_ID: ''
    ```

-   JSON模式的文件

    如果文件中每行数据为一个JSON object，可以使用JSON方式进行解析，参考示例如下所示。

    ```
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in you k8s cluster
      name: json-file-example
    spec:
      # logstore name to upload log
      logstore: k8s-file
      logtailConfig:
        # log file's input type is 'file'
        inputType: file
        # logtail config name, should be same with [metadata.name]
        configName: json-file-example
        inputDetail:
          # 对于分隔符类型的日志，logType设置为json_log。
          logType: json_log
          # 日志文件夹
          logPath: /usr/local/ilogtail
          # 文件名, 支持通配符，例如log_*.log。
          filePattern: json_log.LOG
          # 用作解析时间的key，如无需求则填为''。
          timeKey: 'time'
          # 时间解析方式，如无需求则填为''。
          timeFormat: '%Y-%m-%dT%H:%M:%S'
          # 采集容器内的文件，dockerFile设置为true。
          dockerFile: true
          # only collect container with "ALIYUN_LOGTAIL_USER_DEFINED_ID" in docker env config
          dockerIncludeEnv:
            ALIYUN_LOGTAIL_USER_DEFINED_ID: ""
    ```


