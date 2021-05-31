# Do I need to update Logtail settings after the network type is changed?

After the network type is changed from classic network to VPC, you need to restart Logtail and update the settings of the Logtail machine group.

After Logtail is installed, if your ECS network type is changed from classic network to VPC, you need to update Logtail settings by performing the following steps:

1.  Restart Logtail as the admin user.
    -   **In Linux**:

        ```
        sudo /etc/init.d/ilogtaild stop
        sudo /etc/init.d/ilogtaild start
        ```

    -   **In Windows**:

        In **Control Panel**, choose **System and Security** \> **Administrative Tools**. Open the **Services** program, locate the LogtailWorker file, and then right-click the file and click **Restart** in the shortcut menu.

2.  Update the machine group settings.
    -   **Custom identity**

        If a custom identity is configured for the machine group, a VPC is accessible without the need to manually update the machine group settings.

    -   **IP address**

        If the IP address of the ECS server is configured for the machine group, you need to replace the machine group IP address to the one that is obtained after the Logtail restart, namely, the ip field in the app\_info.json file.

        The app\_info.json file is stored in:

        -   /usr/local/ilogtail/app\_info.json in Linux
        -   C:\\Program Files \(x86\)\\Alibaba\\Logtail\\app\_info.json in Windows x64
        -   C:\\Program Files\\Alibaba\\Logtail\\app\_info.json in Windows x32

