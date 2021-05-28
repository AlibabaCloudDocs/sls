# 通过Sidecar-CRD方式采集容器日志

本文介绍如何安装Sidecar及使用CRD方式创建Logtail配置，完成容器日志的采集。

已安装alibaba-log-controller Helm。更多信息，请参见[安装Logtail日志组件](/cn.zh-CN/数据采集/Logtail采集/采集容器日志/安装Logtail日志组件.md)。

## 技术原理

通过Sidecar模式采集日志，依赖于Logtail和业务容器共享的日志目录，业务容器将日志写入到共享目录中，Logtail通过监控共享目录中日志文件的变化并采集日志。更多信息，请参见[Sidecar日志采集介绍](https://kubernetes.io/docs/concepts/cluster-administration/logging/#sidecar-container-with-a-logging-agent)和[Sidecar模式示例](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/#how-pods-manage-multiple-containers)。

## 步骤一：安装Sidecar

Sidecar模式的配置模板如下所示。

```
apiVersion: batch/v1
kind: Job
metadata:
  name: nginx-log-sidecar-demo
  namespace: default
spec:
  template:
    metadata:
      name: nginx-log-sidecar-demo
    spec:
      restartPolicy: Never
      containers:
      - name: nginx-log-demo
        image: registry.cn-hangzhou.aliyuncs.com/log-service/docker-log-test:latest
        command: ["/bin/mock_log"]
        args: ["--log-type=nginx", "--stdout=false", "--stderr=true", "--path=/var/log/nginx/access.log", "--total-count=1000000000", "--logs-per-sec=100"]
        volumeMounts:
        - name: nginx-log
          mountPath: /var/log/nginx
      ##### logtail sidecar container
      - name: logtail
        # more info: https://cr.console.aliyun.com/repository/cn-hangzhou/log-service/logtail/detail
        # this images is released for every region
        image: registry.cn-hangzhou.aliyuncs.com/log-service/logtail:latest
        # when recevie sigterm, logtail will delay 10 seconds and then stop
        command:
        - sh
        - -c
        - /usr/local/ilogtail/run_logtail.sh 10
        livenessProbe:
          exec:
            command:
            - /etc/init.d/ilogtaild
            - status
          initialDelaySeconds: 30
          periodSeconds: 30
        resources:
          limits:
            memory: 512Mi
          requests:
            cpu: 10m
            memory: 30Mi
        env:
          ##### base config
          # user id
          - name: "ALIYUN_LOGTAIL_USER_ID"
            value: "${your_aliyun_user_id}"
          # user defined id
          - name: "ALIYUN_LOGTAIL_USER_DEFINED_ID"
            value: "${your_machine_group_user_defined_id}"
          # config file path in logtail's container
          - name: "ALIYUN_LOGTAIL_CONFIG"
            value: "/etc/ilogtail/conf/${your_region_config}/ilogtail_config.json"
          ##### env tags config
          - name: "ALIYUN_LOG_ENV_TAGS"
            value: "_pod_name_|_pod_ip_|_namespace_|_node_name_|_node_ip_"
          - name: "_pod_name_"
            valueFrom:
              fieldRef:
                fieldPath: metadata.name
          - name: "_pod_ip_"
            valueFrom:
              fieldRef:
                fieldPath: status.podIP
          - name: "_namespace_"
            valueFrom:
              fieldRef:
                fieldPath: metadata.namespace
          - name: "_node_name_"
            valueFrom:
              fieldRef:
                fieldPath: spec.nodeName
          - name: "_node_ip_"
            valueFrom:
              fieldRef:
                fieldPath: status.hostIP
        volumeMounts:
        - name: nginx-log
          mountPath: /var/log/nginx
      ##### share this volume
      volumes:
      - name: nginx-log
        emptyDir: {}
```

1.  登录您的Kubernetes集群。

2.  配置基础运行参数。

    ```
    ##### base config
              # user id
              - name: "ALIYUN_LOGTAIL_USER_ID"
                value: "$\{your\_aliyun\_user\_id\}"
              # user defined id
              - name: "ALIYUN_LOGTAIL_USER_DEFINED_ID"
                value: "$\{your\_machine\_group\_user\_defined\_id\}"
              # config file path in logtail's container
              - name: "ALIYUN_LOGTAIL_CONFIG"
                value: "/etc/ilogtail/conf/$\{your\_region\_config\}/ilogtail_config.json"
    ```

    |参数|说明|
    |:-|:-|
    |$\{your\_region\_config\}|请根据日志服务Project所在地及网络类型填写。其中，地域信息请参见[表 1](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)。     -   如果为公网，格式为`region-internet`，例如：**华东 1（杭州）**为**cn-hangzhou-Internet**。
    -   如果为阿里云内网，格式为`region`。例如：**华东 1（杭州）**为**cn-hangzhou**。 |
    |$\{your\_aliyun\_user\_id\}|配置为您的阿里云账号ID。更多信息，请参见[获取阿里云账号ID](/cn.zh-CN/数据采集/Logtail采集/机器组/配置用户标识.md)。|
    |$\{your\_machine\_group\_user\_defined\_id\}|您机器组的自定义标识，请确保该标识在您的Project所在地域内唯一。更多信息，请参见[创建用户自定义标识机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建用户自定义标识机器组.md)。|

3.  配置挂载路径。

    ```
    volumeMounts:
    - name: nginx-log
    mountPath: /var/log/nginx
    ```

    -   请确保Logtail容器和业务容器挂载相同的目录。
    -   建议使用`emptyDir`的挂载方式。
4.  延迟停止采集的时间。

    通常情况下，延迟停止采集的时间为10秒，即Logtail容器在接收到外部停止信号后会等待10秒再退出，防止有部分数据没有采集完毕。

    ```
    command:        
    - sh        
    - -c        
    - /usr/local/ilogtail/run_logtail.sh 10
    ```


## 步骤二：创建Logtail配置

安装Sidecar后，您只需要定义AliyunLogConfig CRD即可创建Logtail配置，CRD配置格式如下所示。如果您要删除对应的Logtail配置只需删除对应的CRD资源即可。

```
apiVersion: log.alibabacloud.com/v1alpha1      ## 默认值，无需修改。
kind: AliyunLogConfig                          ## 默认值，无需修改。
metadata:
  name: simple-stdout-example                  ## 资源名，在集群内唯一。
spec:
  project: k8s-my-project                      ## Project名称，如果不设置，默认采集日志到日志服务组件安装时的Project。
  logstore: k8s-stdout                         ## Logstore名称，不存在时自动创建。
  machineGroups:                               ## 机器组名称，需要设置成Sidecar中配置的$\{your\_machine\_group\_user\_defined\_id\}。Sidecar与CRD通过此处配置的机器组建立关联。
  - nginx-log-sidecar
  shardCount: 2                                ## (可选)Shard数量，默认为2，支持1~10。
  lifeCycle: 90                                ## (可选)Logstore中数据的存储时间，默认为90，支持1~7300，7300天为永久存储。
  logtailConfig:                               ## Logtail配置。更多信息，请参见[Logtail配置](/cn.zh-CN/开发指南/API 参考/公共资源说明/Logtail配置.md)。
    inputType: file                            ## 采集的数据源类型（file、plugin），不支持采集容器标准输出。
    configName: simple-stdout-example          ## Logtail配置名称，与资源名（metadata.name）保持一致。
    inputDetail:                               ## Logtail配置的详细信息。更多信息，请参见[示例](#section_wwk_qls_wos)。
    ...
```

创建完成后，Logtail容器自动将产生的日志采集到日志服务，您可登录日志服务控制台查看。

**说明：**

-   请确保CRD中的configName字段值在SLS Project中唯一存在。如果多个CRD关联同一个Logtail配置，任意一个CRD的删除、修改均会影响到该Logtail配置，导致其他关联该Logtails配置的CRD状态与服务端不一致。
-   Sidecar只支持文本文件采集，文本文件采集模式中，需把dockerFile选项设置为false。

## 示例

在线下IDC上自建Kubernetes集群安装Sidecar。其中，日志服务所在地域为华东1（杭州），使用的是公网方式采集，待挂载的卷名称为nginx-log ，类型为emptyDir，分别挂载到nginx-log-demo容器和Logtail容器的/var/log/nginx目录下。

1.  安装Sidecar。

    ```
    apiVersion: batch/v1
    kind: Job
    metadata:
      name: nginx-log-sidecar-demo
      namespace: default
    spec:
      template:
        metadata:
          name: nginx-log-sidecar-demo
        spec:
          restartPolicy: Never
          containers:
          - name: nginx-log-demo
            image: registry.cn-hangzhou.aliyuncs.com/log-service/docker-log-test:latest
            command: ["/bin/mock_log"]
            args: ["--log-type=nginx", "--stdout=false", "--stderr=true", "--path=/var/log/nginx/access.log", "--total-count=1000000000", "--logs-per-sec=100"]
            volumeMounts:
            - name: nginx-log
              mountPath: /var/log/nginx
          ##### logtail sidecar container
          - name: logtail
            # more info: https://cr.console.aliyun.com/repository/cn-hangzhou/log-service/logtail/detail
            # this images is released for every region
            image: registry.cn-hangzhou.aliyuncs.com/log-service/logtail:latest
            # when recevie sigterm, logtail will delay 10 seconds and then stop
            command:
            - sh
            - -c
            - /usr/local/ilogtail/run_logtail.sh 10
            livenessProbe:
              exec:
                command:
                - /etc/init.d/ilogtaild
                - status
              initialDelaySeconds: 30
              periodSeconds: 30
            env:
              ##### base config
              # user id
              - name: "ALIYUN_LOGTAIL_USER_ID"
                value: "xxxxxxxxxx"
              # user defined id
              - name: "ALIYUN_LOGTAIL_USER_DEFINED_ID"
                value: "nginx-log-sidecar"
              # config file path in logtail's container
              - name: "ALIYUN_LOGTAIL_CONFIG"
                value: "/etc/ilogtail/conf/cn-hangzhou-internet/ilogtail_config.json"
              ##### env tags config
              - name: "ALIYUN_LOG_ENV_TAGS"
                value: "_pod_name_|_pod_ip_|_namespace_|_node_name_|_node_ip_"
              - name: "_pod_name_"
                valueFrom:
                  fieldRef:
                    fieldPath: metadata.name
              - name: "_pod_ip_"
                valueFrom:
                  fieldRef:
                    fieldPath: status.podIP
              - name: "_namespace_"
                valueFrom:
                  fieldRef:
                    fieldPath: metadata.namespace
              - name: "_node_name_"
                valueFrom:
                  fieldRef:
                    fieldPath: spec.nodeName
              - name: "_node_ip_"
                valueFrom:
                  fieldRef:
                    fieldPath: status.hostIP
            volumeMounts:
            - name: nginx-log
              mountPath: /var/log/nginx
          ##### share this volume
          volumes:
          - name: nginx-log
            emptyDir: {}
    ```

2.  创建Logtail配置。

    -   配置采集的访问日志为/var/log/nginx/access.log ，采集的目的Logstore为nginx-access。

        **说明：** Sidecar只支持文本文件采集，文本文件采集模式中，需把dockerFile选项设置为false。

        ```
        apiVersion: log.alibabacloud.com/v1alpha1
        kind: AliyunLogConfig
        metadata:
          # 配置名称，在您的K8s集群中必须唯一。
          name: nginx-log-access-example
        spec:
          # Project名称。
          project: k8s-nginx-sidecar-demo
          # Logstore名称。
          logstore: nginx-access
          # 已应用Logtail配置的机器组，该机器组需与您sidecar中的[ALIYUN_LOGTAIL_USER_DEFINED_ID]保持一致。
          machineGroups:
          - nginx-log-sidecar
          # Logtail配置详情。
          logtailConfig:
            # 采集的数据源类型（file)。
            inputType: file
            # Logtail配置名称，必须与metadata.name相同。
            configName: nginx-log-access-example
            inputDetail:
              # 极简模式日志，logType设置为"common_reg_log"。
              logType: common_reg_log
              # 日志文件夹。
              logPath: /var/log/nginx
              # 文件名, 支持通配符，例如log_*.log。
              filePattern: access.log
              # sidecar模式，dockerFile设置为false。
              dockerFile: false
              # 行首正则表达式，如果为单行模式，设置成 .*。
              logBeginRegex: '.*'
              # 解析正则。
              regex: '(\S+)\s(\S+)\s\S+\s\S+\s"(\S+)\s(\S+)\s+([^"]+)"\s+(\S+)\s(\S+)\s(\d+)\s(\d+)\s(\S+)\s"([^"]+)"\s.*'
              # 提取出的key列表。
              key : ["time", "ip", "method", "url", "protocol", "latency", "payload", "status", "response-size",user-agent"]
        # config for error log
        ```

    -   配置采集的错误日志为/var/log/nginx/error.log ，采集的目的Logstore为nginx-error。

        ```
        # config for error log
        apiVersion: log.alibabacloud.com/v1alpha1
        kind: AliyunLogConfig
        metadata:
          # your config name, must be unique in you k8s cluster
          name: nginx-log-error-example
        spec:
          # Project名称
          project: k8s-nginx-sidecar-demo
          # Logstore名称
          logstore: nginx-error
          # 已应用Logtail配置的机器组，该机器组需与您sidecar中的[ALIYUN_LOGTAIL_USER_DEFINED_ID]保持一致。
          machineGroups:
          - nginx-log-sidecar
          # Logtail配置详情。
          logtailConfig:
            # 采集的数据源类型（file)。
            inputType: file
            # Logtail配置名称，必须与metadata.name相同。
            configName: nginx-log-error-example
            inputDetail:
              # 极简模式日志，logType设置为"common_reg_log"。
              logType: common_reg_log
              # 日志文件夹。
              logPath: /var/log/nginx
              # 文件名, 支持通配符，例如log_*.log。
              filePattern: error.log
              # sidecar模式，dockerFile设置为false。
              dockerFile: false
        ```


