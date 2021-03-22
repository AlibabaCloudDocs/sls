# 配置Ingress日志中心

本文介绍如何开通Ingress访问日志中心，将Ingress日志实时采集到日志服务中并进行可视化分析。

已安装日志组件，详情请参见[安装Logtail日志组件](https://help.aliyun.com/document_detail/157317.html)。

默认情况下，在创建Kubernetes集群时自动安装日志组件。

## 步骤1：部署Ingress采集配置

日志服务采集配置针对Kubernetes进行了CRD扩展，alibaba-log-controller组件会根据您定义的AliyunLogConfig CRD自动创建日志服务相关采集配置和报表资源。

1.  在Kubernetes集群中，定义AliyunLogConfig CRD配置。

    **说明：**

    -   请确保日志组件alibaba-log-controller版本不低于0.2.0.0-76648ee-aliyun。

        如果您在应用了CRD配置后要更新组件版本，请在更新组件版本后，删除该CRD配置并重新应用。

    -   此处的CRD配置只对ACK默认的Ingress Controller中的访问日志格式生效。如果您修改过Ingress Controller的访问日志格式，请修改此处CRD配置中的正则表达式提取processor\_regex部分，具体修改内容请参见[通过DaemonSet-CRD方式采集日志](/cn.zh-CN/数据采集/Logtail采集/采集容器日志/通过DaemonSet-CRD方式采集日志.md)中的CRD配置。
    -   如果您当前没有其他系统依赖访问日志，则推荐您将访问日志格式设置为日志服务推荐的格式。设置方式：执行kubectl edit configmap -n kube-system nginx-configuration命令修改configmap，将其中的log-format-upstream字段修改为如下内容：

        ```
        log-format-upstream: $the_real_ip - [$the_real_ip] - $remote_user [$time_local] "$request" $status 
        $body_bytes_sent "$http_referer" "$http_user_agent" $request_length $request_time [$proxy_upstream_name] 
        $upstream_addr $upstream_response_length $upstream_response_time $upstream_status $req_id $host
        ```

    ```
    apiVersion: log.alibabacloud.com/v1alpha1
    kind: AliyunLogConfig
    metadata:
      # your config name, must be unique in you k8s cluster
      name: k8s-nginx-ingress
    spec:
      # logstore name to upload log
      logstore: nginx-ingress
      # product code, only for k8s nginx ingress
      productCode: k8s-nginx-ingress
      # logtail config detail
      logtailConfig:
        inputType: plugin
        # logtail config name, should be same with [metadata.name]
        configName: k8s-nginx-ingress
        inputDetail:
          plugin:
            inputs:
            - type: service_docker_stdout
              detail:
                IncludeLabel:
                  io.kubernetes.container.name: nginx-ingress-controller
                Stderr: false
                Stdout: true
            processors:
            - type: processor_regex
              detail:
                KeepSource: false
                Keys:
                - client_ip
                - x_forward_for
                - remote_user
                - time
                - method
                - url
                - version
                - status
                - body_bytes_sent
                - http_referer
                - http_user_agent
                - request_length
                - request_time
                - proxy_upstream_name
                - upstream_addr
                - upstream_response_length
                - upstream_response_time
                - upstream_status
                - req_id
                - host
                - proxy_alternative_upstream_name
                NoKeyError: true
                NoMatchError: true
                Regex: ^(\S+)\s-\s\[([^]]+)]\s-\s(\S+)\s\[(\S+)\s\S+\s"(\w+)\s(\S+)\s([^"]+)"\s(\d+)\s(\d+)\s"([^"]*)"\s"([^"]*)"\s(\S+)\s(\S+)+\s\[([^]]*)]\s(\S+)\s(\S+)\s(\S+)\s(\S+)\s(\S+)\s*(\S*)\s*\[*([^]]*)\]*.*
                SourceKey: content
    ```

2.  部署Ingress采集配置。

    您可以选择如下任意一种方式进行部署：

    -   方式1：执行kubectl命令完成部署。
    -   方式2：将[步骤1](#step_3jn_t3d_pmt)中的AliyunLogConfig CRD配置保存为nginx-ingress.yaml文件，执行kubectl apply -n kube-system -f命令完成部署。
    -   方式3：使用编排模板完成部署。
        1.  登录[容器服务管理控制台](https://cs.console.aliyun.com)。
        2.  将[步骤1](#step_3jn_t3d_pmt)中的AliyunLogConfig CRD配置保存为编排模板，详情请参见[创建编排模板](/cn.zh-CN/Kubernetes集群用户指南/应用市场/模板管理/创建编排模板.md)。
        3.  基于您所创建的模板创建应用，详情请参见[通过编排模板创建Linux应用](/cn.zh-CN/Kubernetes集群用户指南/应用/工作负载/创建无状态工作负载Deployment.md)。

            其中**命名空间**选择为您所在集群的默认命名空间。


## 步骤2：添加日志中心

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在日志应用区域，单击**Ingress日志中心**中的**进入应用**。

3.  在巡检管理页面，单击**添加**。

4.  在日志接入页面中，配置如下参数，并单击**下一步**。

    |参数|说明|
    |--|--|
    |日志中心名称|配置日志中心名称。|
    |项目Project|选择您已创建的Project。|
    |日志库Logstore|选择您已创建的Logstore，该Logstore需与[步骤1：部署Ingress采集配置](#section_q3e_o1s_96m)中配置的Logstore保持一致。|

5.  在时序转换配置中，保持默认配置，单击**下一步**。

6.  在巡检配置中，保持默认配置，单击**下一步**。

7.  单击**完成**。


配置完成后，您可在Ingress日志中心查看相关的报表并进行日志的查询分析、下载、投递、加工、告警等操作，详情请参见[云产品日志通用操作](/cn.zh-CN/数据采集/云产品日志采集/云产品日志通用操作.md)。您还可以执行监控数据的查询分析、告警等操作，详情请参见[查询分析时序数据](/cn.zh-CN/时序存储/查询与分析/查询分析时序数据.md)。

