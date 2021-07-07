# Manage machine groups

This topic describes how to view the list of machine groups, modify a machine group, and view the status of a machine group. This topic also describes how to manage Logtail configurations, and delete a machine group in the Log Service console.

## View the list of machine groups

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the destination project.

3.  In the left-side navigation pane, click **Machine Groups** to view the list of machine groups.


## Modify a machine group

1.  In the list of Machine Groups, click the name of the target machine group.

2.  On the Machine Group Settings page, click **Modify**.

3.  After you modify the configurations of the machine group, click **Save**.


## View the status of a machine group

You can view the heartbeat status of a machine group to check whether Logtail is installed on all servers in the machine group.

**Note:** After you create a machine group, wait for 2 minutes before you view the heartbeat status.

1.  In the list of Machine Groups, click the target name of the machine group.

2.  On the Machine Group Settings page, you can view the status of the machine group.

    -   If the heartbeat status is OK, Logtail is installed on each server in the machine group, and is connected to Log Service.
    -   If the heartbeat status is FAIL, Logtail fails to connect to Log Service. You can click Reason next to the FAIL status for troubleshooting. For more information, see [What can I do if the Logtail client has no heartbeat?](/intl.en-US/Data Collection/FAQ/Troubleshooting for Logtail machine group without heartbeat in the log Service Console.md). If errors persist, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm?spm=a2796.7919406.0.dcontactus3.676a2d23RjosdV#/ticket/add/?productId=1210).

## Manage Logtail configurations

You can create Logtail configurations in Log Service and apply the Logtail configurations to a machine group.

1.  In the list of Machine Groups, click the target name of the machine group.

2.  On the Machine Group Settings page, click **Modify**.

3.  In the Configurations section, you can add Logtail configurations to or remove them from the machine group.

    After a Logtail configuration is added to the machine group, the Logtail configuration is applied to Logtail that is installed on each server in the machine group. After a Logtail configuration is removed from a machine group, the Logtail configuration is no longer applied to Logtail of each server in the machine group.

4.  Click **Save**.


## Delete a machine group

1.  In the list of Machine Groups, click the ![Machine group management icon](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4123359951/p52791.png) icon next to the target machine group, and select **Delete** from the shortcut menu.

2.  In the dialog box that appears, click **OK**.


