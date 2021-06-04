# Rotating shifts and substitute shifts

Log Service allows you to configure rotating shifts and substitute shifts.

## Configure an all-day rotating shift

If you do not set a specific time range for a rotating shift, the rotating shift is valid all day by default. For example, you can set a rotating shift for Operation Staff A, Operation Staff B, Operation Staff C, and Operation Staff D. The four staff are on a rotating shift that switches turns on the first business day every week. For more information, see [Create an on-duty group](/intl.en-US/Alerting/Alerting (New)/User management/Create an on-duty group.md).

-   Configurations

    Set the required parameters:

    -   **Limit**: The valid time range for the rotating shift. Set the value to **None**.
    -   **Object**: The on-duty team. Set the value to **Operation Staff A \(testA\), Operation Staff B \(testB\), Operation Staff C \(testC\), and Operation Staff D \(testD\)**.
    -   **Shift Type**: The shift type. Set the value to **Shift: Weekly Shift**.
    -   **Shift At**: The time when the rotating shift switches. Set the value to **First Business Day 00:00**.
    ![Configure an all-day rotating shift](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4739872261/p264170.png)

-   Shift plan

    Operation Staff A, Operation Staff B, Operation Staff C, and Operation Staff D are on a rotating shift that switches turns on the first business day every week.

    ![Shift plan](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4739872261/p264171.png)


## Configure a rotating shift based on hours

For example, you can set a rotating shift for Operation Staff A, Operation Staff B, and Operation Staff C. The three staff are on a rotating shift that switches turns every 8 hours. For more information, see [Create an on-duty group](/intl.en-US/Alerting/Alerting (New)/User management/Create an on-duty group.md).

-   Configurations

    Set the required parameters:

    -   **Limit**: The valid time for the rotating shift. Set the value to **None**.
    -   **Object**: The on-duty staff. Set the value to **Operation Staff A \(testA\), Operation Staff B \(testB\), and Operation Staff C \(testC\)**.
    -   **Shift Type**: The shift type. Set the value to **Shift: Hourly Shift**.
    -   **Shift At**: The time when the rotating shift switches turns. Set the value to **Every 8 Hours 00 Minutes**.
    ![Configure a rotating shift among three staff](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5739872261/p264177.png)

-   Shift plan

    Operation Staff A, Operation Staff B, and Operation Staff C are on a rotating shift that switches turns every 8 hours.

    ![Shift plan](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5739872261/p264178.png)


## Configure a rotating shift in different time ranges

For example, you can set a rotating shift for Operation Staff A and Operation Staff B. Operation Staff A and Operation Staff B are on a rotating shift that switches turns every day. For more information, see [Create an on-duty group](/intl.en-US/Alerting/Alerting (New)/User management/Create an on-duty group.md).

-   Configurations

    Set the required parameters:

    -   **Limit**: The valid time for the rotating shift. Set the value to **Business Days** and **Business Hours**.
    -   **Object**: The on-duty staff. Set the value to **Operation Staff A \(testA\) and Operation Staff B \(testB\)**.
    -   **Shift Type**: The shift type. Set the value to **Shift: Daily Shift**.
    -   **Shift At**: The time when the rotating shift switches. Set the value to **Every 1 Days Start of Business Day**.
    ![Configure a rotating shift in different time ranges](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5739872261/p264179.png)

-   Shift plan

    Operation Staff A and Operation Staff B are on a rotating shift that switches turns every day.

    ![Shift plan](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5739872261/p264180.png)


## Configure an all-day substitute shift

When an on-duty staff is on leave, you can configure a substitute shift. For example, Operation Staff A and Operation Staff B are on a weekly rotating shift in April 2021. However, Operation Staff B is on leave from April 26 to April 30, 2021. In this case, you can configure a substitute shift in which Operation Staff C can replace Operation Staff B during this time range. For more information, see [Create an on-duty group](/intl.en-US/Alerting/Alerting (New)/User management/Create an on-duty group.md).

-   Configurations

    Set the required parameters:

    -   **Object**: The on-duty staff. Set the value to **Operation Staff B \(testB\)**.
    -   **Substitute**: The substitute. Set the value to **Operation Staff C \(testC\)**.
    -   **Limit**: The valid time range for the substitute shift. Set the value to **None**.
    ![A regular substitute](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5739872261/p264183.png)

-   Shift plan

    Operation Staff C is on duty from April 26 to April 30, 2021.

    ![Shift plan](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5739872261/p264184.png)


## Configure a substitute shift in different time ranges

For example, Operation Staff A and Operation Staff B are on a weekly rotating shift for April 2021. However, Operation Staff A is unavailable from 00:00 to 12:00 every Monday. In this case, you can configure a substitute shift in which Operation Staff C can replace Operation Staff A during this time range. For more information, see [Create an on-duty group](/intl.en-US/Alerting/Alerting (New)/User management/Create an on-duty group.md).

-   Configurations

    Set the required parameters:

    -   **Object**: The on-duty staff. Set the value to **Operation Staff A \(testA\)**.
    -   **Substitute**: The substitute. Set the value to **Operation Staff C \(testC\)**.
    -   **Limit**: The valid time range for the substitute shift. Set the value to **Every Week** and **Monday 00:00 - Monday 12:00**.
    ![Configure a substitute shift in different time ranges](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5739872261/p264308.png)

-   Shift plan

    Operation Staff C is on duty from 00:00 to 12:00 on April 5, April 12, and April 26, 2021.

    ![Shift plan](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5739872261/p264307.png)


## Multiple substitutes

If you configure multiple substitute shifts, the most recent substitute shift is valid. For example, Operation Staff A and Operation Staff B are on a weekly rotating shift for April 2021. However, Operation Staff B is on leave from April 26 to April 30, 2021. In this case, you can configure a substitute shift in which Operation Staff C can replace Operation Staff B during this time range. Then, you can set Operation Staff D as the substitute for Operation Staff C if Operation Staff C is on leave on April 30, 2021.

In the final shift plan, Operation Staff C is on duty from April 26 to April 29, 2021. Operation Staff D is on duty on April 30, 2021.

![Shift plan](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5739872261/p264314.png)

