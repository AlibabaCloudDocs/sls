# Suppress alerts

You can suppress alerts if you want to stop receiving notifications for similar alerts. For example, if a critical alert is triggered and you want to silence alerts whose severity is not critical, you can create a suppression policy.

## Overview

The objects of the alert suppression policy are the source alert and the destination alert. The source alert is the alert that triggers the alert. The destination alert is the alert that is suppressed. If you want to suppress alerts, the following conditions must be met:

-   The source alert and the destination alert must be in the same merge set.
-   The source alert must be in the triggered status.
-   The time that you specify to suppress an alert must be earlier than the time specified to send the alert notification. If the source alert is cleared before an alert notification is sent, the specified suppression policy becomes invalid. In this case, the alert notification is sent if conditions are met for the destination alert.

![Example](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5529872261/p255484.png)

## Example

If a Kubernetes cluster triggers a critical alert and you want to silence alerts whose severity is not critical, you can create a suppression policy. For more information, see [Create an alert policy](/intl.en-US/Alerting/Alerting (New)/Alert Management/Create an alert policy.md).

-   In the Condition dialog box, specify conditions for the source alert.
-   In the Denoise Alert dialog box, specify conditions for the destination alert.

![Suppress alerts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5529872261/p264677.png)

