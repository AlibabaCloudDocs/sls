# Create an alert template

Log Service sends alert notifications based on the content that is specified in alert templates.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Go to the Alert Template page.

    1.  In the Projects section, click the name of the required project.

    2.  In the left-side navigation pane, click **Alerts**.

    3.  Click **Open Alert Center** and choose **Alert Management** \> **Alert Template**.

3.  Create an alert template.

    1.  Click **Create**.

    2.  Configure the ID and name of the content template.

    3.  Configure notification content for each alert notification method.

        In the Add Content Template dialog box, you can configure notification content on the SMS, Voice, Email, DingTalk \(WebHook\), WebHook-Custom, and Notifications tabs.

        ![Alert template](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0864072261/p245226.png)

        |Notification method|Parameter|
        |-------------------|---------|
        |SMS|The following parameters are available:        -   **Language**: the language of the alert notification.
        -   **Content**: the content of the alert notification. The content must be 1 to 100 characters in length.

You can use template variables. For more information, see [t2061590.dita\#reference\_2061590](/intl.en-US/Alerting/Alerting (New)/Notification management/Manage notification methods/Template variables.md). |
        |Voice|The following parameters are available:        -   **Language**: the language of the alert notification.
        -   **Content**: the content of the alert notification. The content must be 1 to 100 characters in length.

You can use template variables. For more information, see [t2061590.dita\#reference\_2061590](/intl.en-US/Alerting/Alerting (New)/Notification management/Manage notification methods/Template variables.md). |
        |Email|The following parameters are available:        -   **Language**: the language of the alert notification.
        -   **Content**: the content of the alert notification. The content must be 1 to 500 characters in length.

You can use template variables. For more information, see [t2061590.dita\#reference\_2061590](/intl.en-US/Alerting/Alerting (New)/Notification management/Manage notification methods/Template variables.md). |
        |DingTalk \(Webhook\)|The following parameters are available:        -   **Title**: the title of the alert notification. The title must be 1 to 100 characters in length.

You can use template variables. For more information, see [t2002327.dita\#task\_2002327/section\_7uv\_wvc\_7wh](t2002327.dita#task_2002327/section_7uv_wvc_7wh).

        -   **Content**: the content of the alert notification. The content must be 1 to 500 characters in length.

You can use template variables. For more information, see [t2061590.dita\#reference\_2061590](/intl.en-US/Alerting/Alerting (New)/Notification management/Manage notification methods/Template variables.md). |
        |Webhook-Custom|The following parameters are available:        -   **Sending Mode**: Valid options include Single and Batch.

            -   If you select Batch and specify the maximum number of items sent in a group, only the first N alerts in an alert set are sent.
            -   If you select Batch and the content you configured can be parsed into JSON data, the JSON data is sent. Otherwise, string arrays are sent.
For example, if you customize the content as `{"project": "${project}", "alert_name": "${alert_name}"}`, the following notifications are sent for two alerts:

            -   Single: sends two alert notifications. Content: `{"project": "project-1", "alert_name": "alert-1"}` and `{"project": "project-2", "alert_name": "alert-2"}`.
            -   Batch: sends one alert notification. Content: `[{"project": "project-1", "alert_name": "alert-1"}, { "project": "project-2", "alert_name": "alert-2"}]`.
        -   **Content**: the content of the alert notification. The content must be 1 to 500 characters in length.

You can use template variables. For more information, see [t2061590.dita\#reference\_2061590](/intl.en-US/Alerting/Alerting (New)/Notification management/Manage notification methods/Template variables.md).

**Note:** The default request header is **Content-Type: application/json;charset=utf-8** when Log Service sends alert notifications. If a webhook receiver requires request headers in other formats, you can customize the request headers when you configure alert notifications. For more information, see [Webhook](/intl.en-US/Alerting/Alerting (New)/Notification management/Manage notification methods/Notification methods.md). |
        |Notifications|The following parameters are available:        -   **Language**: the language of the alert notification.
        -   **Content**: the content of the alert notification. The content must be 1 to 500 characters in length.

You can use template variables. For more information, see [t2061590.dita\#reference\_2061590](/intl.en-US/Alerting/Alerting (New)/Notification management/Manage notification methods/Template variables.md). |


