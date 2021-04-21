# Configure events

Log Service allows you to configure drill-down events for raw logs to visualize logs and obtain more log details. You can configure default events and advanced events. This topic describes how to configure events for raw logs in the Log Service console.

-   The indexing feature is enabled and configured. For more information, see [Configure indexes](/intl.en-US/Index and query/Configure indexes.md).
-   A Logstore is created if you configure an advanced event to open a Logstore. For more information, see [Create a Logstore](/intl.en-US/Data Collection/Preparation/Manage a Logstore.md).
-   A saved search is created if you configure an advanced event to open a saved search. For more information, see [Saved search](/intl.en-US/Index and query/Query/Saved search.md).

    Placeholder variables are configured in the destination saved search if you configure variables. For more information, see [Set a placeholder variable](/intl.en-US/Query and visualization/Dashboard/Add an analysis chart to a dashboard.md).

-   A dashboard is created if you configure an advanced event to open a dashboard. For more information, see [Create a dashboard](/intl.en-US/Query and visualization/Dashboard/Create a dashboard.md).

    Placeholder variables are configured in the destination dashboard if you configure variables. For more information, see [Set a placeholder variable](/intl.en-US/Query and visualization/Dashboard/Add an analysis chart to a dashboard.md).

-   An HTTP link is created if you configure an advanced event to open a custom HTTP link.

Drilling is an essential feature in data analysis. This feature allows you to view more details by moving to different layers of data. Drilling includes rolling up and drilling down. Drilling down allows you to move to deeper data layers to gain an insight into data. This way, you can extract more value from data and make informative decisions. Log Service allows you to configure default events and advanced events to analyze raw logs.

## Configure default events

When you configure default events, you can add conditions to query statements by using the AND and OR operators or create new query statements.

On the Table or Raw Data tab, click a field value. The **Default** dialog box appears. The following figure shows the operations that you can perform.

![query](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4381009161/p266514.png)

For example, the query statement you entered in the search box is `* | SELECT status as dim, count(1) as c group by dim`. If you click the value 203.0.113.1 in the host field, the query statement in the search box varies based on the event action you select.

|Event action|Description|Result|
|------------|-----------|------|
|**Add to Query**|Append the keyword that you click to the query statement by using the AND operator and query the data.|`* and host: "203.0.113.1" | SELECT status as dim, count(1) as c group by dim`|
|**Exclude from Query**|Append the keyword that you click to the query statement by using the NOT operator.|`* not host: "203.0.113.1" | SELECT status as dim, count(1) as c group by dim`|
|**Add Search**|Delete the query statement from the search box and create a query statement by using the specified keyword.|`* and host: "203.0.113.1"`|

## Configure advanced events

You can configure advanced events for log fields to analyze logs at a deeper level. You can configure an advanced event to open a Logstore, saved search, dashboard, or a custom HTTP link.

On the Table or Raw Data tab, click ![icon](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4381009161/p266517.png) and then click **Event Settings** to go to the Advanced Event Settings window.

![event](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4381009161/p266516.png)

**Note:** You can configure up to 10 advanced events for each log field.

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the project in which you want to query and analyze logs.

3.  On the **Log Storage** \> **Logstores** tab, click the Logstore where logs are stored.

4.  On the **Raw Logs** tab, click Table or Raw Data. Then, click ![icon](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5381009161/p266518.png) and **Event Settings** to go to the Advanced Event Settings window.

5.  In the Advanced Event Settings window, click the field for which you want to add the advanced events, and click **Add Event**.

6.  In the **Event Settings** section, set the required parameters.

    You can configure an advanced event to open a Logstore, saved search, dashboard, or a custom HTTP link. The following table describe the parameters.

    -   **Open Logstore**

        Set the event action to open a Logstore. The following table describe the parameters.

        |Parameter|Description|
        |:--------|:----------|
        |**Configuration Name**|The name of the advanced event.|
        |**Event Action**|Select **Open Logstore**.|
        |**Open in New Tab**|If you turn on this switch, the specified Logstore is opened in a new tab when the advanced event is triggered.|
        |**Time Range**|The time range that is used to query the data in the specified Logstore. Valid values:         -   **Default**: The time range that is used to query the data in the Logstore to which you are redirected. The default time range is 15 Minutes \(Relative\).
        -   **Use Query Time**: The time range that is used to query the data in the Logstore to which you are redirected. This time range is the same as the time range specified to query raw logs.
        -   **Relative**: The time range that is used to query the data in the Logstore to which you are redirected. This time range is accurate to the second.
        -   **Time Frame**: The time range that is used to query the data in the Logstore to which you are redirected. This time range is accurate to the minute, hour, or day. |
        |**Select Logstore**|The name of the Logstore to which you want to be redirected. When an advanced event is triggered, you are redirected to the Logstore page.|
        |**Inherit Filtering Conditions**|If you turn on the **Inherit Filtering Conditions** switch, the filtering conditions of the current query are synchronized to the destination Logstore by using the `AND` operator.|
        |**Filter**|If you enter a filter statement on the **Filter** tab, the filter statement is synchronized to the destination Logstore by using the `AND` operator. The filter statement can contain fields that you specify in the **Optional Parameter Fields** field. For example, if you click `${__topic__}`, the variable is appended to the query statement of the destination Logstore by using the `AND` operator. |
        |**Variable**|Not supported.|

    -   Open Saved Search

        Set the event action to open a saved search. The following table describes the parameters.

        |Parameter|Description|
        |:--------|:----------|
        |**Configuration Name**|The name of the advanced event.|
        |**Event Action**|Select **Open Saved Search**.|
        |**Open in New Tab**|If you turn on this switch, the specified saved search is opened in a new tab when the advanced event is triggered.|
        |**Time Range**|The time range of the data that the saved search queries. Valid values:         -   **Default**: The time range of the data that the saved search queries is the default time range of a Logstore. The default time range is 15 Minutes \(Relative\).
        -   **Use Query Time**: The time range of the data that the saved search queries is the same as the time range specified when you query raw logs.
        -   **Relative**: The time range of the data that the saved search queries is accurate to the second.
        -   **Time Frame**: The time range of the data that the saved search queries is accurate to the minute, hour, or day. |
        |**Select Saved Search**|The name of the saved search to which you want to be redirected.|
        |**Inherit Filtering Conditions**|If you turn on the **Inherit Filtering Conditions** switch, the filtering conditions of the current query are synchronized to the saved search to which you are redirected. The filtering conditions are appended to the saved search by using the `AND` operator.|
        |**Filter**|If you enter a filter statement on the **Filter** tab, the filter statement is appended to the destination saved search by using the `AND` operator. The filter statement can contain fields that you specify in the **Optional Parameter Fields** field. For example, if you click `${__topic__}`, the variable is appended to the query statement of the destination saved search by using the `AND` operator. |
        |**Variable**|Log Service allows you to modify a saved search by using variables. If you configure a variable that is the same as a variable in the saved search, the configured variable replaces the variable in the saved search. Click **Variable** to go to the Variable tab and configure variables. **Note:**

        -   If you configure variables for the event, you must first configure placeholder variables for the saved search to which you want to be redirected. For more information, see [Set a placeholder variable](/intl.en-US/Query and visualization/Dashboard/Add an analysis chart to a dashboard.md).
        -   You can add a maximum of five dynamic variables and five static variables.
        -   Dynamic variables: The field value that you click to trigger the event is used as the variable for the query.
            -   **Variable**: The name of the dynamic variable. For example, the placeholder variable that you specify in the saved search is `dynamic_ip`.
            -   **Variable Value Column**: The column where the variable value is located. For example, if you select `__source__`,

the value of the `__source__` field replaces the placeholder variable in the destination saved search.

        -   Static variables: The static variable that you specify is used for the query.
            -   **Variable**: The name of the static variable. For example, the placeholder variable that you specify in the saved search is `static_ip`.
            -   **Static Value**: The value of the static variable that is used to replace the placeholder variable in the destination saved search. For example, `203.0.113.1`

indicates that the value `203.0.113.1` of the `static_ip` field replaces the placeholder variable in the destination saved search. Logs whose placeholder variable value is `203.0.113.1` are queried. |

    -   Open Dashboard

        Set the event action to open a dashboard. The following table describes the parameters.

        |Parameter|Description|
        |:--------|:----------|
        |**Configuration Name**|The name of the advanced event.|
        |**Event Action**|Select **Open Dashboard**.|
        |**Open in New Tab**|If you turn on this switch, the specified dashboard is opened in a new tab when the advanced event is triggered.|
        |**Time Range**|The time range that is used to query for the dashboard to which you are redirected. Valid values:         -   **Default**: The time range that is used to query for the dashboard to which you are redirected is the default time range. The default time range is 15 Minutes \(Relative\).
        -   **Use Query Time**: The time range of the chart on the destination dashboard is the time range of the chart that is specified on the source dashboard when the event is triggered.
        -   **Relative**: The time range that is used to query for the dashboard to which you are redirected. This time range is accurate to the second.
        -   **Time Frame**: The time range that is used to query for the dashboard to which you are redirected. This time range is accurate to the minute, hour, or day. |
        |**Select Dashboard**|The name of the dashboard to which you want to be redirected.|
        |**Inherit Filtering Conditions**|If you turn on the **Inherit Filtering Conditions** switch, the filtering conditions of the source dashboard are synchronized to the destination dashboard when the event is triggered.|
        |**Filter**|If you enter a filter statement on the **Filter** tab, the filter statement is synchronized to the destination dashboard. The filter statement can contain fields that you specify in the **Optional Parameter Fields** field. For example, if you click `${__source__}`, only the logs that contain the `${__source__}` fields are displayed in the destination dashboard. |
        |**Variable**|The variables that you configure are synchronized to the destination dashboard when the event is triggered. Click **Variable** to go to the Variable tab and configure variables. **Note:**

        -   If you configure variables for the event, you must first configure placeholder variables for the chart of the destination dashboard to which you want to be redirected. For more information, see [Set a placeholder variable](/intl.en-US/Query and visualization/Dashboard/Add an analysis chart to a dashboard.md).
        -   You can add a maximum of five dynamic variables and five static variables.
        -   Dynamic variables: The field value that you click to trigger the event is used as the variable to query.
            -   **Variable**: The name of the dynamic variable. For example, the placeholder variable that you specify in the destination dashboard is `dynamic_ip`.
            -   **Variable Value Column**: The column where the variable value is located. For example, if you select `__source__`,

the value of the `__source__` field replaces the placeholder variable in the destination dashboard.

        -   Static variable: The static variable that you specify is used for the query.
            -   **Variable**: The name of the static variable. For example, the placeholder variable that you specify in the destination dashboard is `static_ip`.
            -   **Static Value**: The value of the static variable that is used to replace the placeholder variable in the destination dashboard. For example, `203.0.113.1`

indicates that the value `203.0.113.1` of the `static_ip` field replaces the placeholder variable in the destination dashboard. Logs whose placeholder variable value is `203.0.113.1` are queried. |

    -   Open HTTP Link

        Set the event action to open a custom HTTP link.

        -   The path in the HTTP link is the path of the destination file.
        -   If you add an optional parameter to the path and click a field value to trigger the advanced event, the added parameter is replaced by the field value. At the same time, you are redirected to the new HTTP link.
        |Parameter|Description|
        |:--------|:----------|
        |**Configuration Name**|The name of the advanced event.|
        |**Event Action**|Select **Custom HTTP Link**.|
        |**Protocol**|The protocol type that is used to access the custom HTTP link. You can select HTTP or custom protocol.|
        |**Enter Link**|The address to which you want to be redirected. For example, if you enter `www.baidu.com/s?wd=${sls_project}`, you are redirected to this address after the event is triggered. The $\{sls\_project\} parameter is replaced by the name of your project. |
        |**Use System Variables**|If you turn on the **Use System Variables** switch, you can insert variables that are provided by Log Service into the HTTP link. The variables are $\{sls\_project\}, $\{sls\_dashboard\_title\}, $\{sls\_chart\_name\}, $\{sls\_chart\_title\}, $\{sls\_region\}, $\{sls\_start\_time\}, $\{sls\_end\_time\}, $\{sls\_realUid\}, and $\{sls\_aliUid\}.|
        |**Transcoding**|If you turn on the **Transcoding** switch, the HTTP link is encoded.|
        |**Optional Parameter Fields**|If you add an optional parameter to the path, the parameter is replaced by the field value when you click a field value to trigger the advanced event.|


## Example

The following example describes how to store access logs in a Logstore named accesslog. In this example, a saved search is created to query the page view \(PV\) distribution of IP addresses and request methods. On the Raw Logs page, set the advanced event for the remote\_addr field to open a saved search. Then, click remote\_addr. You are redirected to the saved search to view the PV distribution.

Raw log entry:

```
__source__:127.0.0.1
__tag__:__receive_time__:1613759995
__topic__:nginx_access_log
body_bytes_sent:5077
host:www.dre.mock.com
http_referer:www.jn.mock.com
http_user_agent:Mozilla/5.0 (X11; CrOS i686 12.0.742.91) AppleWebKit/534.30 (KHTML, like Gecko) Chrome/12.0.742.93 Safari/534.30
http_x_forwarded_for:119.0.25.193
remote_addr:203.0.113.1
remote_user:gp_02
request_length:3932
request_method:POST
request_time:35
request_uri:/request/path-2/file-4
status:200
time_local:19/Feb/2021:18:39:50
upstream_response_time:0.09
```

Procedure

1.  Query the PV distribution of requests whose request method is POST and status code is 200. Create a saved search named **PV Distribution of IP Addresses and Request Method**. The following example shows the query statement and query result:

    ```
    * and request_method: POST and status: 200 | select count(*) as pv, remote_addr as ip,request_method as method group by ip,method order by ip desc
    ```

    ![step1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5381009161/p266519.png)

2.  Set the `method` and `status2` variables in the query statement. The following example shows the query statement:

    ```
    * and request_method: ${method} and status: ${status2} | select count(*) as pv, remote_addr as ip,request_method as method group by ip,method order by ip desc
    ```

3.  On the Raw Logs tab, set the advanced event for the remote\_addr field to **Open Saved Search** and set the following parameters.
    -   Select Quick Query: Select PV Distribution of IP Addresses and Request Method.
    -   Filter: You do not need to specify the parameters on this tab.
    -   Variables: Set the key of a static variable to status2 and the value to 400. Set the key of a dynamic variable to method and the value to request\_method.
4.  On the Raw Logs tab, choose **remote\_addr** \> **PV Distribution of IP Addresses and Request Method**.

    In the raw log entry, the request\_method is GET and the status is 404.

    ![step4](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5381009161/p266520.png)

5.  Click the name of the saved search. The following query statement is displayed in the window that appears:

    ```
    * and request_method: GET and status: 400 | select count(*) as pv, remote_addr as ip,request_method as method group by ip,method order by ip desc
    ```

6.  View the query result of the saved search.

    In this example, the value of the static variable status2 is 400, which indicates the status field. The value of the request\_method field is GET and the dynamic value of the variable method is GET. The result of the saved search shows the PV distribution of IP addresses whose request method is GET and status code is 400.

    For example, the value of therequest\_method field is PUT. The result of the saved search shows the PV distribution of IP addresses whose request method is PUT and status code is 400.

    ![step6](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5381009161/p266521.png)


