# Overview

The alert management system is a subsystem that is used to manage denoising policies and alert statuses.

## Implementation

Alerts are sent from the alert monitoring system to the alert management system. The alert management system silences, suppresses, and deduplicates alerts based on alert policies. Then, the processed alerts are sent to the notification management system. You can manage alert incidents in the alert management system. The alert management system also provides dashboards such as the Alert Link Center and the Monitoring Rule Center.

The following figure shows the architecture of the alert management system

![Alert management](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3219872261/p254202.png)

## Alert policies

Alert policies are configuration entities of the alert management system. If the alert management system receives alerts \(including recovery notifications\), the system denoises alerts based on alert policies.

![Alert policies](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3219872261/p264112.png)

-   Merge alerts

    You can merge alerts that have similar attributes to a group. This way, you can prevent alert storms, manage alerts, and perform further operations in a unified manner. For more information, see [Merge alerts]().

    ![Group alerts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3219872261/p254204.png)

-   Suppress alerts

    If multiple alerts are triggered by an alert monitoring rule, you can suppress the alert notifications. For example, a network exception might cause service anomalies, such as a high request latency, a corn-chain exception, or an exception on non-corn chain services. In this case, you might want to receive only critical alert notifications. To do so, you can suppress alerts to avoid receiving all alert notifications at the same time. For more information, see [Suppress alerts]().

    ![Suppress alerts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3219872261/p254205.png)

-   Silence alerts

    You can specify a time range during which no alert notifications can be sent. For example, you can set a silence period if a large number of alerts are triggered during a maintenance and test period. This way, you can avoid receiving the alert notifications. For more information, see [Silence policies]().

    ![Silence alerts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3219872261/p254206.png)

-   Inherit alert policies

    You can inherit alert policies. If you inherit alert policies, alerts are processed based on the combination of the parent policy and the child policy. For more information, see [Inherit alert policies]().

-   Data isolation

    Data of different alert policies is isolated.


