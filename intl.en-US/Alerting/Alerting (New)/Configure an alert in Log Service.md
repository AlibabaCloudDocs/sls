# Configure an alert in Log Service

Log Service provides the alerting feature. You can configure an alert monitoring rule for query results. If the conditions of the alert monitoring rule are met, an alert is triggered and an alert notification is sent. This topic describes how to configure an alert in Log Service.

The Data Lab application of Log Service provides simulated website access logs and related dashboards. The dashboards include Website Audit Center and Website Access Center. You can use Data Lab to query and analyze data. You can also configure alerts for data. In this example, the **request success ratio** and **response\_time trend** charts on the **Website Audit Center** dashboard are monitored. If the request success rate is lower than 90% and the response time is greater than 60 seconds, an alert is triggered. An alert notification is sent to a user group named LogServiceOperations by using an SMS message.

## Step 1: Create users and a user group

Users and user groups are used as the recipients for alert notifications. For example, two users named Alice and Kumar are created, and a user group named LogServiceOperations is created. The Alice and Kumar users are added to the LogServiceOperations user group.

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the destination project.

3.  In the left-side navigation pane, click the ![Alerts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0356352261/p110115.png) icon.

4.  Click **Open Alert Center**.

5.  Create users.

    1.  Click the **Alert Management** drop-down list and select **User Management**.

        ![User Management](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1709872261/p254178.png)

    2.  Click **Add Users**.

    3.  On the Add Users tab, enter the information of the users that you want to add, and click **OK**.

        The following table describes the required parameters and provides examples.

        ```
        # ID, Username, Enabled, Country code-phone number, Receive text message, Receive phone call
        1001,Kumar,true,86-1381111*****,true,true
        1002,Alice,true,86-1381111*****,true,true
        ```

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |**ID**|The unique identifier of the user. The following rules must be met:        -   The ID must start with a letter.
        -   The ID must be 5 to 60 characters in length.
        -   The ID can contain digits, letters, underscores \(\_\), hyphens \(-\), and periods \(.\).
|1001 and 1002|
        |**Username**|The name of the user. The username must be 1 to 20 characters in length, and cannot contain the following characters:

"\\$\|~?&<\>\{\}\`'

|Kumar and Alice|
        |**Enabled**|Specifies whether Log Service can send alert notifications to the user.         -   true: Log Service can send alert notifications to the user.
        -   false: Log Service cannot send alert notifications to the user.
|true|
        |**Country code-phone number**|The phone number of the user. The country code can contain only digits and must be 1 to 4 characters in length.|86-1381111\*\*\*\*\* and 86-1381112\*\*\*\*\*|
        |**Receive text message**|Specifies whether Log Service can send SMS messages to the phone number.         -   true: Log Service can send SMS messages to the phone number.
        -   false: Log Service cannot send SMS messages to the phone number.
|true|
        |**Receive phone call**|Specifies whether Log Service can send voice notifications to the phone number.         -   true: Log Service can send voice notifications to the phone number.
        -   false: Log Service cannot send voice notifications to the phone number.
|true|

6.  Create a user group.

    1.  Click the **Alert Management** drop-down list and select **User Group Management**.

    2.  Click **Create**.

    3.  In the Add User Group dialog box, set the required parameters, and click **OK**.

        The following table describes the required parameters and provides examples.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |**ID**|The unique identifier of the user group. The following rules must be met:        -   The ID must start with a letter.
        -   The ID must be 5 to 60 characters in length.
        -   The ID can contain digits, letters, underscores \(\_\), hyphens \(-\), and periods \(.\).
|group-01|
        |**Group Name**|The name of the user group. The group name must be 1 to 20 characters in length, and cannot contain the following characters:

\\$\|~?&<\>\{\}\`'"

|LogServiceOperations|
        |**Available Members**|The users that you have created.|Kumar and Alice|
        |**Selected Members**|The users that are added to the user group.|Kumar and Alice|
        |**Enabled**|Specifies whether Log Service can send alert notifications to the user group.         -   If you turn on the Enabled switch, Log Service can send alert notifications to the user group.
        -   If you turn off the Enabled switch, Log Service cannot send alert notifications to the user group.
|Turn on the Enabled switch.|


## Step 2: Create an action policy

Action policies are used to manage alert notification methods and the frequency at which alert notifications are sent. For example, an action policy is created for website logs. The alert notifications that are generated by the alerts of an Alibaba Cloud account whose ID is 121\*\*\*\*6408 are sent to the LogServiceOperations user group by using SMS messages.

1.  Click the **Alert Management** drop-down list and select **Action Policy**.

2.  On the Action Policy tab, click **Create**.

3.  In the **Add Action Policy** dialog box, set the **ID** and **Name** parameters.

    |Parameter|Description|Example|
    |---------|-----------|-------|
    |**ID**|The unique identifier of the action policy.|web-01|
    |**Name**|The name of the action policy.|Website Logs\_Action Policy|

4.  Add a primary action policy.

    1.  On the Primary Action Policy tab, click the ![action](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2709872261/p253237.png) icon to create an action group.

    2.  Configure the action group.

        In this example, SMS Message is selected as the notification method. If an alert is triggered, Log Service sends an alert notification to the specified phone numbers by using an SMS message. The following figure shows the required parameters.

        **Note:** If you select SMS Message as the notification method, alert notifications are sent by using random phone numbers. The displayed phone numbers are randomly generated.

        ![action_policy](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2709872261/p253238.png)

        The following table describes the required parameters and provides examples.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |**Notification Method**|The method that is used to send alert notifications.|SMS Message|
        |**Recipient**|Select one or more users or user groups that you have created.|LogServiceOperations|
        |**Alert Template**|Select an alert template.|SLS builtin content template|
        |**Period**|Select a period during which alert notifications are sent.|Any Time|

    3.  Click the ![End ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0356352261/p243468.png) icon of the Action Group dialog box to complete the configuration of the primary action policy.

5.  Click **OK**.


## Step 3: Create an alert monitoring rule for logs

Alert monitoring rules are used to monitor the query results of logs. For example, you can create an alert monitoring rule to monitor the **request success ratio** and **response\_time trend** charts. If the request success rate is lower than 90% and the response time is greater than 60 seconds, an alert is triggered.

1.  On the **Log Storage** \> **Logstores** tab, click the Logstore where logs are stored.

2.  In the upper-right corner of the page, click **Save as Alert**.

3.  In the Alert Rule panel, set the required parameters of the alert monitoring rule, and click **OK**.

    The following table describes the required parameters and provides examples.

    ![Create an alert monitoring rule](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2709872261/p253647.png)

    |Parameter|Description|Example|
    |---------|-----------|-------|
    |**Rule Name**|Specify the name of the alert monitoring rule.|Website Logs\_Alert Monitoring Rule|
    |**Check Frequency**|Specify a check frequency at which query results are checked.     -   **Hourly**: Query results are checked every hour.
    -   **Daily**: Query results are checked at a specified time every day.
    -   **Weekly**: Query results are checked at a specified time on a specified day of every week.
    -   **Fixed Interval**: Query results are checked at a specified interval.
    -   **Cron**: Query results are checked at an interval that is specified by using a CRON expression.

If you use CRON expressions, the minimum precision is 1 minute. The time format is based on the 24-hour clock. For example, `0 0/1 * * *` indicates that a check is performed every hour from 00:00.

|Daily, 00:00|
    |**Query Statistics**|Specify a query statement. You can click the search bar to edit the query statement. If the request success rate is lower than 90% and the response time is greater than 60 seconds, an alert is triggered. In this case, you must set the **Set Operations** parameter to **CORSS JOIN**.

|    -   0: Select the **request success ratio** chart on the **Website Audit Center** dashboard.
    -   1: Select the **response\_time trend** chart on the **Website Audit Center** dashboard. |
    |**Trigger Condition**|Specify the trigger condition of an alert.     -   **Data is returned**: If data is returned for a query, the alert is triggered.
    -   **the query result contains**: If the number of returned rows of a query reaches N, the alert is triggered.
    -   **data matches the expression**: If the returned data of a query matches a specified expression, the alert is triggered.
    -   **the query result contains**: If the number of returned rows of a query reaches N, and the N rows of data matches a specified expression, the alert is triggered.
For more information, see [Syntax of trigger conditions in alert rules](/intl.en-US/Alerting/Alerting (Old)/Relevant syntax and fields for reference/Syntax of trigger conditions in alert rules.md).

|Data is returned, `$0.success_ratio <90 &&$1.avg_upstream_response_time\(s\) >60`**Note:** If a field contains parentheses \(\), you must use backslashes \(\\\) to escape the parentheses \(\). |
    |**Severity**|Specify the alert level. This parameter is used to denoise alerts and manage alert notifications. When you create an alert policy or action policy, you can add conditions based on severities.     -   Simple mode: If you only select a severity, all alerts that are triggered based on the alert monitoring rule have the same severity.
    -   Conditional mode: You can click Create to specify a condition and the related severity. For information about conditional expressions, see [Syntax of trigger conditions in alert rules](/intl.en-US/Alerting/Alerting (Old)/Relevant syntax and fields for reference/Syntax of trigger conditions in alert rules.md).
|Medium-6|
    |**Add Annotation**|Log Service allows you to add non-identifying attributes for alerts. Annotations are formatted in key-value pairs. This parameter is used to denoise alerts and manage alert notifications. When you create an alert policy or action policy, you can add conditions based on annotations. If you want to configure dynamic settings, you can use the field variables in specified query statements.

|    -   Title\(title\): `Monitor the request success rate and average response time of a website`
    -   Description\(desc\): `Request success rate: ${success_ratio},Average response time: ${avg_upstream_response_time(s)}` |
    |**Threshold of Continuous Triggers**|Specify the threshold of continuous triggers. An alert is triggered only when the specified trigger condition is met during continuous check periods. If the trigger condition is not met, no alert is triggered. **Note:** If you use the group evaluation feature, query results are grouped based on specified fields. The number of continuous triggers is calculated for each group, and alerts are separately triggered. For example, query results are grouped into Group1 and Group2. If the number of continuous triggers in Group1 reaches the specified threshold, an alert is triggered. If the number of continuous triggers in Group2 reaches the specified threshold, an alert is also triggered.

|1|
    |**Action Policy**|Select the action policy that you have created to manage notification methods and the frequency at which alert notifications are sent.|Website Logs\_Action Policy|
    |**Cycle**|If duplicate alerts are triggered in the specified duration, the selected action policy is executed only once, and only one alert notification is sent.|5 Minutes|


## Step 4: View the alert records

After you create an alert monitoring rule, Log Service monitors query results based on the rule. If the specified trigger condition is met, an alert is triggered and the associated action policy is executed.

1.  In the left-side navigation pane, click the ![Alerts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0356352261/p110115.png) icon.

2.  Click **Open Alert Center**.

3.  Click the **Alert Management** drop-down list and select **Monitoring Rule Center**.

4.  In the Alert rule latest status section, view the alert monitoring rules that are executed.

    ![View alert monitoring rules](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2709872261/p253652.png)


