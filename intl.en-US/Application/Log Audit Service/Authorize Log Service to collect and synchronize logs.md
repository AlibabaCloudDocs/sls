# Authorize Log Service to collect and synchronize logs

You can use the Log Audit Service application of Log Service to collect cloud service logs of an Alibaba Cloud account. To use the application, you must authorize Log Service to collect logs from relevant Alibaba Cloud services of the current account. If you want to use the application to collect logs across multiple Alibaba Cloud accounts, you must also authorize the other Alibaba Cloud accounts. You can use the AccessKey pair of a Resource Access Management \(RAM\) user who has the required permissions to complete the authorization. You can also follow the steps described in this topic to complete the authorization.

You can use the Log Audit Service application to collect cloud service logs of an Alibaba Cloud account or across multiple Alibaba Cloud accounts.

To collect cloud service logs across multiple Alibaba Cloud accounts, you must authorize the current Alibaba Cloud account and the other Alibaba Cloud accounts.

-   Authorize the current Alibaba Cloud account to receive logs from the other Alibaba Cloud accounts. The logs are stored in the Logstore dedicated to log audit.
-   Authorize the other Alibaba Cloud accounts to synchronize logs to the current Alibaba Cloud account. The logs are stored in the Logstore dedicated to log audit.

The Log Audit Service application of Log Service involves multiple authorization roles and policies. The following tables list the relationships among these roles and policies.

-   Current Alibaba Cloud account

    |Role|Policy|
    |----|------|
    |[sls-audit-service-dispatch](https://ram.console.aliyun.com/roles/sls-audit-service-dispatch)|[AliyunLogAuditServiceDispatchPolicy](https://ram.console.aliyun.com/policies/AliyunLogAuditServiceDispatchPolicy/Custom/content)|
    |[sls-audit-service-monitor](https://ram.console.aliyun.com/roles/sls-audit-service-monitor)|    -   [ReadOnlyAccess](https://ram.console.aliyun.com/policies/ReadOnlyAccess/System/content)
    -   [AliyunLogAuditServiceMonitorAccess](https://ram.console.aliyun.com/policies/AliyunLogAuditServiceMonitorAccess/Custom/content)
    -   [AliyunLogAuditServiceK8sAccess](https://ram.console.aliyun.com/policies/AliyunLogAuditServiceK8sAccess/Custom/content) \(Required only when Kubernetes collection is enabled\) |

-   Other Alibaba Cloud accounts

    |Role|Policy|
    |----|------|
    |[sls-audit-service-monitor](https://ram.console.aliyun.com/roles/sls-audit-service-monitor)|    -   [ReadOnlyAccess](https://ram.console.aliyun.com/policies/ReadOnlyAccess/System/content)
    -   [AliyunLogAuditServiceMonitorAccess](https://ram.console.aliyun.com/policies/AliyunLogAuditServiceMonitorAccess/Custom/content)
    -   [AliyunLogAuditServiceK8sAccess](https://ram.console.aliyun.com/policies/AliyunLogAuditServiceK8sAccess/Custom/content) \(Required only when Kubernetes collection is enabled\) |


## Authorize Log Service to collect cloud service logs of the current Alibaba Cloud account

1.  Log on to the [RAM console](https://ram.console.aliyun.com/).

    We recommend that you use a RAM user to complete the authorization. The RAM user must be granted the read and write permissions on RAM. For example, you can attach the AliyunRAMFullAccess policy to the RAM user.

2.  Create a policy named AliyunLogAuditServiceDispatchPolicy.

    1.  In the left-side navigation pane, choose **Permissions** \> **Policies**. On the page that appears, click **Create Policy**.

    2.  On the Create Custom Policy page, set the required parameters, and then click **OK**. The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |**Policy Name**|Set the value to **AliyunLogAuditServiceDispatchPolicy**.|
        |**Configuration Mode**|Select **Script**.|
        |**Policy Document**|Replace the content in the editor with the following script:         ```
{
    "Version": "1",
    "Statement": [
        {
            "Action": "log:*",
            "Resource": [
                "acs:log:*:*:project/slsaudit-*"
            ],
            "Effect": "Allow"
        }
    ]
}
        ``` |

3.  Follow [Step 2](#step_5vt_bc3_gnt) to create a policy named AliyunLogAuditServiceMonitorAccess.

    The following table describes the parameters.

    |Parameter|Description|
    |---------|-----------|
    |**Policy Name**|Set the value to **AliyunLogAuditServiceMonitorAccess**.|
    |**Configuration Mode**|Select **Script**.|
    |**Policy Document**|Replace the content in the editor with the following script:     ```
{
    "Version": "1",
    "Statement": [
        {
            "Action": "log:*",
            "Resource": [
                "acs:log:*:*:project/slsaudit-*",
                "acs:log:*:*:app/audit"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "rds:ModifySQLCollectorPolicy",
                "vpc:*FlowLog*",
                "drds:*SqlAudit*",
                "kvstore:ModifyAuditLogConfig",
                "polardb:ModifyDBClusterAuditLogCollector"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
    ``` |

    **Note:** To collect Kubernetes data, you must create a policy named AliyunLogAuditServiceK8sAccess. The following script shows the policy document. After the AliyunLogAuditServiceK8sAccess policy is created, you must create a role named sls-audit-service-monitor and attach the policy to the role. For more information, see [Step 5](#step_50a_1cj_0w9).

    ```
    {
        "Version": "1",
        "Statement": [
            {
                "Action": "log:*",
                "Resource": [
                    "acs:log:*:*:project/k8s-log-*"
                ],
                "Effect": "Allow"
            }
        ]
    }
    ```

4.  Create a role named sls-audit-service-dispatch and attach the AliyunLogAuditServiceDispatchPolicy policy to the role.

    1.  In the left-side navigation pane, click **RAM Roles**. On the page that appears, click **Create RAM Role**. The Create RAM Role wizard appears.

    2.  In the Select Role Type step, select **Alibaba Cloud Service**, and then click **Next**.

    3.  In the Configure Role step, set the required parameters, and then click **OK**. The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |Role Type|Select **Normal Service Role**.|
        |RAM Role Name|Set the value to **sls-audit-service-dispatch**.|
        |Select Trusted Service|Select **Log Service** from the drop-down list. The following script shows the content of the trust policy:         ```
{    
"Statement": [
        {
            "Action": "sts:AssumeRole",
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "log.aliyuncs.com"
                ]
            }
        }
    ],
    "Version": "1"
}
        ``` |

    4.  In the Finish step, click **Add Permissions to RAM Role**.

    5.  In the Add Permissions dialog box, set the required parameters to attach a policy to the role.

        In the Select Policy section, select **Custom Policy**, select the **AliyunLogAuditServiceDispatchPolicy** policy from the policy list, and then click OK.

5.  Create a role named sls-audit-service-monitor and attach the AliyunLogAuditServiceMonitorAccess policy to the role.

    1.  In the left-side navigation pane, click **RAM Roles**. On the page that appears, click **Create RAM Role**. The Create RAM Role wizard appears.

    2.  In the Select Role Type step, select **Alibaba Cloud Service**, and then click **Next**.

    3.  In the Configure Role step, set the required parameters, and then click **OK**. The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |Role Type|Select **Normal Service Role**.|
        |RAM Role Name|Set the value to **sls-audit-service-monitor**.|
        |Select Trusted Service|Select **Log Service** from the drop-down list. The following script shows the content of the trust policy:         ```
{    
"Statement": [
        {
            "Action": "sts:AssumeRole",
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "log.aliyuncs.com"
                ]
            }
        }
    ],
    "Version": "1"
}
        ``` |

    4.  In the Finish step, click **Add Permissions to RAM Role**.

    5.  In the Add Permissions dialog box, set the required parameters to attach a policy to the role.

        In the Select Policy section, select **Custom Policy** and select the **AliyunLogAuditServiceMonitorAccess** policy from the policy list. Select **System Policy**, select the **ReadOnlyAccess** policy from the policy list, and then click OK.


## Authorize Log Service to collect logs from cloud services across multiple Alibaba Cloud accounts

1.  Obtain the IDs of the Alibaba Cloud accounts.

    1.  Log on to the [Account Management console](https://account-intl.console.aliyun.com/).

        Log on to the console by using each Alibaba Cloud account.

    2.  Go to the Security Settings page and obtain the ID of the account.

2.  Authorize the current Alibaba Cloud account.

    1.  Log on to the [RAM console](https://ram.console.aliyun.com/).

        We recommend that you use a RAM user to complete the authorization. The RAM user must be granted the read and write permissions on RAM. For example, you can attach the AliyunRAMFullAccess policy to the RAM user.

    2.  In the left-side navigation pane, click **RAM Roles**.

    3.  On the RAM Roles page, find and click the **sls-audit-service-dispatch** role to go to the details page of the role.

    4.  Click the Trust Policy Management tab. On the tab, replace the content in the editor with the following script:

        ```
        {
            "Statement": [
                {
                    "Action": "sts:AssumeRole",
                    "Effect": "Allow",
                    "Principal": {
                        "Service": [
                            "log.aliyuncs.com"
                        ]
                    }
                }
            ],
            "Version": "1"
        }
        ```

3.  Authorize the other Alibaba Cloud accounts.

    1.  Log on to the [RAM console](https://ram.console.aliyun.com/).

        We recommend that you use a RAM user to complete the authorization. The RAM user must be granted the read and write permissions on RAM. For example, you can attach the AliyunRAMFullAccess policy to the RAM user.

    2.  In the left-side navigation pane, choose **Permissions** \> **Policies**. On the page that appears, click **Create Policy**.

    3.  On the Create Custom Policy page, set the required parameters, and then click **OK**. The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |**Policy Name**|Set the value to **AliyunLogAuditServiceMonitorAccess**.|
        |**Configuration Mode**|Select **Script**.|
        |**Policy Document**|Replace the content in the editor with the following script:         ```
{
    "Version": "1",
    "Statement": [
        {
            "Action": "log:*",
            "Resource": [
                "acs:log:*:*:project/slsaudit-*",
                "acs:log:*:*:app/audit"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "rds:ModifySQLCollectorPolicy",
                "vpc:*FlowLog*",
                "drds:*SqlAudit*",
                "kvstore:ModifyAuditLogConfig",
                "polardb:ModifyDBClusterAuditLogCollector"                      
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}

                                                    
        ``` |

        **Note:** To collect Kubernetes data, you must create a policy named AliyunLogAuditServiceK8sAccess. The following script shows the policy document. After the AliyunLogAuditServiceK8sAccess policy is created, you must create a role named sls-audit-service-monitor and attach the policy to the role. For more information, see [Step 3.iv](#step_i8i_1sb_c5u).

        ```
        {
            "Version": "1",
            "Statement": [
                {
                    "Action": "log:*",
                    "Resource": [
                        "acs:log:*:*:project/k8s-log-*"
                    ],
                    "Effect": "Allow"
                }
            ]
        }
        ```

    4.  Create a role named sls-audit-service-monitorand attach the AliyunLogAuditServiceMonitorAccess policy to the role.

        1.  In the left-side navigation pane, click **RAM Roles**. On the page that appears, click **Create RAM Role**. The Create RAM Role wizard appears.
        2.  In the Select Role Type step, select **Alibaba Cloud Service**, and then click **Next**.
        3.  In the Configure Role step, set the required parameters, and then click **OK**. The following table describes the parameters.

            |Parameter|Description|
            |---------|-----------|
            |Role Type|Select **Normal Service Role**.|
            |RAM Role Name|Set the value to **sls-audit-service-monitor**.|
            |Select Trusted Service|Select **Log Service** from the drop-down list.|

        4.  In the Finish step, click **Add Permissions to RAM Role**.
        5.  In the Add Permissions dialog box, set the required parameters to attach a policy to the role.

            In the Select Policy section, select **Custom Policy** and select the **AliyunLogAuditServiceMonitorAccess** policy from the policy list. Select **System Policy**, select the **ReadOnlyAccess** policy from the policy list, and then click OK.

        6.  Go back to the RAM Roles page, find and click the **sls-audit-service-monitor** role to go to the details page of the role.
        7.  Click the Trust Policy Management tab. On the tab, replace the content in the editor with the following script.

            Replace Alibaba Cloud account ID in the following script with the ID of the current Alibaba Cloud account. For information about how to obtain the Alibaba Cloud account ID, see [Step 1](#step_nk5_xxr_2bu).

            ```
             {
                "Statement": [
                    {
                        "Action": "sts:AssumeRole",
                        "Effect": "Allow",
                        "Principal": {
                            "Service": [
                                "Alibaba Cloud account ID@log.aliyuncs.com",
                                "log.aliyuncs.com"
                            ]
                        }
                    }
                ],
                "Version": "1"
            }
            ```


