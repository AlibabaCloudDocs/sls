# Enable the inner-ActionTrail feature

This topic describes how to enable the inner-ActionTrail feature and collect operations logs of cloud services by using the Log Service console.

-   You are authorized to use the inner-ActionTrail feature.
-   ActionTrail is authorized to ues the AliyunActionTrailDefaultRole role to ship logs to Log Service.

    You can go to the [Cloud Resource Access Authorization](https://ram.console.aliyun.com/#/role/authorize?request=%7B%22Requests%22:%7B%22request1%22:%7B%22RoleName%22:%22AliyunActionTrailDefaultRole%22,%22TemplateId%22:%22DefaultRole%22%7D%7D,%22ReturnUrl%22:%22https:%2F%2Factiontrail.console.aliyun.com%2Fcn-hangzhou%2Fevent-list%3Faccounttraceid%3D1c54b57f6641430aa281cfc2d62cf398upoo%22,%22Service%22:%22ActionTrail%22%7D) page to complete the authorization.

    **Note:**

    -   This operation is required only when you enable the inner-ActionTrail feature for the first time. You must complete the authorization by using your Alibaba Cloud account.
    -   If you use a RAM user to log on to ActionTrail, you must grant required permissions to the RAM user. For more information, see [RAM user authorization](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).
    -   To ensure that operations logs can be shipped to Log Service, do not revoke permissions from the RAM role or delete the RAM role.
-   A project and a Logstore are created. For more information, see [Quick start](/intl.en-US/Quick Start/Quick start.md).

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, click the **Platform Operation Log \(Inner-ActionTrail\)** card.

    You can also log on to the [ActionTrail console](https://actiontrail.console.aliyun.com/). On the page that appears, choose **Inner-ActionTrail** \> **Trails**. On the Create Trail page, set the parameters to ship operations logs to Log Service.

    **Note:**

    -   If you enable the inner-ActionTrail feature in the ActionTrail console, Log Service creates a dedicated Logstore named innertrail\_Trail Name.
    -   If you enable the inner-ActionTrail feature in the Log Service console, the settings that you configure in the Log Service console are not synchronized to the ActionTrail console. If you enable the inner-ActionTrail feature in the Log Service console and create a trail in the ActionTrail console, the settings that you configure in the ActionTrail console overwrite the settings that you configure in the Log Service console.
    -   If you cannot find the **Platform Operation Log \(Inner-ActionTrail\)** card in the Import Data section or **Inner-ActionTrail** \> **Trails** in the ActionTrail console, submit a ticket or contact technical support.
3.  In the Specify Logstore step, select the target project and Logstore, and click **Next**.

    You can also click **Create Now** to create a project and a Logstore. For more information, see [Step 1: Create a project and a Logstore](/intl.en-US/Quick Start/Quick start.md).

4.  In the Specify Data Source step, click **Next**.

    ![Import data](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2191420061/p130651.png)

    **Note:**

    -   All operations logs are shipped to only one Logstore.
    -   You can disable the inner-ActionTrail feature in the Specify Data Source step.
    -   You can disable the inner-ActionTrail feature by choosing **Inner-ActionTrail** \> **Trails** in the ActionTrail console.
    -   After you disable the inner-ActionTrail feature, new logs are not shipped to the dedicated Logstore. The logs that have been shipped to the dedicated Logstore are automatically deleted after the retention period expires.
5.  In the Configure Query and Analysis step, click **Next**.

    The indexing feature is automatically enabled for the dedicated Logstore and indexes are created for the data in the Logstore.


After operations logs are shipped to Log Service, you can query, analyze, download, ship, and transform the logs. You can also configure alerts for the logs. For more information, see [Common operations on logs of Alibaba Cloud services](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).

