# Escalate alert incidents

If an alert incident is not confirmed or resolved within a specified time range, the incident is processed based on the action policies that are specified on the Second Action Policy tab. This way, the alert can be handled at the earliest opportunity.

You can confirm, resolve, or ignore alert incidents on the Alert Rules/Incidents tab. The following figure shows alert incident phases.

**Note:** If the alert management system receives a specific recovery notification for an alert, the incident status automatically switches to the complete \(resolved\) status.

![Switch incident phases](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5639872261/p264190.png)

Assume that the IT operation team of Company A is monitoring its website logs. If an exception log exists, an alert is triggered and the alert notification is sent by email to the on-duty group of the IT operation team. If an alert incident is not confirmed within 15 minutes, the incident is escalated and the notification is sent by voice call. If an alert incident is not confirmed within 4 hours, the incident is escalated and the notification is sent to the leader of the IT operation team. For more information, see [Create an action policy](/intl.en-US/Alerting/Alerting (New)/Notification management/Create an action policy.md).

**Note:** If an alert incident is in the complete status and the alert management system receives another alert, the time settings for the incident restart. Example: An alert notification is escalated if an incident is not resolved within 4 hours. An alert is triggered at 00:00 and the status of the alert incident is changed to complete at 01:00. Then, another alert is triggered at 02:00. If the incident is not resolved within 4 hours at 06:00, the system escalates the alert notification.

![Escalate alert incidents](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6508072261/p264222.png)

