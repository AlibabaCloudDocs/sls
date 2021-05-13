# 通过Sidecar-控制台方式采集容器日志

本文介绍如何安装Sidecar及使用控制台方式创建Logtail配置，完成容器日志的采集。

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
    |$\{your\_region\_config\}|请根据日志服务Project所在地及网络类型填写。其中，地域信息请参见[表 1](/intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)。     -   如果为公网，格式为`region-internet`，例如：**华东 1（杭州）**为**cn-hangzhou-Internet**。
    -   如果为阿里云内网，格式为`region`。例如：**华东 1（杭州）**为**cn-hangzhou**。 |
    |$\{your\_aliyun\_user\_id\}|配置为您的阿里云账号ID。更多信息，请参见[获取阿里云账号ID](/intl.zh-CN/数据采集/Logtail采集/机器组/配置用户标识.md)。|
    |$\{your\_machine\_group\_user\_defined\_id\}|您机器组的自定义标识，请确保该标识在您的Project所在地域内唯一。更多信息，请参见[创建用户自定义标识机器组](/intl.zh-CN/数据采集/Logtail采集/机器组/创建用户自定义标识机器组.md)。|

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


## 步骤二：创建采集配置

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在接入数据区域，单击**正则-文本日志**。

    本文以采集正则-文本文件为例，其他文本文件采集请参见[概述](/intl.zh-CN/数据采集/Logtail采集/采集文本日志/概述.md)。

3.  选择目标Project和Logstore，单击**下一步**。

4.  创建机器组。

    如果您已有可用的机器组 ，可直接单击**使用现有机器组**。

    1.  确认已创建机器组，单击**确认安装完毕**。

    2.  配置机器组相关参数，单击**下一步**。

        **机器组标识**选择**用户自定义标识**，将[步骤一：安装Sidecar](#section_0t9_br8_3q0)时配置的`ALIYUN_LOGTAIL_USER_DEFINED_ID`填入**用户自定义标识**框中。

        ![配置机器组](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6330559951/p53026.png)

5.  选中目标机器组，将该机器组从**源机器组**移动到**应用机器组**，单击**下一步**。

    **说明：** 如果创建机器组后立刻应用，可能因为连接未生效，导致心跳为**FAIL**，您可单击**自动重试**。如果还未解决，请参见[Logtail机器组无心跳]()进行排查。

6.  设置Logtail配置。

    目前支持极简模式、Nginx访问日志、分隔符日志、JSON日志、正则日志等格式。具体操作，请参见[概述](/intl.zh-CN/数据采集/Logtail采集/采集文本日志/概述.md)。

    **说明：** **是否为Docker文件**选项需保持关闭。

    ![配置采集方式](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6330559951/p54674.png)

7.  配置查询与分析属性，单击**下一步**。

    默认已经创建索引，您也可以手动修改。


