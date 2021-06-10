# Overview

Log Service allows you to use webhooks to receive the alerts of external monitoring systems, such as Grafana and Prometheus. You can manage alerts in the alert management system in which alerts are denoised and incidents are managed. Then, you can use the notification management system to send alert notifications to specified users.

## Alert processing flow

The following figure shows the processing flow of external alerts, such as Grafana alerts and Prometheus alerts.

![Alert processing flow](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4607133261/p266275.png)

## Severity levels

The alerting system of Log Service supports five levels of alert severity: critical, high, medium, low, and report. You can specify the following keywords of alert severity in an external monitoring system. The keywords are case-insensitive. After external alerts are ingested into Log Service, Log Service automatically maps the specified keywords to the related alert severities in Log Service. If keywords cannot be mapped, the default severity is medium.

|Severity|Keyword|
|--------|-------|
|Critical|critical, disaster, blocker, immediate, fatal, crit, sev0, 'sev 0', or p0|
|High|E, H, high, err, error, urgent, major, 'sev 1', sev1, or p1|
|Medium|M, medium, unknown, warn, warning, 'not classified', average, normal, 'sev 2', sev2, or p2|
|Low|L, I, info, information, suggestion, minor, informational, 'sev 3', sev3, or p3|
|Report|report, dbg, debug, verbose, trivial, page, ok, 'sev 4', sev4, or p4|

