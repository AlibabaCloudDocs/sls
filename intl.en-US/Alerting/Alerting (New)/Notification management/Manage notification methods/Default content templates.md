# Default content templates

If an alert template that you select is not customized, Log Service uses the default content for the template.

## SMS message

```
Alert name:${alert_name},Status:${status},Severity:${severity},Fire time:${fire_time}
```

## Voice call

```
Alert name:${alert_name},Status:${status},Severity:${severity},Fire time:${fire_time}
```

## Email

```
<table border="1" cellpadding="5" style="width: 100%%; border-collapse: collapse;">
  <thead>
    <tr>
      <td>Project</td>
      <td>AlertName</td>
      <td>FireTime</td>
      <td>AlertTime</td>
      <td>Status</td>
      <td>Severity</td>
      <td>Labels</td>
      <td>Annotations</td>
      <td>Actions</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>${project}</td>
      <td>${alert_name}</td>
      <td>${fire_time}</td>
      <td>${alert_time}</td>
      <td>${status}</td>
      <td>${severity}</td>
      <td>${labels}</td>
      <td>${annotations}</td>
      <td>
        <a href="${dashboard_url}">Dashboard</a>
        <a href="${query_url}">Detail</a>
        <a href="${alert_url}">Mute</a>
      </td>
    </tr>
  </tbody>
</table>
```

## DingTalk

```
- Project: ${project}
- Alert name: ${alert_name}
- Fire time: ${fire_time}
- Alert time: ${alert_time}
- Status: ${status}
- Severity: ${severity}
- Label: ${labels}
- Annotation: ${annotations}

[[Dashboard](${dashboard_url})]
[[Details](${query_url})]
[[Mute for 5 minutes](${alert_url})]
```

## Webhook

```
{
  "aliuid": "${aliuid}",
  "alert_instance_id": "${alert_instance_id}",
  "alert_id": "${alert_id}",
  "alert_name": "${alert_name}",
  "region": "${region}",
  "project": "${project}",
  "alert_time": ${alert_time},
  "fire_time": ${fire_time},
  "resolve_time": ${resolve_time},
  "status": "${status}",
  "results": ${results},
  "fire_results": ${fire_results},
  "fire_results_count": ${fire_results_count},
  "labels": ${labels},
  "annotations": ${annotations},
  "severity": ${severity}
}
```

## Message Center

```
<table border="1" cellpadding="5" style="width: 100%%; border-collapse: collapse;">
  <thead>
    <tr>
      <td>Project</td>
      <td>AlertName</td>
      <td>FireTime</td>
      <td>AlertTime</td>
      <td>Status</td>
      <td>Severity</td>
      <td>Labels</td>
      <td>Annotations</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>${project}</td>
      <td>${alert_name}</td>
      <td>${fire_time}</td>
      <td>${alert_time}</td>
      <td>${status}</td>
      <td>${severity}</td>
      <td>${labels}</td>
      <td>${annotations}</td>
    </tr>
  </tbody>
</table>
```

