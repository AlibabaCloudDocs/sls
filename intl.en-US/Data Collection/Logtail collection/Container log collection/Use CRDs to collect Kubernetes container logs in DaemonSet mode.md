# Use CRDs to collect Kubernetes container logs in DaemonSet mode

After you install Logtail in DaemonSet mode in a Kubernetes cluster, you can use a custom resource definition \(CRD\) to configure log collection of Kubernetes clusters. This topic describes how to use CRDs to collect Kubernetes container logs in DaemonSet mode.

The Helm package alibaba-log-controller is installed. For more information, see [Install Logtail](/intl.en-US/Data Collection/Logtail collection/Container log collection/Install Logtail.md).

## Procedure

![Kubernetes-CRDs implementation](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5996549951/p2686.png)

The following steps describe the process to collect logs:

1.  Use the `kubectl` tool or other tools to create an aliyunlogconfigs CRD.
2.  The alibaba-log-controller package detects the creation of the CRD.
3.  The alibaba-log-controller Deployment controller sends a request to Log Service to create a Logstore, configure a server group, and configure Logtail based on the CRD.
4.  Logtail periodically requests the master node where log collection is configured to obtain new or updated configuration files and perform hot reloading.
5.  Logtail collects stdout and stderr logs or text logs from each container based on the updated configurations.
6.  Logtail sends the collected logs to Log Service.

## Configure log collection

You can configure an AliyunLogConfig CRD to collect logs. The following script shows how to configure a CRD. To delete log collection configurations, delete the corresponding CRD.

```
apiVersion: log.alibabacloud.com/v1alpha1      ## The default setting, which you do not need to modify. 
kind: AliyunLogConfig                          ## The default setting, which you do not need to modify. 
metadata:
  name: simple-stdout-example                  ## The resource name, which must be unique in the cluster. 
spec:
  project: k8s-my-project                      ## The project name. You can specify a project that is not occupied. By default, the project is created when you installed the Helm package. The field is optional. 
  logstore: k8s-stdout                         ## The Logstore name. A Logstore is automatically created if the specified Logstore does not exist. 
  shardCount: shardCount: 2                                ## The number of shards. Value range: 1 to 10. Default value: 2. The field is optional. 
  lifeCycle: 90                                ## The retention period for which log data is stored in the Logstore. Unit: days. Value range: 1 to 7300. Default value: 90. The value 7300 indicates that log data is permanently stored in the Logstore. The field is optional. 
  logtailConfig:                               ## The Logtail settings.
    inputType: plugin                          ## The type of input data sources. Valid values: file and plugin. 
    configName: simple-stdout-example          ## The name of the Logtail configuration file. This name must be the same as the resource name specified by the metadata.name field. 
    inputDetail:                               ## The detailed settings of Logtail. For more information, see the following examples. 
      ...
```

For more information about the `logtailConfig` field, see [Logtail configuration files](/intl.en-US/Developer Guide/API Reference/Common resources/Logtail configuration files.md). For configuration examples of the `logtailConfig` field, see [Example configurations for collecting stdout and stderr logs](#section_c3g_wm1_f2b) and [Example configurations for collecting text logs](#section_qlv_zm1_f2b).

After you complete the configurations, Logtail automatically collects and uploads logs to Log Service.

## View log collection configurations

You can view log collection configurations by using a Kubernetes CRD or the Log Service console. For more information about how to view log collection configurations in the console, see [Manage Logtail configurations for log collection](/intl.en-US/Data Collection/Logtail collection/Machine Group/Manage Logtail configurations for log collection.md).

**Note:** If you use a CRD to configure log collection and modify the configurations in the console, the modifications are overwritten after you update the CRD.

-   Use the `kubectl get aliyunlogconfigs` command to view all Logtail configurations.

    ```
    [root@iZbp1dsbiaZ ~]# kubectl get aliyunlogconfigs
    NAME                   AGE
    regex-file-example     10s
    regex-stdout-example   4h
    simple-file-example    5s
    ```

-   Use the `kubectl get aliyunlogconfigs ${config_name} -o yaml` command to view the detailed settings and status of a Logtail configuration file. The `{config_name}` field in the command is the name of the Logtail configuration file that you want to query.

    The `status` field in the output indicates whether the Logtail configuration file is applied. If the `statusCode` field value is 200, the Logtail configurations are applied. If the `statusCode` field value is not 200, the Logtail configuration file is not applied.

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


## Example configurations for collecting stdout and stderr logs

To collect Kubernetes stdout and stderr logs, set `inputType` to `plugin` and specify the configuration details in the `plugin` field under `inputDetail`. For more information, see [Use the console to collect Kubernetes stdout and stderr logs in DaemonSet mode](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use the console to collect Kubernetes stdout and stderr logs in DaemonSet mode.md).

-   Collect logs in simple mode

    The following example shows how to collect stdout and stderr logs of all containers except the containers whose environment variables include `COLLECT_STDOUT_FLAG=false`.

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

-   Collect logs in custom mode

    The following example shows how to collect access logs of Grafana and parse the access logs into structured data. The environment variable of the Grafana container is `GF_INSTALL_PLUGINS=grafana-piechart-....`. You can set `IncludeEnv` to `GF_INSTALL_PLUGINS: ''`to collect only stdout and stderr logs from this container.

    ![Collect logs in custom mode](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5996549951/p2687.png)

    The following example shows the format of Grafana access log entries:

    ```
    t=2018-03-09T07:14:03+0000 lvl=info msg="Request Completed" logger=context userId=0 orgId=0 uname= method=GET path=/ status=302 remote_addr=172.16.64.154 time_ms=0 size=29 referer=
    ```

    The following sample code shows how to use regular expressions to parse access logs:

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
                  # Collect stdout logs, but do not collect stderr logs.
                  Stdout: true
                  Stderr: false
                  # Collect stdout logs of the containers whose environment variable keys include GF_INSTALL_PLUGINS. 
                  IncludeEnv:
                    GF_INSTALL_PLUGINS: ''
            processors:
              -
                # Use a regular expression.
                type: processor_regex
                detail:
                  # By default, the key of the data that is collected from Dockers is content. 
                  SourceKey: content
                  # Use a regular expression to extract fields. 
                  Regex: 't=(\d+-\d+-\w+:\d+:\d+\+\d+) lvl=(\w+) msg="([^"]+)" logger=(\w+) userId=(\w+) orgId=(\w+) uname=(\S*) method=(\w+) path=(\S+) status=(\d+) remote_addr=(\S+) time_ms=(\d+) size=(\d+) referer=(\S*).*'
                  # The extracted keys. 
                  Keys: ['time', 'level', 'message', 'logger', 'userId', 'orgId', 'uname', 'method', 'path', 'status', 'remote_addr', 'time_ms', 'size', 'referer']
                  # Reserve original fields. 
                  KeepSource: true
                  NoKeyError: true
                  NoMatchError: true
    ```

    The following example shows a sample log entry that is collected to Log Service after the configurations are applied:

    ![Collected log data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5996549951/p2689.png)


## Example configurations for collecting text logs

To collect Kubernetes text logs, set `inputType` to `file` and specify the configuration details in the plugin field under `inputDetail`. For more information, see [Use the console to collect Kubernetes text logs in DaemonSet mode](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use the console to collect Kubernetes text logs in DaemonSet mode.md).

-   Collect logs in simple mode

    The following example shows how to collect text logs from containers whose environment variable keys include `ALIYUN_LOGTAIL_USER_DEFINED_ID`. The log path is `/data/logs/app_1` and the log file name is `simple.LOG`.

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
          # Set logType to common_reg_log. 
          logType: common_reg_log
          # Specify the log path.
          logPath: /data/logs/app_1
          # Specify the names of log files that you want to collect. You can include wildcard characters in the field value, for example, log_*.log. 
          filePattern: simple.LOG
          # Set dockerFile to true. 
          dockerFile: true
          # only collect container with "ALIYUN_LOGTAIL_USER_DEFINED_ID" in docker env config
          dockerIncludeEnv:
            ALIYUN_LOGTAIL_USER_DEFINED_ID: ""
    ```

-   Use regular expressions to parse collected logs

    The following example shows a log entry that is generated by a Java application. The log entry contains error stack traces, which may divide the log entry into multiple lines. You must specify a regular expression to match the first line of a log entry.

    ```
    [2018-05-11T20:10:16,000] [INFO] [SessionTracker] [SessionTrackerImpl.java:148] Expiring sessions
    java.sql.SQLException: Incorrect string value: '\xF0\x9F\x8E\x8F",...' for column 'data' at row 1
    at org.springframework.jdbc.support.AbstractFallbackSQLExceptionTranslator.translate(AbstractFallbackSQLExceptionTranslator.java:84)
    at org.springframework.jdbc.support.AbstractFallbackSQLException
    ```

    The following script shows an example Logtail configuration file that includes regular expressions:

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
          # Set logType to common_reg_log. 
          logType: common_reg_log
          # Specify the log path.
          logPath: /app/logs
          # Specify the names of log files that you want to collect. You can include wildcard characters in the field value, for example, log_*.log. 
          filePattern: error.LOG
          # The regular expression that is used to match the first line of a log entry.
          logBeginRegex: '\[\d+-\d+-\w+:\d+:\d+,\d+]\s\[\w+]\s.*'
          # Use a regular expression to parse logs
          regex: '\[([^]]+)]\s\[(\w+)]\s\[(\w+)]\s\[([^:]+):(\d+)]\s(.*)'
          # Specify the keys extracted from log entries.
          key : ["time", "level", "method", "file", "line", "message"]
          # Specify the format of the time field that is extracted from log entries. The field is optional. If you specify the timeFormat field, data is parsed based on the time zone (UTC+0) of the region to which the Logtail container belongs. If the region is in the UTC+8 time zone, the time when a log is generated is 8 hours later than the time when the log is parsed. For example, if you upload a log generated at 11:00 to Log Service, the log is parsed to 19:00 on the same day. 
          timeFormat: '%Y-%m-%dT%H:%M:%S'
          # Set dockerFile to true. 
          dockerFile: true
          # only collect container with "ALIYUN_LOGTAIL_USER_DEFINED_ID" in docker env config
          dockerIncludeEnv:
            ALIYUN_LOGTAIL_USER_DEFINED_ID: ""
    ```

    The following example shows a sample log entry that is collected to Log Service after the configurations are applied:

    ![Use regular expressions to collect logs](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5996549951/p5237.png)

-   Use delimiters to parse collected logs

    To use delimiters to parse collected logs, use the following example code:

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
          # Set logType to delimiter_log. 
          logType: delimiter_log
          # Specify the log path.
          logPath: /usr/local/ilogtail
          # Specify the names of log files that you want to collect. You can include wildcard characters in the field value, for example, log_*.log. 
          filePattern: delimiter_log.LOG
          # Use multi-character delimiters.
          separator: '|&|'
          # Specify the keys extracted from log entries.
          key : ['time', 'level', 'method', 'file', 'line', 'message']
          # The key used to parse time. If you do not need to set a key, enter ''. 
          timeKey: 'time'
          # The method used to parse time. If you do not need to set a method, enter ''. 
          timeFormat: '%Y-%m-%dT%H:%M:%S'
          # Set dockerFile to true. 
          dockerFile: true
          # only collect container with "ALIYUN_LOGTAIL_USER_DEFINED_ID" in docker env config
          dockerIncludeEnv:
            ALIYUN_LOGTAIL_USER_DEFINED_ID: ''
    ```

-   Use the JSON method to parse collected logs in the JSON format

    If each line of log data in a file is a JSON object, you can parse the file by using the following format:

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
          # Set logType to json_log. 
          logType: json_log
          # Specify the log path.
          logPath: /usr/local/ilogtail
          # Specify the names of log files that you want to collect. You can include wildcard characters in the field value, for example, log_*.log. 
          filePattern: json_log.LOG
          # The key used to parse time. If you do not need to set a key, enter ''. 
          timeKey: 'time'
          # The method used to parse time. If you do not need to set a method, enter ''. 
          timeFormat: '%Y-%m-%dT%H:%M:%S'
          # Set dockerFile to true. 
          dockerFile: true
          # only collect container with "ALIYUN_LOGTAIL_USER_DEFINED_ID" in docker env config
          dockerIncludeEnv:
            ALIYUN_LOGTAIL_USER_DEFINED_ID: ""
    ```


