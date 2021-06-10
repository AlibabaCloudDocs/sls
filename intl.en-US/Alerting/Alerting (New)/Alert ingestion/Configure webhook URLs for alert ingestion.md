# Configure webhook URLs for alert ingestion

You can configure webhook URLs for alert ingestion by creating an alert ingestion service and an alert ingestion application in the Log Service console. Then, Log Service can receive alerts from external monitoring systems.

## Step 1: Create an alert ingestion service and an alert ingestion application

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Go to the Alert Ingestion page.

    1.  In the Projects section, click a project.

    2.  In the left-side navigation pane, click **Alerts**.

    3.  Click **Open Alert Center** and choose **Alert Management** \> **Alert Ingestion**.

3.  Create an alert ingestion service.

    1.  Click **Create**.

    2.  In the Add Service dialog box, specify the ID and name of the service, and click **Save**.

4.  Create an alert ingestion application.

    1.  Click **Application** in the Actions column of the service.

    2.  In the Application Management dialog box, click **Create**.

    3.  In the Add Application dialog box, set the required parameters, and click **Save**. The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |**ID**|The ID of the alert ingestion application.|
        |**Name**|The name of the alert ingestion application.|
        |**Protocol Verification**|
        |**Protocol**|The format of alerts. Valid values:        -   Prometheus Alert: receives Prometheus alerts.
        -   Grafana Alert: receives Grafana alerts. |
        |**Alert Policy**|The alert policy that is used to merge, silence, and suppress alerts. For more information, see [Alert management overview](/intl.en-US/Alerting/Alerting (New)/Alert Management/Overview.md).|
        |**Action Policy**|The action policy that is used to manage alert notification methods and the frequency at which alert notifications are sent. For more information, see [Create an action policy](/intl.en-US/Alerting/Alerting (New)/Notification management/Create an action policy.md). **Note:** This parameter is valid only if you select Simple Mode or Standard Mode in the **Alert Policy** section, or select **sls.builtin.dynamic** for the **Action Policy** parameter when you create an alert policy. For more information, see [Dynamic action policies](/intl.en-US/Alerting/Alerting (New)/Alert Management/Denoise alerts/Dynamic action policies.md). |
        |**Cycle**|If duplicate alerts are triggered in the specified cycle, the selected action policy is executed only once, and only one alert notification is sent.|
        |**Whitelist**|If you turn on this switch, you must add an AccessKey ID. An AccessKey ID is part of the path to an access port. Only the webhook URL that includes a valid AccessKey ID can be used to access alerts. For example, you specify an AccessKey ID as **AEDC\*\*\*\*ERT**, and then you set the \{ACCESS\_KEY\_ID\} variable of the path\_prefix parameter in Prometheus to **AEDC\*\*\*\*ERT**. In this case, the related alerts can be ingested into the alerting system of Log Service.

**Note:** The Resource Access Management \(RAM\) user to which the AccessKey pair belongs must be granted the AliyunLogOpenEventWrite permission. For more information, see [Appendix: Obtain an AccessKey ID](#section_v9z_jmt_d4y). |
        |**Filter**|
        |**Filter by Keyword**|The alert ingestion system ingests only the alerts that contain one or more specified keywords.|
        |**Enrichment**|
        |**Add Label**|Add labels for alerts. Labels are formatted in key-value pairs. For example, you can set the key of a label to **Environment** and set the related value to **Staging environment**. For more information, see [Labels](/intl.en-US/Alerting/Alerting (New)/Alert monitoring/Orchestration of monitoring rules/Labels and annotations.md).|
        |**Add Annotation**|Add annotations for alerts. Annotations are formatted in key-value pairs. For example, set the key of an annotation to **title** and set the related value to **Prometheus alert**. For more information, see [Annotations](/intl.en-US/Alerting/Alerting (New)/Alert monitoring/Orchestration of monitoring rules/Labels and annotations.md). **Note:** Alert template variables cannot be referenced when you set the Add Annotation parameter. |
        |**Quota**|
        |**Peak Requests**|The maximum number of notifications that can be received per minute. After the threshold is exceeded, no more alerts can be ingested. Valid values: 100 to 10000. Unit: Requests/Minute.|


## Step 2: Obtain webhook URLs

You must use the related webhook URL when you configure required parameters in Prometheus or Grafana.

1.  In the Application Management dialog box, click **Webhook URLs** of the alert ingestion application.

2.  In the Webhook URLs dialog box, select the region where alerts are ingested.

    **Note:** If your Prometheus or Grafana server is deployed on an Elastic Compute Service \(ECS\) instance, we recommend that you select the region where the ECS instance resides and use the related LAN or virtual private cloud \(VPC\) webhook URL. You can also use the Internet webhook URL of a region.

3.  Move the pointer over the blank area next to the text box and copy the full URL, domain name, or subpath of the webhook URL.

    Replace the \{ACCESS\_KEY\_ID\} variable in a webhook URL with the AccessKey ID that is granted the required permissions. For information about how to obtain an AccessKey ID, see [Appendix: Obtain an AccessKey ID](#section_v9z_jmt_d4y).

    ![Webhook URLs](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0527133261/p267355.png)


## Appendix: Obtain an AccessKey ID

To ensure the security of your Alibaba Cloud account, we recommend that you use a Resource Access Management \(RAM\) user rather than the Alibaba Cloud account to ingest alerts. The RAM user that you use to ingest alerts must be granted the AliyunLogPutOpenEventPolicy permission. To obtain an AccessKey ID, perform the following steps:

1.  Create a RAM user. For more information, see [Create a RAM user](/intl.en-US/RAM User Management/Basic operations/Create a RAM user.md).

2.  Grant the AliyunLogPutOpenEventPolicy permission to the RAM user. For more information, see [Grant permissions to a RAM user](/intl.en-US/RAM User Management/Authorization management/Grant permissions to a RAM user.md).

3.  Create an AccessKey pair that includes an AccessKey ID for the RAM user. For more information, see [Create an AccessKey pair for a RAM user](/intl.en-US/Security Settings/AccessKey pairs/Create an AccessKey pair for a RAM user.md).


-   [t2071251.md\#]()
-   [Ingest Grafana alerts into Log Service]()

