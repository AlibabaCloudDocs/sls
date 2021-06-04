# Limits

This topic describes the limits of the alerting feature in Log Service.

|Category|Item|Description|
|--------|----|-----------|
|Alert monitoring|Maximum number of monitoring tasks|A maximum of 100 monitoring tasks can be created for each project. To increase the quota, you can [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm). |
|Search and analytic statements|For more information about the limits on search and analytic statements, see [Search, analytics, and visualization](/intl.en-US/Product Introduction/Limits/Search, analytics, and visualization.md).|
|Query result|If more than 1,000 rows are returned for each query, only the first 1,000 rows are selected for set operations. If you specify three query statements and select **No Merge**, the first 100 rows are returned for each query. |
|Number of query statements|Valid values: 1 to 3.|
|Length of a field value|If the length of a field value exceeds 1,024 characters, only the first 1,024 characters are extracted for analysis.|
|Time range for a query statement|The time range for a query statement cannot exceed 24 hours.|
|Alert management|Regions where incident management is supported|Incident management is supported in the following regions:-   Mainland China: China \(Chengdu\), China \(Heyuan\), China \(Hohhot\), China \(Guangzhou\), China \(Qingdao\), China \(Ulanqab\), China \(Hong Kong\), China \(Hangzhou\), China \(Shenzhen\), China \(Shanghai\), China East 2 Finance, China South 1 Finance
-   Outside mainland China: Singapore \(Singapore\), UAE \(Dubai\), Malaysia \(Kuala Lumpur\), India \(Mumbai\), Japan \(Tokyo\), Germany \(Frankfurt\), UK \(London\), Australia \(Sydney\), US \(Silicon Valley\), and US \(Virginia\) |
|Incident comment|You can add a maximum of 10 comments for each incident. If you add more than 10 comments, old comments are automatically overwritten by new comments, starting from the oldest comment.|
|Notification management|Alert template variable|The maximum size of a variable is 2 KB. If the size exceeds 2 KB, the excess part is truncated.|
|Notification method quota|Each recipient can receive a maximum of 9,999 emails, SMS messages, or phone calls per day. For more information, see [Configure notification quotas]().|

