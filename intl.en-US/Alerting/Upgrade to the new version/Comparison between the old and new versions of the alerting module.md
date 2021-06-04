# Comparison between the old and new versions of the alerting module

The alerting module is upgraded to support alert monitoring, alert management, and notification management. This topic compares the old and new versions of the alerting module based on the architecture, features, and configurations.

## Upgraded architecture

If alerts are triggered based on an alert monitoring rule, these alerts are denoised based on a specified alert policy. Then, the alerts are dispatched by using the notification methods that are specified in the action policy. The new alerting module can also be used to manage incident statuses and escalate alerts.

-   Old workflow

    ![Old architecture](../images/p254337.png)

-   New workflow

    ![New architecture](../images/p244844.png)


## Optimized features

The upgraded features include optimized and added features.

-   Optimized features

    |Feature|Old version|New version|
    |-------|-----------|-----------|
    |Log monitoring|If data is returned for a query, an alert is triggered.|You can specify whether to trigger an alert if data is returned for a query.|
    |If a specified condition is met, an alert is triggered.|You can specify whether to trigger an alert if the number of returned rows reaches a specified value.|
    |Time series data monitoring|If data is returned for a query, an alert is triggered. The syntax of search and analytic statements is complex.|You can specify whether to trigger an alert if data is returned for a query. You can also specify whether to trigger an alert if the number of returned rows reaches a specified value.|
    |If data is returned for a query, an alert is triggered.|You can specify whether to trigger an alert if data is returned for a query.|
    |If a specified condition is met, an alert is triggered.|You can specify whether to trigger an alert if the number of returned rows reaches a specified value.|
    |Union queries are not supported.|Union queries are supported.|
    |Report association|When you create an alert monitoring rule, you must associate the rule with at least one chart.|When you create an alert monitoring rule, you do not need to associate the rule with a chart.|
    |Associated monitoring of Logstores or Metricstores|When you perform a union query, you can use only the CROSS JOIN and No Merge operations.|When you perform a union query, you can use various set operations. The set operations include CROSS JOIN, No Merge, JOIN, LEFT JOIN, RIGHT JOIN, FULL JOIN, LEFT EXCLUDE JOIN, and RIGHT EXCLUDE JOIN.|
    |Alert deduplication|In a time window, duplicate alerts that are triggered based on the same alert monitoring rule are removed.|Duplicate alerts can be removed based on specified labels. You can also specify the frequency at which alert notifications are sent.|

-   Added features

    The following table describes the new features in terms of alert monitoring, alert management, notification management, and alert analysis.

    |Category|Feature|Description|
    |--------|-------|-----------|
    |Alert monitoring|Associated monitoring for Logstores and Metricstores|You can use SQL JOIN clauses or set operations to perform associated monitoring based on query results.|
    |Blacklist and whitelist monitoring|You can use resource data to associate whitelist or blacklist objects.|
    |Associated monitoring for data|You can use set operations on data across projects, regions, and Alibaba Cloud accounts.|
    |Alert severity|You can configure static or dynamic settings for alert severities. You can also specify the severity for a no-data alert.|
    |Label and annotation|You can customize labels and annotations. You can set a label value to a variable.|
    |Multi-group monitoring|You can group query results that are obtained based on an alert monitoring rule. Each group is evaluated. Alert notifications are sent by group.|
    |No-data alert|If no data is returned for a query, an alert is triggered and an alert notification is sent. The incident status can be automatically switched and displayed. You can specify notification methods.|
    |Recovery notification|If an alert is cleared, a recovery notification is sent. The incident status can be automatically switched and displayed. You can specify notification methods.|
    |Alert management|Alert denoising|You can manage global alerts. You can configure silence policies and suppression policies for alerts. You can also group and merge alerts.|
    |Incident management|You can switch incident statuses. An incident can be in the Confirmed, Resolved, or Ignored state. You can also specify incident handlers or configure auto dispatch.|
    |Notification management|Dynamic dispatch and alert escalation|    -   You can configure dynamic dispatch based on alerts. Then, alert notifications can be dynamically dispatched to the specified users, user groups, or on-duty groups of a specified notification method.
    -   If an alert remains unresolved or unconfirmed for a long period of time, the alert is escalated. Then, a notification is sent to the specified recipients by using the specified notification methods. |
    |Recipient management|You can specify users, user groups, or on-duty groups as recipients.|
    |Calendar|Non-business days, business days, and holidays in China and the United States can be automatically identified. Notification methods can be dynamically adjusted.|
    |Shift plan|You can schedule rotating shifts and substitute shifts based on your business requirements. You can configure a custom calendar for an on-duty group. You can customize holidays. Custom holidays can be automatically identified.|
    |Notification method quota|You can specify the quotas of SMS messages, voice calls, and emails. You can specify these quotas for specified users or user groups.|
    |Alert analysis|Monitoring Rule Center, Alert Link Center, and Troubleshooting Center dashboards|The Monitoring Rule Center dashboard displays the running statuses of alert monitoring rules and the statuses of alerts. The Alert Link Center dashboard displays the entire pipeline of alerts that are triggered based on the related alert monitoring rules. The Troubleshooting Center dashboard displays the statistics of errors that occur in the alert monitoring system, alert management system, and notification management system. You can filter and view alert statuses by region, project, and alert severity.|
    |Global storage|The global storage of alert data allows you to view related incidents or logs in an efficient manner.|


## Parameter changes

The parameters that are required when you configure alerts, notification methods, and alert template variables are changed.

-   Alert monitoring

    After the alerting module is upgraded, the following parameters are added. Other parameters remain the same.

    |Added parameter|Default value|
    |---------------|-------------|
    |Group Evaluation|No Grouping|
    |Set Operations|CROSS JOIN|
    |Trigger Condition|Data is returned|
    |Severity|Medium|
    |No Data Alert|Off|
    |Recovery Notifications|Off|

-   Notification management

    After the alerting module is upgraded, a phone number or email address is extracted as a user identifier and a user is created. The extracted content of a notification is used as the content of an alert template. An action policy is generated based on the specified notification methods. By default, the sls.builtin.dynamic policy is used.

    **Note:**

    -   The same phone number or email address of a notification method automatically matches the related user that is upgraded. Alert notifications are sent to the user.
    -   The same notification content of a notification method automatically matches the related alert template that is upgraded. Alert notifications are sent by using the alert template.
    -   The same notification method automatically matches the related action policy that is upgraded. Alert notifications are sent based on the action policy.
    |Notification method|New version|Old version|
    |-------------------|-----------|-----------|
    |SMS message|Username + Phone number + Alert template|Phone number + Content|
    |Voice call|Username + Phone number + Alert template|Phone number + Content|
    |Email|Username + Email address + Alert template|Email address + Content|
    |DingTalk|Username + Phone number + Alert template|Request URL + @Phone number in DingTalk + Content|
    |None|Alert template|Content of an SMS message|
    |Content of a voice call|
    |Content of an email|
    |Content of a custom webhook|
    |Content of a DingTalk webhook|
    |Content of a message from Message Center|

-   Alert template variables

    The alert template variables of the new alerting module are adjusted to be consistent with the variables that are used in alert policies. Multiple variables are added. The following table compares the variable names of the old and new versions.

    |Variable of the old version|Variable of the new version|Description|
    |---------------------------|---------------------------|-----------|
    |Aliuid|aliuid|The ID of the Alibaba Cloud account to which a project belongs.|
    |Project|project|The project to which an alert monitoring rule belongs.|
    |AlertID|alert\_instance\_id|The ID of an alert.|
    |AlertDisplayName|alert\_name|The display name of an alert monitoring rule.|
    |Condition|condition|The trigger condition of an alert monitoring rule. The variables in the trigger condition are replaced by the values that trigger the alert. Each value is enclosed in a pair of brackets \[\].|
    |RawCondition|raw\_condition|The original trigger condition of an alert monitoring rule. The variables in the trigger condition are not replaced by the actual values.|
    |Dashboard|dashboard|The name of the dashboard with which an alert monitoring rule is associated.|
    |DashboardUrl|dashboard\_url|The URL of the dashboard with which an alert monitoring rule is associated.|
    |FireTime|fire\_time|The time when an alert is triggered.|
    |FullResultUrl|query\_url|The URL that is used to query the records that an alert is triggered.|
    |Results|results|The parameters and results of a query. The value is of the array type. For information about the fields in the results variable, see [Structure of the results variable](#section_n3h_eiq_56w). **Note:** A maximum of 100 alert notifications can be sent. |

    For more information, see [System variables](System variablest1893310.dita#concept_2505857) and [Alert template variables](Alert template variablest2061590.dita#reference_2061590).


## Structure of the results variable

|Field of the old version|Field of the new version|Description|
|------------------------|------------------------|-----------|
|Query|query|A query statement.|
|LogStore|store|The destination Logstore of a query.|
|StartTime|start\_time|The time when a query starts.|
|StartTimeTs|start\_time\_ts|The time when a query starts. The time is in the UNIX timestamp format.|
|EndTime|end\_time|The time when a query ends.|
|EndTimeTs|end\_time\_ts|The time when a query ends. The time is in the UNIX timestamp format.|
|RawResults|raw\_results|The query result that is formatted in an array. Each element in the array is a log entry. An array can contain a maximum of 100 elements.|
|RawResultsAsKv|raw\_results\_as\_kv|The query result that is formatted in key-value pairs. **Note:** This field can be used as a template variable. However, no data is stored to a Logstore for this field. |
|RawResultCount|raw\_result\_count|The number of raw log entries that are returned.|
|FireResult|fire\_result|The log entry that records the triggers of an alert. If no alert is triggered, the parameter value is null.|
|FireResultAsKv|fire\_result\_as\_kv|The log entry that records the triggers of an alert. The log entry is formatted in key-value pairs. **Note:** This field can be used as a template variable. However, no data is stored to a Logstore for this field. |

