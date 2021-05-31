# Why am I unable to collect SLB access logs?

This topic describes how to troubleshoot in cases where you are unable to collect SLB access logs.

## 1. Check whether the access log collection function has been activated for SLB instances.

Activate the access log collection function for each SLB instance separately. Then, the generated access logs can be written into your Logstore in real time.

To do so, log on to the SLB console, and choose **Logs** \> **Access Logs** to view the **Access Logs \(Layer-7\)** list.

-   Verify that the specified SLB instance exists.
-   Confirm the **Storage Path** of the SLB instance.

    This column displays information about the project and Logstore. In this case, make sure that you check whether SLB logs exist in the correct location in the console.


## 2. Check whether RAM users are correctly authorized.

During activation of the access log collection function, the system guides you through RAM user authorization. The function can be successfully activated only after RAM users are successfully authorized. If RAM users are incorrectly created or deleted, the collected logs cannot be delivered to your Logstore.

**Troubleshooting**

Log on to the [RAM Console](https://ram.console.aliyun.com/#/role/list). On the **RAM Roles** page, check whether the AliyunLogArchiveRole role exists.

-   **If AliyunLogArchiveRole does not exist**, use your Alibaba Cloud account to log on to the RAM console and click the [quick authorization link](https://ram.console.aliyun.com/#/role/authorize?request=%7B%22Requests%22%3A%20%7B%22request1%22%3A%20%7B%22RoleName%22%3A%20%22AliyunLogArchiveRole%22%2C%20%22TemplateId%22%3A%20%22Archive%22%7D%7D%2C%20%22ReturnUrl%22%3A%20%22https%3A//sls.console.aliyun.com/%22%2C%20%22Service%22%3A%20%22Log%22%7D) to create the RAM users required for authorization.

-   **If AliyunLogArchiveRole exists**, click the role name and check whether the role is correctly authorized.

    The following shows the default policy. If your policy has been modified, we recommend that you replace the current policy with the default policy.

    ```
    {
      "Version": "1",
      "Statement": [
        {
          "Action": [
            "log:PostLogStoreLogs"
          ],
          "Resource": "*",
          "Effect": "Allow"
        }
      ]
    }
    ```


## 3. Check whether any log is generated.

If you do not find any SLB access log in the Log Service console, it is likely that no log has been generated. Possible causes include:

-   Layer-7 listening is not configured for the current instance.

    **Currently, only instances configured with layer-7 listening are supported**. Common layer-7 listening protocols include HTTP and HTTPS. For more information, see [Listener overview](/intl.en-US/Classic Load Balancer/User Guide/Listeners/Listener overview.md).

-   Historical logs that were generated before activation of the access log collection function are not collected.

    Instead, only logs that are generated after activation of the access log collection function are collected.

-   The specified instance did not receive a request.

    Logs are generated only when you request access to the listener of the instance.


