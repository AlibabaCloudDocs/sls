# Modify the global default calendar

Log Service provides the global default calendar. You can use the global default calendar to synchronize business days and holidays in different time zones and countries. You can modify the default time zone, business days and holidays of a country on the calendar. The topic describes how to modify the global default calendar.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Go to the Global Default Calendar tab.

    1.  In the Projects section, click a destination project.

    2.  In the left-side navigation pane, click ![Alerts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8943972261/p110115.png).

    3.  Click **Open Alert Center**.

    4.  Click **Global Default Calendar** from the **Alert Management** drop-down list.

3.  Click **Calendar Settings** and set the required parameters.

    The following information describes the parameters:

    -   **Time Zone**: Select a time zone based on your region.
    -   **Business Days**: Select the business days.
    -   **Non-Business Days**: Select the non-business days.
        -   **Ignore**: Indicate that non-business days are not set based on the calendar of a specified country.
        -   **China \(update in real time\)**: Synchronize non-business days based on the calendar of China.
        -   **USA**: Synchronize non-business days based on the calendar of the United States.
    -   **Business Hours**: Select the business hours.
4.  Set business days and holidays for specific dates and click **Save**.

    The following information describes the parameters:

    -   **Date**: Select a date range for business days or holidays.
    -   **Type**: The date type. You can set a specific date to a business day or holiday.
        -   **Business Days**
        -   **Holidays**
    -   **Color**: The display color for business days or holidays.
    -   **Period**: The valid time range for business days or holidays.

        **Note:**

        -   We recommend that you select All Day if you set a business day to a holiday.
        -   We recommend that you select a time range if you set a holiday to a business day.

