# Deduplicate alerts based on fingerprints

Alerts with the same fingerprint are considered as the same alert. The alert management system deletes duplicated alerts to prevent alert storms. This topic describes how to deduplicate alerts based on fingerprints.

An alert monitoring rule can trigger multiple alerts. Then, the alert management system calculates a fingerprint for each alert. Alerts with the same fingerprint are considered as the same alert. An alert fingerprint is calculated based on the following attributes:

-   The ID of the Alibaba Cloud account to which an alert monitoring rule belongs
-   The project to which an alert monitoring rule belongs
-   The ID of an alert monitoring rule
-   Alert labels

The following example includes sample configurations of an alert monitoring rule.

```
# Query statement
* | select count(*) as cnt

# Trigger condition
cnt > 0
```

Assume that the system checks the alert data every minute. If the specified condition is met, an alert is triggered every minute. If the triggered alerts have the same fingerprint, the alert management system deletes the duplicated alerts and retains only one alert.

For example, the following three alerts are triggered:

```
// Alert1
{
  "aliuid": "12345",
  "project": "Project1",
  "alert_id": "alert-123",
  "labels": {
    "host": "host-1"
  },
  "annotations": {
    "title": "High CPU utilization",
    "desc": "The current CPU utilization is 90%."
  }
}

// Alert2
{
  "aliuid": "12345",
  "project": "Project1",
  "alert_id": "alert-123",
  "labels": {
    "host": "host-1"
  },
  "annotations": {
    "title": "High CPU utilization",
    "desc": "The current CPU utilization is 95%."
  }
}

// Alert3
{
  "aliuid": "12345",
  "project": "Project1",
  "alert_id": "alert-123",
  "labels": {
    "host": "host-2"
  },
  "annotations": {
    "title": "High CPU utilization",
    "desc": "The current CPU utilization is 90%."
  }
}
```

Alert 1 and Alert 2 are considered as the same alert. Alert 1 and Alert 3 are not considered as the same alert because Alert 1 and Alert 3 have different fingerprints \(labels\).

