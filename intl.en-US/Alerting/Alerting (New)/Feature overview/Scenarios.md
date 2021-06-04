# Scenarios

This topic describes the scenarios in which you can use the alerting feature of Log Service.

## Development operation and maintenance

Developers can monitor each phase of service lifecycle by using the alerting feature of Log Service. This way, developers can identify errors that may occur when code is modified or published and fix the errors at the earliest opportunity. The alerting feature also allows you to monitor logs such as Kubernetes output logs, event logs, and application logs that are imported to Log Service from different environments. The environments include the development environment, staging environment, and production environment. If a version error or metric exception such as network latency or jitter is detected in the environment, notifications are sent to developers in a timely manner.

The Log Audit Service and SLB Log Center applications of Log Service provide built-in alert templates. This simplifies the process to create alerts tasks for developers.

## IT operation

IT operation teams need to ensure the reliability and stability of the entire IT infrastructure. If an enterprise or organization imports its log or metric data to Log Service, the IT operation team can monitor whether the data is stable. For example, the team can monitor metrics, such as request response time, load threshold, and error rate in real time. This allows the operation team to identify and resolve the errors at the earliest opportunity. The alerting feature provides a mechanism that can notify the responsible person 24/7. The alerting feature allows you to merge alerts, configure denoising policies, and configure dynamic alert policies. The system identifies the responsible person based on on-duty schedules and calendars and automatically performs follow-up operations. For example, the system can send recovery notifications, update alert statuses, and escalate alert severities.

## Intelligent operation and maintenance

Developers and IT operation teams can use the machine learning feature and alerting feature of Log Service to monitor log data and time series data. For example, they can use these features to cluster data, predict data trends, and detect data anomalies. The query and analysis feature of Log Service provides more than 10 machine learning functions that can be used when you create alert rules. You can use the functions to smooth, detect, and decompose single-time series data. You can also use the functions to cluster multi-time series data and mine the pattern of multi-attribute fields. For more information, see [Machine learning overview](/intl.en-US/Index and query/Machine learning syntax and functions/Overview.md).

![Intelligent operation and maintenance](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6829872261/p261935.png)

## Security operation and maintenance

After an enterprise or organization imports its audit and security-related data and events to Log Service, security teams can continuously monitor the data. This way, the systems can identify external and internal incompliant operation and security threats. If the system detects exceptions, the system automatically sends notifications based on the severity and source monitoring rules and automatically performs follow-up operations. For example, the system can create a dashboard to visualize the data.

The Log Audit Service application allows you to collect logs and events of Alibaba Could services from multiple Alibaba Cloud accounts. The Log Audit Service application is integrated with the threat intelligence feature and alerting feature and provides about 100 alert templates that are related to operation compliance and security. This improves the work efficiency of security teams. For more information, see [Log Audit Service](/intl.en-US/Application/Log Audit Service/Background information.md).

## Business operation

After an enterprise or organization imports data or metrics to Log Service, the business operation team can continuously monitor the data or metrics. The business operation team includes the marketing team, customer operation team, and finance team. For example, the team can monitor the metrics of user numbers, user popularity, PVs, and bills of Alibaba Cloud services to check if any exception occurs. This way, the team can respond to metric changes or exceptions, such as abnormal expenses in a timely manner. This improves operation efficiency and reduces business and finance risks.

Cost Manager allows you to import bills for Alibaba Cloud services and provides dozens of alert templates to monitor budgets and expenses. This simplifies the operation of the finance team. For more information, see [Cost Manager](/intl.en-US/Application/Cost Manager/Cost Manager.md).

