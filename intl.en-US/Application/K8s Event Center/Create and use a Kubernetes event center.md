# Create and use a Kubernetes event center

This topic describes how to create a Kubernetes event center. After you create a Kubernetes event center, you can view event statistics, query event details, view pod lifecycle, configure alerts, and perform custom queries.

Kubernetes event centers record the status changes of Kubernetes clusters. A status change is recorded when you create, run, or delete a pod, or when a component exception occurs. Kubernetes event centers aggregate all events in Kubernetes clusters in real time. This allows you to store, query, analyze, and visualize event data and configure alerts for these events.

## Billing

After a Logstore is associated with a Kubernetes event center, you can store event data in the Logstore free of charge for a maximum of 90 days. During this period, you can write 256 MB of data per day to the Logstore. The data size is equivalent to 250,000 events. In most cases, each online Kubernetes cluster generates 1,000 events per day. By default, events are retained for 90 days. Therefore, you can use the Kubernetes event center free of charge if you do not extend the event retention period. Examples:

-   If you do not extend the event retention period and the Kubernetes cluster generates 1,000 events per day, you can use the Kubernetes event center free of charge.
-   If you extend the event retention period to 105 days and the Kubernetes cluster generates 1,000 events per day, you are charged at a rate of USD 0.014 per day after 90 days. For more information, see [Pay-as-you-go](/intl.en-US/Pricing/Pay-as-you-go.md).

## Step 1: Create an event center

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Log Application section, click **K8s Event Center**.

3.  On the Event Centers Management tab, click **Add**.

4.  In the Add Event Center panel, set the required parameters.

    -   Select **Existing Project**, and then select a project from the **Project** drop-down list.
    -   Select **Kubernetes Cluster**, and then select a Kubernetes cluster from the **Kubernetes Cluster** drop-down list. If you use this method to create an event center, a project named k8s-log-\{cluster-id\} is automatically created for the event center.
5.  Click **Next**.

    **Note:** After you create an event center, a Logstore named k8s-event is automatically created in the specified project. Related dashboards and alerts are also created for the event center.


## Step 2: Deploy the eventer and node-problem-detector components

Before you can use the Kubernetes event center, you must deploy the eventer and node-problem-detector components in the Kubernetes cluster.

-   Deploy the eventer and node-problem-detector components in a Container Service for Kubernetes \(ACK\) cluster

    The node-problem-detector component in the Marketplace of ACK provides the node-problem-detector and event collection features. For more information, see [Scenario 3: Use node-problem-detector with kube-eventer to raise alerts upon node anomalies](/intl.en-US/User Guide for Kubernetes Clusters/Observability/Monitoring management/Event monitoring.md).

    1.  Log on to the [ACK console](https://cs.console.aliyun.com/).
    2.  In the left-side navigation pane, choose **Marketplace** \> **App Catalog**.
    3.  On the App Catalog page, click the Alibaba Cloud Apps tab. On this tab, click **ack-node-problem-detector**.
    4.  On the ack-node-problem-detector page, click the **Parameters** tab. On this tab, modify the parameter settings in the eventer section.

        -   enabled: Set the value of the enabled parameter under **eventer** \> **sinks** \> **sls** to true.
        -   topic: Optional. Set the value to your cluster name. The name can contain only letters, underscores \(\_\), and hyphens \(-\).
        -   project: Set the value to the name of the project that you select when you create the Kubernetes event center.
        -   logstore: Set the value to k8s-event.
        ```
          sinks:
             sls:
               enabled: true
               # If you want the monitoring results to be notified by sls, set enabled to true.
               topic: "my-cluster"
               project:  "{sls-project-name}"
               # You can view the project information by logging in to the
               # SLS console. Please fill in the name of the project here.
               # eg: your project name is k8s-log-cc18a5f3443dhdss22654da,
               # then you can fill k8s-log-cc18a5f3443dhdss22654da to project label.
               logstore: "k8s-event"
               # You can view the project information by logging in to the
               # SLS console. Please fill the logstore address in here.
        ```

    5.  Click **Create** to complete the deployment.
-   Deploy the eventer and node-problem-detector components in a self-managed Kubernetes cluster
    1.  Configure event collection. For more information, see [Collect Kubernetes events](/intl.en-US/Data Collection/Logtail collection/Container log collection/Collect Kubernetes events.md).
    2.  For more information about how to deploy the node-problem-detector component, visit [GitHub](https://github.com/kubernetes/node-problem-detector).

## Step 3: Use the event center

After you create a Kubernetes event center and deploy the eventer and node-problem-detector components, you can use the Kubernetes event center. You can use the event center to view event statistics, query event details, view pod lifecycle, configure alerts, and perform custom queries.

In the left-side navigation pane of the K8s Event Center page, find the event center and click the **![Kubernetes event center - 002](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1706498951/p77384.png)** icon to perform the following operations.

|Operation|Description|
|---------|-----------|
|View event statistics|Click **Event Overview** to view the statistics of important events. For example, you can view the number of error events \(compared with yesterday or last week\), statistics of warning events, trends of top 10 important events, and statistics of pod out of memory \(OOM\) events. **Note:** If an OOM event occurs, you cannot view the information of the related pod. You can view only the node, process name, and process ID of the event. However, you can query the pod restart event close to the time when the pod OOM event occurred. Then, you can find the pod. |
|Query event details|Click **Event Details**. On the Event Details tab, query events by specifying one or more search conditions such as level, type, target, host, namespace, and name. Then, you can view the statistics and details of the specified events.|
|View the lifecycle of a pod|Click **Pod Lifecycle**. On the page that appears, set the search conditions. The information of all events that occurred on the pod is displayed in a chart. You can filter warning events or error events of the pod by severity level.|
|Configure alerts|Click **Alert Configuration** to configure event alerts. For more information, see [Procedure](#step_nmt_vqo_f3p).|
|Perform custom queries|Click **Custom Query** to query events by using custom search conditions. For information about the query syntax, see [Overview](/intl.en-US/Index and query/Overview.md). All events in the event center are stored in the Logstore. You can use all features of the Logstore. For example, you can perform custom queries, process consumption events, and create custom reports or alerts.

To obtain the name of the project that is associated with the event center, use one of the following two methods:

-   Click Custom Query. On the page that appears, find the name of the project in the URL of the page. The URL of the page is in the `[https://sls.console.aliyun.com/lognext/app/k8s-event/project/k8s-log-xxxx/logsearch/k8s-event](https://sls.console.aliyun.com/lognext/app/k8s-event/project/k8s-log-c0ae5df15fbf34b47ba3a9684e6ee2bee/logsearch/k8s-event)` format. The k8s-log-xxxx field indicates the name of the project.
-   Click Event Center Management. On the Event Centers page, view the name of the related project. |
|Configure custom alerts|A Kubernetes event center supports system alerts. The Kubernetes event center also allows you to configure custom alerts. Click Custom Query. On the page that appears, enter a query statement to query a Kubernetes event, and then click **Save as Alert**. In the Alert Rule panel, set the required parameters. For more information, see [Alert overview](/intl.en-US/Query and visualization/Alarm/Overview.md).

For example, to create an alert named FailedPreStopHook, enter the query statement `* and FailedPreStopHook | SELECT "object-namespace", "object-name", "reason", "message"` in the search box on the page. Then, click **Save as Alert**. In the Alert Rule panel, set the required parameters and click OK.

**Note:** If the alert name is prefixed by K8s, you can view the alert in the Events section of the Alert Configuration tab. Otherwise, you can view the alert only in the Modify Alert panel. |

The following procedure describes how to configure an alert:

1.  In the left-side navigation pane of the K8s Event Center page, find the event center and click the **![Kubernetes event center - 002](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1706498951/p77384.png)** icon.

2.  Click **Alert Configuration** to open the Alert Configuration tab.

3.  Add a notification method.

    1.  Click **Add Notification Method**.

    2.  In the Add Notification Method panel, set the required parameters.

        |Parameter|Description|
        |---------|-----------|
        |Name|The name of the notification method.|
        |Alert Interval|The interval between two consecutive alert notifications. Default value: 5. Unit: minutes. **Note:** To avoid the receipt of excessive alerts, we recommend that you set the alert interval to at least 2 minutes. |
        |ActionType|The notification type. Valid values: SMS, Voice, Email, WebHook-DingTalk Bot, WebHook-Custom, and Notifications. You can select one or more notification methods. For more information, see [Configure notification methods](/intl.en-US/Query and visualization/Alarm/Configure notification methods.md).|

    3.  Click **OK**.

4.  Enable the alert notification feature.

    1.  In the Events section, click **Modify**.

    2.  Find the event for which you want to enable the alert notification feature, turn on the enable switch, and then select an alert notification method.

        **Note:** We recommend that you enable the alert notification feature for all events. If excessive alert notifications are sent, you can disable the feature for some events or increase the notification interval.

    3.  Click **Save**.


## Delete an event center

Choose **K8s Event Center** \> **Event Center Management**. On the Event Center Management tab, find the event center that you want to delete, and then click the **![Kubernetes Event Center](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1706498951/p85814.png)** icon in the Actions column.

## FAQ

-   Why is data unavailable in a Kubernetes event center?

    After you deploy a Kubernetes event center, new events are collected to the event center. You can click Custom Query to query the events. We recommend that you set the query time range to 1 Day. If no data is available in the Kubernetes event center, this issue may occur based on the following two scenarios:

    -   After you deploy the Kubernetes event center, no events are generated in the associated Kubernetes cluster.

        You can run the `kubectl get events --all-namespaces` command to check whether new events are generated in the cluster.

    -   The specified parameter values are invalid when you deploy the eventer and node-problem-detector components.
        -   If you use an ACK cluster, log on to the ACK console. In the left-side navigation pane, choose **Applications** \> **Releases**. On the page that appears, select the related cluster from the Cluster drop-down list. Then, click **Update** to view the configurations of the **ack-node-problem-detector** component. For more information, see [Step 2: Deploy the eventer and node-problem-detector components](#section_7eu_9y2_50c).
        -   If you use a self-managed Kubernetes cluster, for more information about the parameter settings, see [Collect Kubernetes events](/intl.en-US/Data Collection/Logtail collection/Container log collection/Collect Kubernetes events.md).
-   How do I view the logs of the pod that is associated with an event?
    -   If you use an ACK cluster, log on to the ACK console. In the left-side navigation pane, choose **Applications** \> **Pods**. On the page that appears, select the associated cluster from the Cluster drop-down list, and select **kube-system** from the **Namespace** drop-down list. Enter the eventer keyword in the search box to search for the target pod. Click View Details to view the logs of the pod.
    -   If you use a self-managed Kubernetes cluster, view the logs of the pod whose name is prefixed by event-sls. The pod is in the **kube-system** namespace.

