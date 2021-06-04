# Non-customized notification content

When Log Service sends an alert notification \(except webhook notification\), the notification includes customized content and built-in content.

## SMS message

The following script shows the non-customized content of an SMS message:

```
Log Service has a total of ${count} alerts. Alert details: ${details}
```

For example, the customized content of an SMS message is `The ${alert_name} alert in the ${project} project is triggered`. The SMS message that you receive is `Log Service has one alert. Alert details: The alert-test alert is triggered in the project-test project`.

## Voice call

The following script shows the non-customized content of an voice call:

```
Log Service has a total of ${count} alerts. Alert details: ${details}
```

## Email

The following script shows the non-customized content of an email:

```
Dear User:
    Log Service has a new set of alerts (${count} entries). Alert details:
${details}
```

## DingTalk

The following script shows the non-customized content of a DingTalk message:

```
### ${title}

- Number of alerts: ${count}
 -Merge rule: ${groupBy}

#### Alert details:

${details}

---

${atText}
```

## Message Center

The following script shows the non-customized content of a message from Message Center:

```
Dear Mr. Wang: Log Service has ${count} alerts. Alert details:
${details}
```

