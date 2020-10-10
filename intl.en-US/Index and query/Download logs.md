# Download logs

This topic describes how to download logs from Log Service to an on-premises host.

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click a project.

3.  On the **Log Management** \> **Logstores** tab, choose **![Management icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9484688951/p52166.png)** \> **Search & Analysis** to the right of a Logstore.

4.  In the upper-right corner of the Raw Logs tab, click ![Log Download](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1523359951/p41716.png).

5.  In the Log Download dialog box, select a method to download logs, and click **OK**.

    -   Download Log in Current Page: The downloaded file is in the CSV format.
    -   Download All Logs with Cloud Shell: Follow the prompts on the page that appears to complete the download.

        **Note:** The Cloud Shell server is located in the China \(Shanghai\) region. If you download logs from a Logstore that does not reside in the China \(Shanghai\) region, you are charged for data transfer. For more information about pricing, visit the [product page of Log Service](https://www.alibabacloud.com/product/log-service/pricing?spm=5176.2020520112.0.0.3f7214dauuCvAP).

    -   Download All Logs Using Command Line Tool: Follow the instructions on the page that appears to complete the download.

        **Note:**

        -   An AccessKey pair is an identity credential that consists of an AccessKey ID and AccessKey secret. You must set the access-id and access-key parameters in the command based on your AccessKey pair. If you want to use an Alibaba Cloud account to download logs, you can obtain the AccessKey pair of the Alibaba Cloud account in the [User Management console](https://usercenter.console.aliyun.com/#/manage/ak). If a Resource Access Management \(RAM\) user has been created and an AccessKey pair has been created for the RAM user in the [RAM console](https://ram.console.aliyun.com/), you can also use the RAM user for download instead.
        -   If the host on which the Log Service command line is installed reside in the same region as the current project, we recommend that you click running resides in the same region as the current project, we recommend that you click **Switch to Internal Endpoint**. The internal network provides a higher download speed and no charges are incurred during the download.

