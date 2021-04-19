# Usage notes

Log Service integrates Alibaba Cloud Anti-DDoS Origin to provide the mitigation analysis feature. After you enable the mitigation analysis feature, you can query and analyze mitigation logs that record the events of an Anti-DDoS Origin instance. The events include traffic scrubbing, blackhole filtering, and traffic rerouting. The feature can be used to identify website access exceptions and analyze website operations. This topic describes the assets, billing, and limits of the mitigation analysis feature for Anti-DDoS Origin.

## Assets

-   A dedicated project and dedicated Logstore

    By default, log Service is used to create a project by default named ddosbgp-project-Alibaba Cloud account ID-cn-hangzhou and a Logstore named ddosbgp-logstore. This applies after you enable the log analysis feature of Anti-DDoS Origin.

-   Dedicated dashboards

    By default, Log Service generates two dedicated dashboards for the mitigation logs of Anti-DDoS Origin.

    **Note:** We recommend that you do not make changes to the dedicated dashboards. This may affect the usability of the dashboards. You can create a custom dashboard to view log analysis results. For more information, see [Create a dashboard](/intl.en-US/Query and visualization/Dashboard/Create a dashboard.md).

    |Dashboards|Description|
    |----------|-----------|
    |Anti-DDoS Origin Events Report|The report records Anti-DDoS Origin statistics on blackhole filtering and traffic rerouting for protected websites.|
    |Anti-DDoS Origin Mitigation Report|The report records how Anti-DDoS Origin scrubs the attack traffic of the protected websites. The report includes data such as Inbound Traffic Monitor, Distribution of Inbound Traffic \(sort by scrub center\) and Protocol of Inbound Traffic.|


## Billing

-   If you enable the log analysis feature in the Anti-DDoS Origin console, you are billed based on the log retention period and storage space. During the public preview analysis step, the full log analysis and event reports of protected traffic for Anti-DDoS Origin is provided free of charge.
-   Anti-DDoS Origin pushes logs to Log Service, you can query, analyze, monitor, and view log analysis results free of charge. However, you are charged based on the standard pricing of Log Service when you read, transform and ship data, or send alerts by using SMS or voice messages. For more information, see [Log Service pricing](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).

## Limits

-   You can write only Anti-DDoS Origin logs to a dedicated Logstore.
-   You cannot delete a dedicated Logstore.
-   You cannot modify the log retention period for a dedicated Logstore on the Log Service console. However, you can modify the log retention period in the Anti-DDoS Origin console. You can set the value. The value ranges from 30 to 180 days.
-   If the storage space of a dedicated Logstore is full, no more logs can be written to the Logstore.

    **Note:** You can view the usage of log storage space in the Anti-DDoS Origin console. However, the usage is not updated in real time. The displayed usage is delayed by two hours.


