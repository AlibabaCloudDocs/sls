# Create an on-duty group

Log Service allows you to create on-duty groups to arrange shifts based on cycles and business hours. This topic describes how to create an on-duty group.

A user or user group is created. For more information, see [Create users and user groups](/intl.en-US/Alerting/Alerting (New)/User management/Create users and user groups.md).

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Go to the Shift Group Management tab.

    1.  In the Projects section, click a destination project.

    2.  In the left-side navigation pane, click **Alerts**.

    3.  Click **Open Alert Center**.

    4.  In the **Alert Management** drop-down list, select **Shift Group Management**.

3.  Click **Create**.

4.  Set the required parameters in the Add Shift Group dialog box.

    |Parameter|Description|
    |---------|-----------|
    |**Identifier**|The unique identifier of an on-duty group. An identifier must meet the following rules:    -   The identifier must start with a letter.
    -   The identifier must be 5 to 60 characters in length.
    -   The identifier is a string that can contain digits, letters, underscores \(\_\), hyphens \(-\), and periods \(.\). |
    |**Group Name**|The name of an on-duty group.|
    |**Enable**|Specifies whether to enable the on-duty group.     -   If you turn on the Enable switch, the on-duty group is in the normal state and can be assigned as a valid on-duty group.
    -   If you turn off the Enable switch, the on-duty group is disabled and cannot be assigned as a valid on-duty group. |

5.  Configure a shift plan.

    You can view shift plans on the Final Shift Plan, Work Shift, or Substitute Shifts chart. After you set a rotating shift or a substitute shift, you can view the final schedule on the Final Shift Plan chart. If an employee in an on-duty group is unavailable, you can assign a substitute by using the substitute shift feature.

    1.  Configure a rotating shift.

        1.  Click **![jiahao](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3809872261/p249337.png)** \> **Work Shifts**.
        2.  In the Add Work Shift dialog box, set the following parameters and click **Save**.

            |Parameter|Description|
            |---------|-----------|
            |**Start At**|The time when the rotating shift starts.|
            |**End At**|The time when the rotating shift ends.|
            |**Limit**|The specific time range for the rotating shift.             -   **None**: The rotating shift is available 24 hours every day.
            -   **Business Days**: The rotating shift is valid only on business days. You can customize a specific time range.
            -   **Non-Business Days**: The rotating shift is valid only on non-business days. You can customize a specific time range.
            -   **Every Day**: The rotating shift is valid every day. You can customize a specific time range.
            -   **Every Week**: The rotating shift is valid for a specific period every week. |
            |**Object**|The staff who is on duty. You can select multiple users and user groups. For more information, see [Create users and user groups](/intl.en-US/Alerting/Alerting (New)/User management/Create users and user groups.md). **Note:** If a user or user group is disabled, the user or user group is automatically filtered out when you configure a rotating shift. |
            |**Shift Type**|You can select an hourly shift, daily shift, weekly shift, or monthly shift.|
            |**Shift At**|The specific time when the rotating shift switches based on the shift type.|

    2.  Configure a substitute shift.

        If an employee in an on-duty group is unavailable, you can assign a substitute by using the substitute shift feature.

        1.  Click **![jiahao](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3809872261/p249337.png)** \> **Substitute Shifts**.
        2.  In the Add Substitute dialog box, set the following parameters and click **Save**.

            |Parameter|Description|
            |---------|-----------|
            |**Object**|The original staff who is on duty.|
            |**Substitute**|The substitute staff.|
            |**Start At**|The time when the substitute shift starts.|
            |**End At**|The time when the substitute shift ends. You can select an end time for the substitute shift. You can select to end the substitute shift at the end of this week, next week, this quarter, this year. You can also customize an end time. |
            |**Limit**|The specific time range for the substitute shift.             -   **None**: The substitute shift is available 24 hours every day.
            -   **Business Days**: The substitute shift is valid only on business days. You can customize a specific time range.
            -   **Non-Business Days**: The substitute shift is valid only on non-business days. You can customize a specific time range.
            -   **Every Day**: The substitute shift is valid every day. You can customize a specific time range.
            -   **Every Week**: The substitute shift is valid for a specific period every week. |

    3.  View the final shift plan.

        Click **Rotating Shift and Substitute Shift** \> **Final Shift**.

        ![d](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3809872261/p251109.png)

6.  Configure a calendar.

    Log Service provides Custom Calendar and Global Default Calendar. You can set the time zone, holidays, and default business hours for a calendar. The time that you set for a rotating shift and substitute shift is based on the settings of the calendar.

    -   **Custom Calendar**: Customize business days and holidays on a calendar. The calendar settings are valid only for the on-duty group.
    -   **Global Default Calendar**: The default calendar for the preset country. If you select Global Default Calendar, the current on-duty group uses the same calendar as other on-duty groups. For more information, see [Modify the global default calendar]().
7.  Click **Save**.


