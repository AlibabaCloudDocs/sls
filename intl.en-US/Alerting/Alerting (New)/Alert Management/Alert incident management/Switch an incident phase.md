# Switch an incident phase

An alert incident can be in different phases and some operations may switch the phase of an incident. For example, if an incident is in the start phase, after you confirm the incident, the phase switches to processing.

## Phase

An incident has the following phases:

|Phase|Description|
|-----|-----------|
|Start|The incident is in the pending evaluation state. This indicates that the incident is waiting to be confirmed, resolved, or ignored.|
|Processing|The incident is confirmed.|
|Complete|The incident is resolved or ignored.|

## Operation

The following operations may switch the phase of an incident.

|Operation|Description|
|---------|-----------|
|Trigger an alert|The specified conditions are met and an alert is triggered.|
|Send recovery notifications|The conditions are not met during the next check and a recovery notification is sent.|
|Perform operations in the console|Confirm, resolve, or ignore an incident in the console.|

## Switch an incident phase

The following figure shows the process of changing an incident phase.

![Switch an incident phase](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3839872261/p264451.png)

