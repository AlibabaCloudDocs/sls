# Create users and user groups

If you use SMS messages, emails, or voice calls to send alert notifications, you must specify users or user groups to whom the alert notifications are sent. This topic describes how to create users and user groups.

## Step 1: Create users

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Go to the User Management page.

    1.  In the Projects section, click the name of the required project.

    2.  In the left-side navigation pane, click the ![Alerts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8943972261/p110115.png) icon.

    3.  Click **Open Alert Center** and choose **Alert Management** \> **User Management**.

3.  Create users.

    -   Create a single user
        1.  Click **Create**.
        2.  In the Create User dialog box, set the following parameters and click **OK**.

            |Parameter|Description|
            |---------|-----------|
            |**ID**|The unique identifier of the user. Take note of the following naming conventions:            -   The name must start with a letter.
            -   The ID must be 5 to 60 characters in length.
            -   The ID can contain letters, digits, underscores \(\_\), hyphens \(-\), and periods \(.\). |
            |**Username**|The name of the user. Take note of the following naming conventions:            -   The ID must be 1 to 20 characters in length.
            -   The following special characters are not supported:

"\\$\|~?&<\>\{\}\`' |
            |**Phone Number**|The mobile phone number of the user. Add a country code before each mobile phone number, for example, 86-1381111\*\*\*\*\*.             -   If you turn on the **Receive Text Message** switch, Log Service sends SMS notifications to users based on the alert settings.
            -   If you turn on the **Receive Phone Call** switch, Log Service sends voice call notifications to users based on the alert settings. |
            |**Email**|The email address of the user. You can click **Create** to add multiple email addresses. Take note of the following naming conventions:            -   Format: username@domain name.organization
            -   The username must be 1 to 64 characters in length and can contain only digits, letters, underscores \(\_\), hyphens \(-\), and periods \(.\).
            -   The domain name must be 1 to 64 characters in length and can contain only digits, letters, underscores \(\_\), and hyphens \(-\).
**Note:** You must specify at least a phone number or an email address. |
            |**Enabled**|If you turn on the **Enabled** switch, Log Service sends alert notifications to users.|

    -   Create multiple users at a time
        1.  Click **Add Users**.
        2.  On the Add Users tab of the Create User dialog box, enter the user information, and click **OK**.

            You can enter the user information by using the following format:

            ```
            #ID, Username, Enabled, Country code-phone number, Receive text message, Receive phone call, Email 1, Email 2, Email 3
            bob,wangxiansheng,true,86-1381111*****,true,true,alice@example.com,bob@example.com
            ```

            **Note:**

            -   Separate multiple fields with commas \(,\).
            -   Specify each field value based on the requirements that are provided when you create a single user.
            -   You can refer to the examples on the Sample tab.

## Step 2: Create user groups

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Go to the User Groups page.

    1.  In the Projects section, click the name of the required project.

    2.  In the left-side navigation pane, click the ![Alerts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8943972261/p110115.png) icon.

    3.  Click **Open Alert Center** and choose **Alert Management** \> **User Group Management**.

3.  Create user groups.

    -   Create a single user group
        1.  Click **Create**.
        2.  In the Add User Group dialog box, set the following parameters and click **OK**.

            |Parameter|Description|
            |---------|-----------|
            |**ID**|The unique identifier of the user group. Take note of the following naming conventions:            -   The ID must start with a letter.
            -   The ID must be 5 to 60 characters in length.
            -   The ID can contain letters, digits, underscores \(\_\), hyphens \(-\), and periods \(.\). |
            |**Group Name**|The name of the user group. Take note of the following naming conventions:            -   The name must be 1 to 20 characters in length.
            -   The following special characters are not supported:

"\\$\|~?&<\>\{\}\`' |
            |**Available Members**|The users that you have created.|
            |**Selected Members**|The users who are added to the user group.|
            |**Enabled**|If you turn on the **Enabled** switch, Log Service sends alert notifications to the user group.|

    -   Create multiple user groups at a time
        1.  Click **Add User Group**.
        2.  On the Add User Group tab of the Add User Group dialog box, enter the user information, and click **OK**.

            You can enter the user group information by using the following format:

            ```
            #ID, Group name, Enabled, Members
            sls_dev,O&M and development group of Log Service,false, Wangxianshen;Alan;Alice;Claire
            ```

            **Note:**

            -   Separate multiple fields with commas \(,\).
            -   Specify each field value based on the requirements that are provided when you create a single user group.
            -   Separate multiple member IDs with semicolons \(;\).
            -   You can refer to the examples on the Sample tab.

