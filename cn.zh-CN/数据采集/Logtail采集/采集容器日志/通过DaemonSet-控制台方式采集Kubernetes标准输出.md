# 通过DaemonSet-控制台方式采集Kubernetes标准输出

本文介绍如何通过控制台创建采集配置，并以DaemonSet采集方式采集Kubernetes标准输出。

已安装alibaba-log-controller Helm，详情请参见[安装Logtail日志组件](/cn.zh-CN/数据采集/Logtail采集/采集容器日志/安装Logtail日志组件.md)。

## 功能特点

Logtail支持采集容器内产生的标准输出，并附加容器的相关元数据信息一起上传到日志服务。标准输出文件采集具备以下功能特点。

-   支持采集标准输出文件（stdout）、标准出错文件（stderr）。
-   支持通过Label指定采集的容器。
-   支持通过Label指定排除的容器。
-   支持通过环境变量指定采集的容器。
-   支持通过环境变量指定排除的容器。
-   支持多行日志（例如java stack日志等）。
-   支持Docker容器日志自动打标签。
-   支持Kubernetes容器日志自动打标签。

**说明：**

-   本文的Label为Docker inspect中的Label，并不是Kubernetes配置中的Label。
-   本文中的环境变量为容器启动中配置的环境变量信息。

## 实现原理

Logtail与Docker的Domain Socket进行通信，查询该Docker上运行的所有容器，并根据容器配置的Label和环境变量信息定位需要被采集的容器。Logtail会通过docker logs命令获取指定容器的日志。

Logtail在采集容器的标准输出时，会定期将采集的点位信息保存到checkpoint文件中，若Logtail停止后再次启动，会从上一次保存的点位开始采集日志。

![实现原理](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2330559951/p2950.png)

## 使用限制

-   此功能目前仅支持Linux，依赖Logtail 0.16.0及以上版本，版本查看与升级参见[安装Logtail（Linux系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)。
-   Logtail默认通过`/var/run/docker.sock`访问Docker，请确保该Domain Socket存在且具备访问权限。
-   多行日志限制：为保证多行组成的一条日志不因为输出延迟而被分割成多条，多行日志情况下，采集的最后一条日志默认都会缓存一段时间。默认缓存时间为3秒，可通过`BeginLineTimeoutMs`设置，但此值不能低于1000（毫秒），否则容易出现误判。
-   采集停止策略：当容器被停止后，Logtail监听到容器`die`的事件后会停止采集该容器的标准输出，若此时采集出现延迟，则可能丢失停止前的部分输出。
-   Docker日志驱动类型限制：目前标准输出采集仅支持JSON类型的日志驱动。
-   上下文限制：默认一个采集配置在同一上下文中，若需要每个容器的日志在不同上下文中，请单独为每个容器创建采集配置。
-   数据处理：采集到的数据默认字段为`content`，支持通用的处理配置。请参见[处理数据](/cn.zh-CN/数据采集/Logtail采集/使用Logtail插件采集数据/处理数据.md)配置一种或多种处理方式。

## 创建采集配置

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在接入数据区域，单击**Kubernetes标准输出-容器**。

3.  在选择日志空间页签中，选择目标Project和Logstore，单击**下一步**。

    您也可以单击**立即创建**，重新创建Project和Logstore。

4.  创建机器组，创建完成后单击**确认安装完毕**。

    如果您已有可用的机器组 ，可直接单击**使用现有机器组**。

5.  在机器组配置页签中，应用机器组。

    选择一个机器组，将该机器组从**源机器组**移动到**应用机器组**。

6.  设置数据源，单击**下一步**。

    在插件配置中填写您的采集配置信息，示例如下所示。

    ```
    {
     "inputs": [
         {
             "type": "service_docker_stdout",
             "detail": {
                 "Stdout": true,
                 "Stderr": true,
                 "IncludeLabel": {
                     "io.kubernetes.container.name": "nginx"
                 },
                 "ExcludeLabel": {
                     "io.kubernetes.container.name": "nginx-ingress-controller"
                 },
                 "IncludeEnv": {
                     "NGINX_SERVICE_PORT": "80"
                 },
                 "ExcludeEnv": {
                     "POD_NAMESPACE": "kube-system"
                 }
             }
         }
     ]
    }
    ```

    该输入源类型为： `service_docker_stdout`。

    |配置项|类型|是否必选|说明|
    |:--|:-|:---|:-|
    |IncludeLabel|map类型，其中key为string，value为string|必选|默认为空，表示采集所有容器日志。如果key非空，value为空时，则采集label中所有包含此Key的容器数据。 **说明：**

    -   多个键值对之间为或关系，即只要容器的label满足任一键值对即可被采集。
    -   value默认为字符串匹配，即只有value和容器名称完全相同才会匹配。如果该值以^开头并且以$结尾，则为正则匹配，例如：value配置为^\(kube-system\|istio-system\)$，可同时匹配kube-system和istio-system。 |
    |ExcludeLabel|map类型，其中key为string，value为string|可选|默认为空，表示不排除任何容器。如果key非空，value为空时，则排除label中所有包含此key的容器。 **说明：**

    -   多个键值对之间为或关系，即只要容器的label满足任一键值对即可被采集。
    -   value默认为字符串匹配，即只有value和容器名称完全相同才会匹配。如果该值以^开头并且以$结尾，则为正则匹配，例如：value配置为^\(kube-system\|istio-system\)$，可同时匹配kube-system和istio-system。 |
    |IncludeEnv|map类型，其中key为string，value为string|可选|默认为空，表示采集所有容器数据。当key非空，value为空时，则采集environment中所有包含此key的容器。 **说明：**

    -   多个键值对之间为或关系，即只要容器的变量环境满足任一键值对即可被采集。
    -   value默认为字符串匹配，即只有value和容器名称完全相同才会匹配。如果该值以^开头并且以$结尾，则为正则匹配，例如：value配置为^\(kube-system\|istio-system\)$，可同时匹配kube-system和istio-system。 |
    |ExcludeEnv|map类型，其中key为string，value为string|可选|默认为空，表示不排除任何容器。当key非空，value为空时，则排除Environment中所有包含此key的容器。 **说明：**

    -   多个键值对之间为或关系，即只要容器的变量环境满足任一键值对即可被采集。
    -   value默认为字符串匹配，即只有value和容器名称完全相同才会匹配。如果该值以^开头并且以$结尾，则为正则匹配，例如：value配置为^\(kube-system\|istio-system\)$，可同时匹配kube-system和istio-system。 |
    |Stdout|bool|可选|默认为true。设置为false，表示不采集stdout数据。|
    |Stderr|bool|可选|默认为true。设置为false，表示不采集stderr数据。|
    |BeginLineRegex|string|可选|默认为空，表示行首匹配的正则表达式。若该表达式匹配某行，则将该行作为新的一条日志；否则将此行数据连接到上一条日志。|
    |BeginLineTimeoutMs|int|可选|行首匹配超时时间，默认为3000，单位为毫秒。若3秒内没有新日志出现，则将最后一条日志输出。|
    |BeginLineCheckLength|int|可选|行首匹配的长度，默认为10×1024，单位为字节。如果行首规则在前N个字节即可体现，可设置此参数，以此提升行首匹配效率。|
    |MaxLogSize|int|可选|日志最大长度，默认为512×1024，单位为字节。若日志超过该项配置，则不继续查找行首，直接上传。|

    **说明：**

    -   本文档中IncludeLabel、ExcludeLabel为Docker inspect中的Label信息。
    -   Kubernetes中的namespace和容器名会映射到Docker的Label中，分别为`io.kubernetes.pod.namespace`和`io.kubernetes.container.name`。例如您创建的pod所属namespace为backend-prod，容器名为worker-server，如果配置Label白名单为`io.kubernetes.pod.namespace : backend-prod`，则收集对应pod中所有容器的日志，如果配置Label白名单为`io.kubernetes.container.name : worker-server`，则收集对应容器的日志。
    -   Kubernetes不建议使用除`io.kubernetes.pod.namespace`和`io.kubernetes.container.name`之外的其他Label。其他情况请使用IncludeEnv/ExcludeEnv。
7.  配置查询分析，单击**下一步**。

    默认已经创建索引，您也可以手动修改。


## 默认字段

Kubernetes集群的每条日志默认上传的字段如下所示。

|字段名|说明|
|:--|:-|
|`_time_`|数据上传时间，例如`2018-02-02T02:18:41.979147844Z`|
|`_source_`|输入源类型，stdout或stderr|
|`_image_name_`|镜像名|
|`_container_name_`|容器名|
|`_pod_name_`|pod名|
|`_namespace_`|pod所在命名空间|
|`_pod_uid_`|pod的唯一标识|
|`_container_id_`|pod的IP地址|

## 普通日志采集配置示例

-   环境变量配置方式

    采集环境变量为`NGINX_PORT_80_TCP_PORT=80`且不为`POD_NAMESPACE=kube-system`的stdout以及stderr日志。

    ![环境变量配置方式](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2330559951/p2951.png)

    采集配置示例如下所示。

    ```
    {
        "inputs": [
            {
                "type": "service_docker_stdout",
                "detail": {
                    "Stdout": true,
                    "Stderr": true,
                    "IncludeEnv": {
                        "NGINX_PORT_80_TCP_PORT": "80"
                    },
                    "ExcludeEnv": {
                        "POD_NAMESPACE": "kube-system"
                    }
                }
            }
        ]
    }
    ```

-   Label配置方式

    采集Label为`io.kubernetes.container.name=nginx`且不为`type=pre`的stdout以及stderr日志。

    ![Label配置方式 ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2330559951/p2952.png)

    采集配置示例如下所示。

    ```
    {
        "inputs": [
            {
                "type": "service_docker_stdout",
                "detail": {
                    "Stdout": true,
                    "Stderr": true,
                    "IncludeLabel": {
                        "io.kubernetes.container.name": "nginx"
                    },
                    "ExcludeLabel": {
                        "type": "pre"
                    }
                }
            }
        ]
    }
    ```


## 多行日志采集配置示例

多行日志采集对于Java异常堆栈输出的采集尤为重要，这里介绍一种标准Java的标准输出日志的采集配置。

-   日志示例

    ```
    2018-02-03 14:18:41.968  INFO [spring-cloud-monitor] [nio-8080-exec-4] c.g.s.web.controller.DemoController : service start
    2018-02-03 14:18:41.969 ERROR [spring-cloud-monitor] [nio-8080-exec-4] c.g.s.web.controller.DemoController : java.lang.NullPointerException
    at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193)
    at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166)
    at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:199)
    at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:96)
    ...
    2018-02-03 14:18:41.968  INFO [spring-cloud-monitor] [nio-8080-exec-4] c.g.s.web.controller.DemoController : service start done
    ```

-   采集配置

    采集label为`app=monitor`的输入日志，日志以固定格式的日期开头（为提高匹配效率，这里只判断行首的10个字节）。

    ```
    {
    "inputs": [
      {
        "detail": {
          "BeginLineCheckLength": 10,
          "BeginLineRegex": "\\d+-\\d+-\\d+.*",
          "IncludeLabel": {
            "app": "monitor"
          }
        },
        "type": "service_docker_stdout"
      }
    ]
    }
    ```


## 采集数据处理示例

Logtail对于采集到的Docker标准输出，支持数据处理，详情请参见[处理数据](/cn.zh-CN/数据采集/Logtail采集/使用Logtail插件采集数据/处理数据.md)。

-   采集Label为`app=monitor`输入的日志，日志以固定格式的日期开头（为提高匹配效率，这里只判断行首的10个字节）。使用正则表达式将日志解析成time、level、module、thread、message，采集配置和处理配置如下所示：

    ```
    {
    "inputs": [
      {
        "detail": {
          "BeginLineCheckLength": 10,
          "BeginLineRegex": "\\d+-\\d+-\\d+.*",
          "IncludeLabel": {
            "app": "monitor"
          }
        },
        "type": "service_docker_stdout"
      }
    ],
    "processors": [
        {
            "type": "processor_regex",
            "detail": {
                "SourceKey": "content",
                "Regex": "(\\d+-\\d+-\\d+ \\d+:\\d+:\\d+\\.\\d+)\\s+(\\w+)\\s+\\[([^]]+)]\\s+\\[([^]]+)]\\s+:\\s+([\\s\\S]*)",
                "Keys": [
                    "time",
                    "level",
                    "module",
                    "thread",
                    "message"
                ],
                "NoKeyError": true,
                "NoMatchError": true,
                "KeepSource": false
            }
        }
    ]
    }
    ```

    例如日志`2018-02-03 14:18:41.968 INFO [spring-cloud-monitor] [nio-8080-exec-4] c.g.s.web.controller.DemoController : service start done`，经过处理后输出内容如下所示：

    ```
    __tag__:__hostname__:logtail-dfgef
    _container_name_:monitor
    _image_name_:registry.cn-hangzhou.aliyuncs.xxxxxxxxxxxxxxx
    _namespace_:default
    _pod_name_:monitor-6f54bd5d74-rtzc7
    _pod_uid_:7f012b72-04c7-11e8-84aa-00163f00c369
    _source_:stdout
    _time_:2018-02-02T14:18:41.979147844Z
    time:2018-02-02 02:18:41.968
    level:INFO
    module:spring-cloud-monitor
    thread:nio-8080-exec-4
    class:c.g.s.web.controller.DemoController
    message:service start done
    ```

-   采集label为`app=monitor`输入的日志，日志为JSON格式，采集配置和处理配置如下所示：

    ```
    {
    "inputs": [
      {
        "detail": {
          "IncludeLabel": {
            "app": "monitor"
          }
        },
        "type": "service_docker_stdout"
      }
    ],
    "processors": [
        {
            "type": "processor_json",
            "detail": {
                "SourceKey": "content",
                "NoKeyError":true,
                "KeepSource": false
            }
        }
    ]
    }
    ```


