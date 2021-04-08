# Configure alert policies

Action policies are attached to the alert rules in the Log Audit Service application based on the built-in alert policy. You cannot replace the built-in alert policy. However, you can modify the configurations of the policy. This topic describes how to create alert policies and modify the action policies that are attached to the built-in alert policy.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Log Application section, click **Log Audit Service**.

3.  In the left-side navigation pane, choose **Audit Alert** \> **Policy Settings** \> **Alert Policy**.

4.  Find the built-in alert policy and click **Edit**.

5.  Modify the action policies that are attached to the built-in alert policy.

    By default, the built-in alert policy are attached to the built-in action policy: action\_policy="sls.app.audit.builtin You can modify the value of action\_policy on the **Route Consolidation Policy** tab. This way, you can attach other action policies to the built-in alert policy. For more information about how to create action policies, see [Configure action policies]().

6.  On the Silence Policy tab, add a silence policy.

    You can configure a silence policy to manage the alerts that are triggered within a specified period of time. Log Service allows you to configure silence policies in basic edit mode or advanced edit mode. If the basic edit mode does not meet your business requirements, you can enable the advanced edit mode to configure finer-grained alert policies.

    -   Basic edit mode
        1.  In the Action Policy to Add section, set the required parameters, and click **Add Policy**. The following table describes the parameters.

            ![Silence policy](../images/p188426.png)

            The preceding figure shows that the alert is discarded if you perform related operations by using the Alibaba Cloud account 12\*\*\*\*768768 from the UNIX timestamp 1607072446 to 1607086846. The following table describes the required parameters.

            |Parameter|Description|
            |---------|-----------|
            |Condition|Related actions are performed based on the specified alert policy, which specifies the action, object, operator, and object value.|
            |Silence Time|Select a type and a time range.|

        2.  In the Added Policies section, check the configured policies.

            You can also modify the added policies and adjust the execution sequence of these policies.

            -   To modify a policy, click **Edit** next to the policy.
            -   To adjust the execution sequence of a policy, click the Up or Down arrow next to the policy.
            ![Alert policy](../images/p188206.png)

        3.  Confirm the policy and click **OK**.
    -   Advanced edit mode
        1.  On the Silence Policy tab, turn on the **Advanced Edit mode** switch.
        2.  In the **Action Policy to Add** dialog box, enter a policy statement and click **OK**.

