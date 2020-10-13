# Use the console to collect Kubernetes stdout and stderr logs in DaemonSet mode

This topic describes how to configure Logtail in the Log Service console to collect Kubernetes stdout and stderr logs in DaemonSet mode.

The Helm package alibaba-log-controller is installed. For more information, see [Install Logtail](/intl.en-US/Data Collection/Logtail collection/Container log collection/Install Logtail.md).

## Features

Logtail can collect and upload container stdout and stderr logs with container metadata to Log Service. The collection of Kubernetes stdout and stderr logs has the following features:

-   Collects stdout and stderr logs in real time.
-   Uses labels to specify containers for log collection.
-   Uses labels to exclude containers from log collection.
-   Uses environment variables to specify containers for log collection.
-   Uses environment variables to exclude containers from log collection.
-   Supports multi-line logs such as Java Stack logs.
-   Supports automatic labeling for Docker container logs.
-   Supports automatic labeling for Kubernetes container logs.

**Note:**

-   The preceding labels are retrieved by using the docker inspect command. These labels are not the labels that are specified in a Kubernetes cluster.
-   The preceding environment variables are the environment variables that you have specified to start containers.

## Implementation

A Logtail container uses a UNIX domain socket to communicate with the Docker daemon. The Logtail container queries all Docker containers and locates the specified Docker containers based on specified labels and environment variables. Logtail collects the logs of the specified Docker containers by using the docker logs command.

When Logtail collects the stdout and stderr logs of a Docker container, Logtail periodically stores checkpoints to a checkpoint file. If Logtail is restarted, Logtail collects logs from the last checkpoint.

![Implementation](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8323359951/p2950.png)

## Limits

-   Logtail version: Only Logtail 0.16.0 or later that runs on Linux can be used to collect stdout and stderr logs. For more information about Logtail versions and version updates, see [Install Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md).
-   Permissions: By default, Logtail uses the `/var/run/docker.sock` socket to access the docker daemon. Make sure that a UNIX domain socket is available and the Logtail container has permissions to access the docker daemon.
-   Multi-line log entries: To ensure that a multi-line log entry is not split into multiple log entries due to output latency, the last collected multi-line log entry is cached for 3 seconds by default. You can set the cache time by specifying the `BeginLineTimeoutMs` parameter. The parameter value cannot be less than 1,000 ms. Otherwise, an error may occur.
-   Stop policy: When a container is stopped and Logtail detects the `die` event on the container, Logtail stops collecting stdout and stderr logs of the container. In this case, if a collection delay occurs, some stdout or stderr logs that are generated before the stop action may be lost.
-   Docker logging driver: The logging driver collects stdout and stderr logs only in JSON files.
-   Context: By default, logs that are collected from different containers by using a collection configuration file are in the same context. If you want the logs of each container to be in different contexts, create a Logtail configuration file for each container.
-   Data processing: The collected data is contained in the `content` field. The data can be processed by using a common processing method.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, click **Kubernetes - Standard Output**.

3.  In the Specify Logstore step, select the project and Logstore, and then click **Next**.

    You can also click **Create Now** to create a project and a Logstore.

4.  In the Create Machine Group step, create a machine group as prompted, and then click **Complete Installation**.

    If a machine group is available, click **Use Existing Machine Groups**.

5.  In the Machine Group Settings step, apply the configurations to the machine group.

    Select the created machine group and move the group from **Source Server Groups** to **Applied Server Groups**.

6.  In the Specify Data Source step, specify the data source, and then click **Next**.

    Enter log collection configurations in the Plug-in Config field. The following example shows the required parameters:

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

    The type of input data sources is `service_docker_stdout`.

    |Parameter|Type|Required|Description|
    |:--------|:---|:-------|:----------|
    |IncludeLabel|map|Yes|The value of the IncludeLabel parameter is a map. The keys and values of the map are strings. The default value of this parameter is an empty map. This default value indicates that logs from all containers are collected. If keys are not empty and values are empty, logs are collected from the containers whose label keys match the specified keys. **Note:**

    -   Key-value pairs are associated by the OR operator. If a label key-value pair of a container matches one of the specified key-value pairs, logs of the container are collected.
    -   By default, values in the map are strings. Logs are collected from the containers whose names match the values. If you use a regular expression to specify a value, logs are collected from the containers whose names match the regular expression. For example, if you specify a value that starts with a caret \(^\) and ends with a dollar sign \($\) such as ^\(kube-system\|istio-system\)$, logs are collected from the container named kube-system and the container named istio-system. |
    |ExcludeLabel|map|No|The value of the ExcludeLabel parameter is a map. The keys and values of the map are strings. The default value of this parameter is an empty map. This default value indicates that logs from all containers are collected. If keys are not empty and values are empty, logs are not collected from the containers whose label keys match the specified keys. **Note:**

    -   Key-value pairs are associated by the OR operator. If a label key-value pair of a container matches one of the specified key-value pairs, logs of the container are collected.
    -   By default, values in the map are strings. Logs are collected from the containers whose names match the values. If you use a regular expression to specify a value, logs are collected from the containers whose names match the regular expression. For example, if you specify a value that starts with a caret \(^\) and ends with a dollar sign \($\) such as ^\(kube-system\|istio-system\)$, logs are collected from the container named kube-system and the container named istio-system. |
    |IncludeEnv|map|No|The value of the IncludeEnv parameter is a map. The keys and values of the map are strings. The default value of this parameter is an empty map. This default value indicates that logs from all containers are collected. If keys are not empty and values are empty, logs are collected from the containers whose environment variable keys match the specified keys. **Note:**

    -   Key-value pairs are associated by the OR operator. If an environment variable key-value pair of a container matches one of the specified key-value pairs, logs of the container are collected.
    -   By default, values in the map are strings. Logs are collected from the containers whose names match the values. If you use a regular expression to specify a value, logs are collected from the containers whose names match the regular expression. For example, if you specify a value that starts with a caret \(^\) and ends with a dollar sign \($\) such as ^\(kube-system\|istio-system\)$, logs are collected from the container named kube-system and the container named istio-system. |
    |ExcludeEnv|map|No|The value of the ExcludeEnv parameter is a map. The keys and values of the map are strings. The default value of this parameter is an empty map. This default value indicates that logs from all containers are collected. If keys are not empty and values are empty, logs are not collected from the containers whose environment variable keys match the specified keys. **Note:**

    -   Key-value pairs are associated by the OR operator. If an environment variable key-value pair of a container matches one of the specified key-value pairs, logs of the container are collected.
    -   By default, values in the map are strings. Logs are collected from the containers whose names match the values. If you use a regular expression to specify a value, logs are collected from the containers whose names match the regular expression. For example, if you specify a value that starts with a caret \(^\) and ends with a dollar sign \($\) such as ^\(kube-system\|istio-system\)$, logs are collected from the container named kube-system and the container named istio-system. |
    |Stdout|bool|No|Default value: true. If you set the value of this parameter to false, stdout logs are not collected.|
    |Stderr|bool|No|Default value: true. If you set the value of this parameter to false, stderr logs are not collected.|
    |BeginLineRegex|string|No|The regular expression that is used to match a line for the first line of a log entry. The default value of this parameter is an empty string. If a line matches the specified regular expression, the line is considered the first line of a new log entry. Otherwise, the line is considered a part of the last log entry.|
    |BeginLineTimeoutMs|int|No|The timeout period for the specified regular expression to match a line. Default value: 3000. Unit: ms. If no new log entry appears within 3 seconds, the last log is uploaded.|
    |BeginLineCheckLength|int|No|The size of the first line of a log entry that matches the specified regular expression. Default value: 10 × 1,024. Unit: bytes. You can specify this parameter to check whether the start of the line matches the regular expression. This improves match efficiency.|
    |MaxLogSize|int|No|The maximum size of a log entry. Default value: 512 × 1,024. Unit: bytes. If the size of a log entry exceeds the specified value, the log entry is uploaded.|

    **Note:**

    -   The preceding IncludeLabel and ExcludeLabel parameters are included in the label information retrieved by using the docker inspect command.
    -   A namespace and a container name in a Kubernetes cluster can be mapped to Docker labels. The value of the LabelKey parameter for a namespace is `io.kubernetes.pod.namespace`. The value of the LabelKey parameter for a container name is `io.kubernetes.container.name`. For example, the namespace of the pod that you have created is backend-prod and the container name is worker-server. In this case, if you set the key-value pair of a whitelist label to `io.kubernetes.pod.namespace : backend-prod`, logs of all containers in the pod are collected. If you set the key-value pair of a whitelist label to `io.kubernetes.container.name : worker-server`, logs of the container are collected.
    -   In a Kubernetes cluster, we recommend that you specify only the `io.kubernetes.pod.namespace` and `io.kubernetes.container.name` labels. You can also specify the IncludeEnv or ExcludeEnv parameter based on your business requirements.
7.  In the Configure Query and Analysis step, specify the indexes, and then click **Next**.

    Indexes are created by default. You can modify the indexes as needed.


## Default fields

The following table lists the fields that are uploaded by default for each Kubernetes log entry.

|Field name|Description|
|:---------|:----------|
|`_time_`|The data upload time, for example, `2018-02-02T02:18:41.979147844Z`.|
|`_source_`|The type of input data sources. Valid values: stdout and stderr.|
|`_image_name_`|The name of an image.|
|`_container_name_`|The name of a container.|
|`_pod_name_`|The name of a pod.|
|`_namespace_`|The namespace where a pod resides.|
|`_pod_uid_`|The unique identifier of a pod.|
|`_container_id_`|The IP address of a pod.|

## Configuration examples of single-line log collection

-   Configure environment variables

    Collect the stdout and stderr logs of the containers that meet the following conditions: Environment variables include `NGINX_PORT_80_TCP_PORT=80` and exclude `POD_NAMESPACE=kube-system`.

    ![Configuration example of environment variables](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8323359951/p2951.png)

    The following script shows the log collection configurations:

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

-   Configure labels

    Collect the stdout and stderr logs of the containers that meet the following conditions: The container labels include `io.kubernetes.container.name=nginx` and exclude `type=pre`.

    ![Configuration example of labels ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8323359951/p2952.png)

    The following script shows the label configurations:

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


## Configuration examples of multi-line log collection

Before you can collect Java exception stack logs, you must configure multi-line log collection. The following section describes how to collect stdout and stderr logs of standard Java applications.

-   Sample log entry

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

-   Log collection configuration

    Collect the logs of the containers that meet the following conditions: The container labels include `app=monitor` and the specified first bytes of a line is of a fixed-format date type. To improve match efficiency, only the first 10 bytes of each line are checked.

    ```
    {
    "inputs": [
      {
        "detail": {
          "BeginLineCheckLength": 10,
          "BeginLineRegex": "\\d+-\\d+-\\d+. *",
          "IncludeLabel": {
            "app": "monitor"
          }
        },
        "type": "service_docker_stdout"
      }
    ]
    }
    ```


## Data processing examples

Logtail can process the collected Docker standard output. For more information, see [Process data](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Process data.md).

-   Collect the logs of the containers that meet the following conditions: The container labels include `app=monitor` and the specified first bytes of a line is of a fixed-format date type. To improve match efficiency, only the first 10 bytes of each line are checked. Regular expressions are used to parse logs into the values of time, level, module, thread, and message. The following script shows the configurations of log collection and data processing:

    ```
    {
    "inputs": [
      {
        "detail": {
          "BeginLineCheckLength": 10,
          "BeginLineRegex": "\\d+-\\d+-\\d+. *",
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

    The collected `2018-02-03 14:18:41.968 INFO [spring-cloud-monitor] [nio-8080-exec-4] c.g.s.web.controller.DemoController : service start done` log entry is processed, as shown in the following script:

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

-   Collect the JSON logs of the containers that meet the following conditions: The container labels include `app=monitor`. The following script shows the configurations of log collection and data processing:

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


