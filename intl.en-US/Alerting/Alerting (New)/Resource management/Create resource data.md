# Create resource data

Resource data is the external data that is associated with specific alert monitoring rules, such as the blacklists and whitelists of monitored objects. You can create, modify, and delete these external data. This topic describes how to create resource data.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Go to the Resource Data page.

    1.  In the Projects section, click the name of the required project.

    2.  In the left-side navigation pane, click the ![Alerts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0356352261/p110115.png) icon.

    3.  Click **Open Alert Center**.

    4.  From the **Alert Management** drop-down list, select **Resource Data**.

3.  Click **Create** to configure resource data.

    |Parameter|Description|
    |---------|-----------|
    |**ID**|The unique ID of the resource. Take note of the following naming conventions:    -   The ID must start with a letter.
    -   The ID must be 3 to 127 characters in length.
    -   The ID can contain letters, digits, underscores \(\_\), hyphens \(-\), and periods \(.\). |
    |**Name**|The name of the resource. Take note of the following naming conventions:    -   The name cannot exceed 40 characters in length.
    -   The following special characters are not supported:

\\$\|~?&<\>\{\}\`'" |
    |**Field**|The fields of the resource. You can click ![jia](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5119872261/p249352.png) to add a field, description, and field type. Field types support strings, floating-point numbers, and integers. Examples: ip\_black\_list, IP address blacklist, and string.|

4.  Click **OK**.


