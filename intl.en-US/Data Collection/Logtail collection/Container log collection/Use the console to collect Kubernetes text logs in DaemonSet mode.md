# Use the console to collect Kubernetes text logs in DaemonSet mode

This topic describes how to configure Logtail in the Log Service console to collect Kubernetes text logs in DaemonSet mode.

The Helm package alibaba-log-controller is installed. For more information, see [Install Logtail](/intl.en-US/Data Collection/Logtail collection/Container log collection/Install Logtail.md).

## Features

Logtail can collect and upload container text logs with container metadata to Log Service. The collection of Kubernetes text logs has the following features:

-   Allows you to specify the log path of a container without the need to manually map the path to a path on the host.
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

## Limits

-   Stop policy: When a container is stopped and Logtail detects the `die` event on the container, Logtail stops collecting logs from the container. In this case, if a collection delay occurs, some logs that are generated before the stop action may be lost.
-   Docker storage driver: Only overlay and overlay2 are supported. For other storage drivers, you must mount the log directory on the local host.
-   Symbolic link: Logtail cannot access the symbolic link of a container. Set the collection directory to an actual path.

## Configure log collection

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, click **Kubernetes - Object**.

3.  In the Specify Logstore step, select the project and Logstore, and then click **Next**.

    You can also click **Create Now** to create a project and a Logstore.

4.  In the Create Machine Group step, create a machine group as prompted, and then click **Complete Installation**.

    If a machine group is available, click **Use Existing Machine Groups**.

5.  In the Machine Group Settings step, apply the configurations to the machine group.

    Select the created machine group and move the group from **Source Server Groups** to **Applied Server Groups**.

6.  In the Logtail Config step, specify the data source, and then click **Next**. The following table describes the required parameters.

    |Parameter|Required|Description|
    |:--------|:-------|:----------|
    |Docker File|Yes|Checks whether the file that is collected from the specified data source is a Docker file.|
    |Label Whitelist|No|If you specify a label whitelist, the LabelKey parameter must be specified. If the value of the LabelValue parameter is not empty, logs are collected from the containers whose label key-value pairs match the specified key-value pairs. If the value of the LabelValue parameter is empty, logs are collected from the containers whose label keys match the specified keys. **Note:**

    -   Key-value pairs are associated by the OR operator. If a label key-value pair of a container matches one of the specified key-value pairs, logs of the container are collected.
    -   By default, the value of the LabelValue parameter is a string. Logs are collected from the container whose name matches the value of the LabelValue parameter. If you use a regular expression to specify the value of the LabelValue parameter, logs are collected from the containers whose names match the regular expression. For example, if you specify the parameter value that starts with a caret \(^\) and ends with a dollar sign \($\) such as ^\(kube-system\|istio-system\)$, logs are collected from the container named kube-system and the container named istio-system.
    -   Do not specify the duplicate values for the LabelKey parameter. Otherwise, the existing value of the LabelValue parameter is overwritten. |
    |Label Blacklist|No|If you specify a label blacklist, the LabelKey parameter must be specified. If the value of the LabelValue parameter is not empty, logs are not collected from the containers whose label key-value pairs match the specified key-value pairs. If the value of the LabelValue parameter is empty, logs are not collected from the containers whose label keys match the specified keys. **Note:**

    -   Key-value pairs are associated by the OR operator. If a label key-value pair of a container matches one of the specified key-value pairs, logs of the container are collected.
    -   By default, the value of the LabelValue parameter is a string. Logs are collected from the container whose name matches the value of the LabelValue parameter. If you use a regular expression to specify the value of the LabelValue parameter, logs are collected from the containers whose names match the regular expression. For example, if you specify the parameter value that starts with a caret \(^\) and ends with a dollar sign \($\) such as ^\(kube-system\|istio-system\)$, logs are collected from the container named kube-system and the container named istio-system.
    -   Do not specify the duplicate values for the LabelKey parameter. Otherwise, the existing value of the LabelValue parameter is overwritten. |
    |Environment Variable Whitelist|No|If you specify an environment variable whitelist, the EnvKey parameter must be specified. If the value of the EnvValue parameter is not empty, logs are collected from the containers whose environment variable key-value pairs match the specified key-value pairs. If the value of the EnvValue parameter is empty, logs are collected from the containers whose environment variable keys match the specified keys. **Note:**

    -   Key-value pairs are associated by the OR operator. If an environment variable key-value pair of a container matches one of the specified key-value pairs, logs of the container are collected.
    -   By default, the value of the EnvValue parameter is a string. Logs are collected from the container whose name matches the value of the EnvValue parameter. If you use a regular expression to specify the value of the EnvValue parameter, logs are collected from the containers whose names match the regular expression. For example, if you specify the value of the EnvValue parameter that starts with a caret \(^\) and ends with a dollar sign \($\) such as ^\(kube-system\|istio-system\)$, logs are collected from the container named kube-system and the container named istio-system. |
    |Environment Variable Blacklist|No|If you specify an environment variable blacklist, the EnvKey parameter must be specified. If the value of the EnvValue parameter is not empty, logs are not collected from the containers whose environment variable key-value pairs match the specified key-value pairs. If the value of the EnvValue parameter is empty, logs are not collected from the containers whose environment variable keys match the specified keys. **Note:**

    -   Key-value pairs are associated by the OR operator. If an environment variable key-value pair of a container matches one of the specified key-value pairs, logs of the container are collected.
    -   By default, the value of the EnvValue parameter is a string. Logs are collected from the container whose name matches the value of the EnvValue parameter. If you use a regular expression to specify the value of the EnvValue parameter, logs are collected from the containers whose names match the regular expression. For example, if you specify the value of the EnvValue parameter that starts with a caret \(^\) and ends with a dollar sign \($\) such as ^\(kube-system\|istio-system\)$, logs are collected from the container named kube-system and the container named istio-system. |
    |Other parameters|N/A|For more information about other parameters, see [Overview](/intl.en-US/Data Collection/Logtail collection/Text logs/Overview.md).|

    **Note:**

    -   A namespace and a container name in a Kubernetes cluster can be mapped to Docker labels. The value of the LabelKey parameter for a namespace is `io.kubernetes.pod.namespace`. The value of the LabelKey parameter for a container name is `io.kubernetes.container.name`. For example, the namespace of the pod that you have created is backend-prod and the container name is worker-server. In this case, you can set the key-value pair of a whitelist label to `io.kubernetes.pod.namespace : backend-prod` or `io.kubernetes.container.name : worker-server`. Then, you can collect logs from only the worker-server container.
    -   In a Kubernetes cluster, we recommend that you specify only the `io.kubernetes.pod.namespace` and `io.kubernetes.container.name` labels. You can also specify the Environment Variable Whitelist or Environment Variable Blacklist parameter based on your business requirements.
7.  In the Configure Query and Analysis step, specify the indexes, and then click **Next**.

    Indexes are created by default. You can modify the indexes as needed.


## Configuration examples

-   Configure environment variables

    Collect the logs of the containers that meet the following conditions: The environment variables include `NGINX_PORT_80_TCP_PORT=80` and exclude `POD_NAMESPACE=kube-system`, the log file path is `/var/log/nginx/access.log`, and logs are parsed in simple mode.

    ![Configuration example of environment variables](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6323359951/p54512.png)

    The following figure shows the configuration of a data source. For more information about log collection modes, see [Use the full regex mode to collect logs](/intl.en-US/Data Collection/Logtail collection/Text logs/Use the full regex mode to collect logs.md), [Collect NGINX logs](/intl.en-US/Data Collection/Logtail collection/Text logs/Collect NGINX logs.md), and [Collect DSV formatted logs](/intl.en-US/Data Collection/Logtail collection/Text logs/Collect DSV formatted logs.md).

    ![Configuration example of a data source](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2996549951/p54511.png)

-   Configure labels

    Collect the logs of the containers that meet the following conditions: The container labels include `io.kubernetes.container.name=nginx`, the log file path is `/var/log/nginx/access.log`, and logs are parsed in simple mode.

    ![Configuration example of labels](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6323359951/p54510.png)

    The following figure shows the configuration of a data source. For more information about log collection modes, see [Use the full regex mode to collect logs](/intl.en-US/Data Collection/Logtail collection/Text logs/Use the full regex mode to collect logs.md), [Collect NGINX logs](/intl.en-US/Data Collection/Logtail collection/Text logs/Collect NGINX logs.md), and [Collect DSV formatted logs](/intl.en-US/Data Collection/Logtail collection/Text logs/Collect DSV formatted logs.md).

    ![Data source configuration](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2996549951/p54509.png)


## Default fields

The following table lists the fields that are uploaded by default for each log entry.

|Field name|Description|
|:---------|:----------|
|\_image\_name\_|The name of an image.|
|\_container\_name\_|The name of a container.|
|\_pod\_name\_|The name of a pod.|
|\_namespace\_|The namespace to which a pod belongs.|
|\_pod\_uid\_|The unique identifier of a pod.|
|\_container\_ip\_|The IP address of a pod.|

