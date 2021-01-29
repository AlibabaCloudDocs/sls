# Basic resources

This topic describes the limits on basic resources of Log Service.

The following table describes the limits on basic resources.

|Resource|Limit|Remarks|
|:-------|:----|:------|
|Project|You can create a maximum of 50 projects for an Alibaba Cloud account.|To increase the quota, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm).|
|Logstore|You can create a maximum of 200 Logstores for a project.|To increase the quota, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm).|
|Shard|-   You can create a maximum of 200 shards for a project.
-   When creating a Logstore in the console, you can create a maximum of 10 shards in the Logstore. When creating a Logstore by using the API, you can create a maximum of 100 shards in the Logstore. You can split shards to increase the number of shards no matter how you create a Logstore.

|To increase the quota, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm).|
|Logtail configuration|You can create a maximum of 100 Logtail configurations for a project.|To increase the quota, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm).|
|Log retention time|You can permanently retain logs. You can also customize the log retention time from 1 to 3,000 days.

|N/A|
|Server group|You can create a maximum of 100 server groups for a project.|To increase the quota, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm).|
|Consumer group|You can create a maximum of 20 consumer groups for a Logstore.|You can delete consumer groups that are no longer used.|
|Saved search|You can create a maximum of 100 saved searches for a project.|N/A|
|Dashboard|-   You can create a maximum of 100 dashboards for a project.
-   You can add a maximum of 50 charts to a dashboard.

|N/A|
|Log entry|-   The maximum size of a log entry collected through the API is 1 MB.
-   The maximum size of a log entry collected through a Logtail is 512KB.

|N/A|
|Field name|The maximum size of a field name in a log entry is 128 bytes.|N/A|
|Field value|The maximum size of a field value in a log entry is less than 1 MB.|N/A|
|Log group|The maximum size of a log group is 5 MB.|N/A|
|Alert|You can create a maximum of 100 alerts for a project.|To increase the quota, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm).|

