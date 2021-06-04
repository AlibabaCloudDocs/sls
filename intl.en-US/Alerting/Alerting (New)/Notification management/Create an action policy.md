# Create an action policy

You can create an action policy to manage how alert notifications are sent. You can configure an action policy to dynamically send alert notifications to a specified user, user group, or on-duty group. You can also specify to escalate alert incidents.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Go to the action policy tab.

    1.  In the Projects section, click a destination project.

    2.  In the left-side navigation pane, click **Alerts**.

    3.  Click **Open Alert Center** and choose **Alert Management** \> **Action Policy**.

3.  On the Action Policy tab, click **Create**.

4.  In the **Add Action Policy** dialog box, set the **ID** and **Name** parameters.

5.  Add a primary action policy.

    1.  On the Primary Action Policy tab, click ![Condition](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6841072261/p243402.png).

        For example, you can configure an action policy to send notifications to a specified user group by SMS messages if the alerts belong to Alibaba Cloud account 174\*\*\*\*2745.

    2.  Configure a condition to trigger an alert.

        ![Condition](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7231172261/p254158.png)

        |Parameter|Description|
        |---------|-----------|
        |Condition|        -   **All**: The specified action policy is executed only if all alerts in a merge set meet the specified condition.
        -   **Any**: The specified action policy is executed if one or more of the alerts in a merge set meets the specified condition. |
        |Conditional expressions|Alerts that meet a conditional expression are processed based on the specified action policy. You can specify an object, operator, and object value for the conditional expression. For example, you can set the object to an Alibaba Cloud account and set the object value to 174 \*\*\*\* 2745.|
        |Mode|You can add multiple conditions in standard mode or advanced mode.

        -   **Standard Mode**: If you specify multiple conditions, the conditions are associated by the AND operator.
        -   **Advanced Mode**: If you specify multiple conditions, you can use the AND or the OR operator to associate the conditions. You can also group multiple conditions into one group by using parentheses. |

    3.  Configure an action group.

        Set the required parameters for notification methods. Available notification methods include email, DingTalk, webhook, and Alibaba Cloud Message Center. For more information, see [Notification methods]().

        ![Action groups](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7231172261/p254167.png)

    4.  Click ![End ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0356352261/p243468.png) in the Condition or Action Group dialog box to end the configuration.

        Click ![Condition](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6841072261/p243402.png) if you want to add more conditions and action groups.

6.  Configure a secondary action policy.

    You can configure a secondary action policy to process an alert if the alert is not resolved within a long period of time. You can enable the **Enabled** switch on the Secondary Action Policy tab and set the required parameters. For more information, see [Step 5](#step_iaj_mxo_am3).

7.  Click **OK**.


