# Enable the real-time log query feature

Before you query Object Storage Service \(OSS\) access logs in the OSS console, you must enable the real-time log query feature in the OSS console. This topic describes how to enable the real-time log query feature in the OSS console.

-   You have created an Object Storage Service \(OSS\) bucket. For more information, see [Create buckets](/intl.en-US/Quick Start/Create buckets.md).

    The Log Service project that is used to store OSS access logs and the OSS bucket belong to the same Alibaba Cloud account and reside in the same region.

-   Log Service is authorized to assume the AliyunLogImportOSSRole role to access OSS.

    You can go to the [Cloud Resource Access Authorization](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.13.39246c0d6l2MJD#/role/authorize?request=%7B%22Requests%22:%20%7B%22request1%22:%20%7B%22RoleName%22:%20%22AliyunLogImportOSSRole%22,%20%22TemplateId%22:%20%22ImportOSSRole%22%7D%7D,%20%22ReturnUrl%22:%20%22https:%2F%2Fsls.console.aliyun.com%2F%22,%20%22Service%22:%20%22Log%22%7D) page to authorize Log Service to access OSS.

    **Note:**

    -   You need to perform this operation only if Log Service was not authorized to assume the AliyunActionTrailDefaultRole role.
    -   If you use a RAM user to log on to OSS, you must authorize the RAM user by using an Alibaba Cloud account. For more information, see [RAM user authorization](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).
    -   You must not delete the RAM role or revoke the permissions from the RAM role. Otherwise, logs cannot be shipped to Log Service.

## Procedure

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click Buckets. On the Buckets page, click the name of the bucket for which you want to enable the log query feature.

3.  Choose **Logging** \> **Real-time Log Query**.

4.  Click **Activate Now**.

    After you enable the real-time log query feature, Log Service immediately collects logs from OSS. At the same time, a dedicated project and a Logstore are automatically created and indexes are created in the Logstore.


After OSS access logs are collected by Log Service, you can query, download, ship, and transform the logs. You can also create alerts for the logs. For more information, see [Common operations on logs of Alibaba Cloud services](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).

