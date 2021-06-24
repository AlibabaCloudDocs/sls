# Basic resources

This topic describes the limits on the basic resources of Log Service.

|Item|Limit|Adjustable|
|:---|:----|:---------|
|Project|You can create a maximum of 50 projects for an Alibaba Cloud account.|To increase the quota, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm).|
|Logstore|You can create a maximum of 200 Logstores for a project.|To increase the quota, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm).|
|Shard|-   You can create a maximum of 400 shards for a project.
-   When you create a Logstore by using the console, you can create a maximum of 10 shards in the Logstore. When you create a Logstore by calling the API, you can create a maximum of 100 shards in the Logstore. You can split shards to increase the number of shards by using one of the preceding two methods.

|To increase the quota, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm).|
|Logtail configuration|You can create a maximum of 200 Logtail configurations for a project.|To increase the quota, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm).|
|Log retention period|You can permanently retain logs. You can also specify a log retention period. Unit: days. Valid values: 1 to 3000.

|N/A|
|Machine group|You can create a maximum of 200 machine groups for a project.|To increase the quota, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm).|
|Consumer group|You can create a maximum of 20 consumer groups for a Logstore.|You can delete consumer groups that are no longer used.|
|Saved search|You can create a maximum of 100 saved searches for a project.|N/A|
|Dashboard|-   You can create a maximum of 100 dashboards for a project.
-   You can add a maximum of 100 charts to a dashboard.

|N/A|
|LogItem|-   The maximum size of a log entry that is collected by calling the API is 1 MB.
-   The maximum size of a log entry that is collected by using Logtail is 512 KB.

|N/A|
|Field name \(key\)|The maximum size of a field name \(key\) is 128 bytes.|N/A|
|Field value \(value\)|The maximum size of a field value \(value\) is 1 MB.|N/A|
|Log group|The maximum size of a log group is 5 MB.|N/A|
|Alert|You can create a maximum of 100 alerts for a project.|To increase the quota, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm).|

