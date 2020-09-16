# Enable the inner-ActionTrail feature

This topic describes how to collect operations logs of cloud services by using the Log Service console.

-   You are authorized to use the inner-ActionTrail feature.
-   ActionTrail is authorized to assume the AliyunActionTrailDefaultRole role to ship logs to Log Service.

    You can go to the [Cloud Resource Access Authorization](https://ram.console.aliyun.com/#/role/authorize?request=%7B%22Requests%22:%7B%22request1%22:%7B%22RoleName%22:%22AliyunActionTrailDefaultRole%22,%22TemplateId%22:%22DefaultRole%22%7D%7D,%22ReturnUrl%22:%22https:%2F%2Factiontrail.console.aliyun.com%2Fcn-hangzhou%2Fevent-list%3Faccounttraceid%3D1c54b57f6641430aa281cfc2d62cf398upoo%22,%22Service%22:%22ActionTrail%22%7D) page to complete the authorization.

    **Note:**

    -   You need to perform this operation only if ActionTrail was not authorized to assume the AliyunActionTrailDefaultRole role.
    -   If you use a RAM user to log on to ActionTrail, you must authorize the RAM user by using an Alibaba Cloud account.
    -   You must not delete the RAM role or revoke the permissions from the RAM role. Otherwise, logs cannot be shipped to Log Service.
-   A project and a Logstore are created. For more information, see [Create a Logtail project and a Logstore](/intl.en-US/Quick Start/Quick start.md).

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, click the **Platform Operation Log \(Inner-ActionTrail\)** card.

    You can also log on to the [ActionTrail console](https://actiontrail.console.aliyun.com/). On the page that appears, choose **Inner-ActionTrail** \> **Trails**. On the Create Trail page, set the parameters to send operations logs to Log Service.

    **Note:**

    -   If you enable the inner-ActionTrail feature in the ActionTrail console and configure Log Service as the shipping destination, a dedicated Logstore named in the format of innertrail\_Trail Name is automatically created to store operations logs.
    -   If you enable the feature in the Log Service console, the settings that you configure in the Log Service console are not synchronized to the ActionTrail console. If you enable the feature in the Log Service console and create a trail in the ActionTrail console, the settings in the ActionTrail console overwrite those in the Log Service console.
    -   If you cannot find the **Inner-ActionTrail** card in the Import Data section or **Inner-ActionTrail** \> **Trails** in the ActionTrail console, submit a ticket or contact technical support.
3.  In the Specify Logstore step, select the target project and Logstore, and click **Next**.

    You can also click **Create Now** to create a project and a Logstore. For more information, see [Step 1: Create a project and a Logstore](/intl.en-US/Quick Start/Quick start.md).

4.  In the Specify Data Source step, click **Next**.

    ![Data Import](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2191420061/p130651.png)

    **Note:**

    -   All operations logs are shipped to the same Logstore.
    -   You can disable the inner-ActionTrail feature in the Specify Data Source step.
    -   You can disable the inner-ActionTrail feature by choosing **Inner-ActionTrail** \> **Trails** in the ActionTrail console.
    -   After you disable the inner-ActionTrail log shipping feature, new logs are not shipped to the dedicated Logstore. The logs that have been shipped to the dedicated Logstore are automatically deleted after the retention period expires.
5.  In the Configure Query and Analysis step, click **Next**.

    The indexing feature is automatically enabled for the dedicated Logstore and indexes are created for the data in the Logstore.


After operations logs are sent to Log Service, you can query, download, ship, and transform the logs. You can also create alerts for the logs. For more information, see [Common operations on logs of Alibaba Cloud services](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).

