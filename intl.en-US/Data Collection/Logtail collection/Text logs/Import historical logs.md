# Import historical logs

This topic describes how to import historical logs from a server to Log Service. By default, Logtail only collects incremental logs from servers. However, you can configure Logtail to collect historical logs.

-   Logtail V0.16.15 \(Linux\), Logtail V1.0.0.1 \(Windows\), or later is installed on the server. For more information, see [Install Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md) or [Install Logtail in Windows](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md).
-   A Logtail configuration file is created and applied to the server group to which the server belongs. For more information, see [Overview of text log collection](/intl.en-US/Data Collection/Logtail collection/Text logs/Overview.md).

    If the Logtail configuration file is used to only import historical files, you can specify a log collection path that does not exist.


Logtail collects logs based on the monitoring of log file modifications. Logtail can also load events from local files to collect logs. Logtail collects historical logs by loading local events.

You must import historical log files in the directory where Logtail is installed. The directory varies depending on different operating systems.

-   Linux: /usr/local/ilogtail
-   Windows:
    -   32-bit: C:\\Program Files\\Alibaba\\Logtail
    -   64-bit: C:\\Program Files \(x86\)\\Alibaba\\Logtail

**Note:**

-   The maximum latency that is allowed to import local events is 1 minute.
-   If a local event is loaded, Logtail sends the `LOAD_LOCAL_EVENT_ALARM` message to the server.
-   To import a large number of files, we recommend that you modify the Logtail startup parameters. You can set the threshold of the CPU usage to 2.0 or a larger value, and set the memory usage required by Logtail to 512 MB or a larger value. For more information, see [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md).

## Procedure

1.  Obtain the unique identifier of the Logtail configuration file.

    Open the user\_log\_config.json file in the directory where Logtail is installed. You can obtain the unique identifier of the Logtail configuration file.

    For example, to obtain the unique identifier of Logtail configuration file in a Linux server, run the following command:

    ```
    grep "##" /usr/local/ilogtail/user_log_config.json | awk '{print $1}'
                        "##1.0##log-config-test$multi"
                        "##1.0##log-config-test$ecs-test"
                        "##1.0##log-config-test$metric_system_test"
                        "##1.0##log-config-test$redis-status"
    ```

2.  Add a local event.

    1.  Create the local\_event.json file in the Logtail installation directory.

    2.  Add the local event in the JSON format to the local\_event.json file of the Logtail installation directory. The following example shows the format of the local event:

        ```
        [ 
          {
            "config" : "${your_config_unique_id}",
            "dir" : "${your_log_dir}",
            "name" : "${your_log_file_name}"
           },
          {
           ...
           }
           ...
        ]
        ```

        **Note:** To prevent Logtail from loading invalid JSON files, we recommend that you first save the configurations of the local event to a temporary file. Then, edit and copy the configurations to the local\_event.json file.

        |Parameter|Description|
        |:--------|:----------|
        |config|Enter the unique identifier that is obtained in [Step 1](#step_ls6_3m5_xfr). Example: \#\#1.0\#\#log-config-test$ecs-test.|
        |dir|The directory in which historical log files are saved. Example: /data/logs. **Note:** The directory cannot end with a forward slash \(`/`\). |
        |name|The name of the historical log file. A name with wildcards is supported. Example: access.log.2018-08-08 and access.log\*.|

        The following example describes how to configure a local event in Linux.

        ```
        $ cat /usr/local/ilogtail/local_event.json
        [
          {
            "config": "##1.0##log-config-test$ecs-test",
            "dir": "/data/log",
            "name": "access.log*"
          },
          {
            "config": "##1.0##log-config-test$tmp-test",
            "dir": "/tmp",
            "name": "access.log.2017-08-09"
          }
        ]                            
        ```


## FAQ

-   How do I check whether Logtail loads Logtail configurations?

    After you save the local\_event.json file, Logtail loads the configurations of the local event to the memory within 1 minute. Then, the content of the local\_event.json file is deleted.

    You can use the following methods to check whether the Logtail configurations are loaded.

    1.  If no content exists in the local\_event.json file, it indicates that the local event in the file is read by Logtail.
    2.  Check whether the ilogtail.LOG file in the Logtail installation directory contains the process local event parameter. If the content in the local\_event.json file is cleared but the process local event parameter does not exist, the content of the local\_event.json file may be invalid and filtered out.
-   Why am I unable to collect a log file after Logtail configurations are loaded?
    -   The Logtail configurations are invalid.
    -   The configurations of the local event in the local\_event.json file are invalid.
    -   The log file does not exist in the path that is specified in the Logtail configurations.
    -   The log file has been collected by Logtail.

