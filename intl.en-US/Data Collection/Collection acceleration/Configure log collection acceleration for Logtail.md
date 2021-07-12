# Configure log collection acceleration for Logtail

This topic describes how to switch the collection mode of Logtail to Global Acceleration after you install Logtail.

The global acceleration feature is enabled. For more information, see [Enable the global acceleration feature](/intl.en-US/Data Collection/Collection acceleration/Enable the global acceleration feature.md).

## Configurations

-   If you enable the global acceleration feature before you install Logtail, select **Global Acceleration** when you install Logtail. For more information, see [Install Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md).
-   If you enable the global acceleration feature after you install Logtail, follow the instructions in this topic to switch the collection mode of Logtail to **Global Acceleration**.

## Procedure

1.  Stop Logtail.

    -   Linux:

        Run the `/etc/init.d/ilogtaild stop` command as the root user.

    -   Windows:
        1.  Choose **Start Menu** \> **Control Panel** \> **Administrative Tools** \> **Services**.
        2.  On the Services window, right-click the LogtailWorker service, and select **Stop**.
2.  Modify the Logtail startup configuration file ilogtail\_config.json.

    Replace the endpoint in the data\_server\_list parameter with `log-global.aliyuncs.com`. For more information, see [Startup configuration file \(ilogtail\_config.json\)](/intl.en-US/Data Collection/Logtail collection/Overview/Logtail configuration files and record files.md).

3.  Start Logtail.

    -   Linux:

        Run the `/etc/init.d/ilogtaild start` command as the root user.

    -   Windows:
        1.  Choose **Start Menu** \> **Control Panel** \> **Administrative Tools** \> **Services**.
        2.  On the Services window, right-click the LogtailWorker service, and select **Start**.

