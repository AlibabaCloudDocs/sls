# Inherit alert policies

This topic describes how to inherit alert policies.

You can inherit alert policies. If you inherit alert policies, alerts are processed based on the combination of the parent policy and the child policy.

For example, the following figures show the configurations of the parent and child policies:

![Parent policy](../images/p254716.png "Parent policy")

![Child policy](../images/p254717.png "Child policy")

The final alert policy is shown in [Figure 3](#fig_3rz_nzt_oqc). When the alert monitoring module executes an alert policy, the parent policy is executed first, then the child policy is executed.

![Final child policy](../images/p254718.png "Final child policy")

In the final configurations of the child policy, if sub-policy exists, the sub-policies can still be executed. Therefore, the end node does not indicate the end of the alert policy. The end node on the right of the conditional node indicates the end to the execution of the alert policy. Therefore, if a parent policy is not executed in an alert monitoring module, the child policy is not executed. If a parent policy is executed in an alert monitoring module, the child policy is executed.

