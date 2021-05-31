# How do I use the Logtail automatic diagnostic tool?

If an exception occurs during log collection, you can use the Logtail automatic diagnostic tool to check whether the exception exists on the Logtail client and quickly locate and resolve errors as instructed by the tool.

**Note:** This tool is currently available only to Linux servers.

## Preparations

1.  Download the script of the diagnostic tool.

    ```
    wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/checkingtool.sh -O checkingtool.sh
    ```

    **Note:** If you fail to download the tool, use the following URL and try again.

    ```
    wget http://logtail-corp.oss-cn-hangzhou-zmf.aliyuncs.com/linux64/checkingtool.sh -O checkingtool.sh
    ```

2.  Install the curl tool.

    The diagnostic tool uses curl to check the network connectivity. Ensure that the curl tool is installed on the server where Logtail is installed.


## Startup of the diagnostic tool

1.  Run the following command to run the diagnostic tool:

    ```
    chmod 744 ./checkingtool.sh
    ./checkingtool.sh
    sh checkingtool.sh
    ```

    The returned information is as follows:

    ```
    [Info]:     Logtail checking tool version : 0.3.0
    [Input]:  please choose which item you want to check :
                 1. MachineGroup heartbeat fail.
                 2. MachineGroup heartbeat is ok, but log files have not been collected.
         Item :
    ```

2.  Enter `1` or `2` as prompted. The script performs different checks based on your choice.

    where:

    -   `1` indicates the machine group heartbeat check. Select this option if the heartbeat status of the machine group is abnormal.
    -   `2` indicates the log collection check. Select this option if the heartbeat status of the machine group is normal but log files are not collected.
    After you select the required option, the diagnostic tool automatically performs the corresponding check.


## Diagnostic flowchart

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8044982951/p30599.png)

## Machine group heartbeat check

After you select the machine group heartbeat check, the diagnostic tool checks the following items:

1.  Check the basic environment.

    -   Whether Logtail is installed.

    -   Whether Logtail is running.

    -   Whether the SSL status is normal.

    -   Whether the network connection with Log Service is normal.

    ```
    [Info]:     Logtail checking tool version : 0.3.0
    [Input]:  please choose which item you want to check :
                    1. MachineGroup heartbeat fail.
                    2. MachineGroup heartbeat is ok, but log files have not been collected.
            Item :1
    [Info]:     Check logtail install files
    [Info]:     Install file: ilogtail_config.json exists.                          [ OK ]
    [Info]:     Install file: /etc/init.d/ilogtaild exists.                         [ OK ]
    [Info]:     Install file: ilogtail exists.                                      [ OK ]
    [Info]:     Bin file: /usr/local/ilogtail/ilogtail_0.14.2 exists.               [ OK ]
    [Info]:     Logtail version :                                                   [ OK ]
    [Info]:     Check logtail running status
    [Info]:     Logtail is runnings.                                                [ OK ]
    [Info]:     Check network status
    [Info]:     Logtail is using ip: 11.XX.XX.187
    [Info]:     Logtail is using UUID: 0DF18E97-0F2D-486F-B77F-XXXXXXXXXXXX
    [Info]:     Check SSL status
    [Info]:     SSL status OK.                                                      [ OK ]
    [Info]:     Check logtail config server
    [Info]:     config server address: http://config.sls.aliyun-inc.com
    [Info]:     Logtail config server OK
    ```

    If an `Error` message appears during the check, troubleshoot the error as prompted.

2.  Check whether you are the owner of the Elastic Compute Service \(ECS\) instance.

    After checking the basic environment, check whether your server is an ECS instance and whether you buy it with your Alibaba Cloud account.

    If this server is not an ECS instance or the account used to buy the server is different from that used to access Log Service, enter `y`. Otherwise, enter `N`.

    ```
    [Input]: Is your server non-Alibaba Cloud ECS or not belong to the same account with the current Project of Log Service ? (y/N)
    ```

    If you enter `y`, the diagnostic tool returns the AliUid information configured on the server where Logtail is installed. Check whether it includes your AliUid. If not, [configure an AliUid for an ECS instance under another Alibaba Cloud account or a server in an on-premises IDC](/intl.en-US/Data Collection/Logtail collection/Machine Group/Configure an account ID for a server.md).

    ```
    [Input]:  Is your server non-Alibaba Cloud ECS or not belong to the same account with the current Project of Log Service ? (y/N)y
    [Info]:     Check aliyun user id(s)
    [Info]:     aliyun user id : 126XXXXXXXXXX79 .                                 [ OK ]
    [Info]:     aliyun user id : 165XXXXXXXXXX50 .                                 [ OK ]
    [Info]:     aliyun user id : 189XXXXXXXXXX57 .                                 [ OK ]
    [Input]:  Is your project owner account ID is the above IDs ? (y/N)
    ```

3.  Check the region.

    Check whether the region of your project is the same as that selected during Logtail installation. If not, [reinstall Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md).

    ```
    [Input]: please make sure your project is in this region : { cn-hangzhou } (y/N) :
    ```

4.  Check the IP address configuration.

    Check whether the IP address of the server where Logtail is installed is configured in your machine group. If not, modify the IP address configuration. For more information, see [Create a machine group with IP addresses as its identifier](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create an IP address-based machine group.md).

    If your machine group is identified by a custom ID, check whether the custom ID configured on the server where Logtail is installed is the same as that configured in your machine group. If not, modify the custom ID configuration. For more information, see [Create a machine group with a custom ID as its identifier](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create a custom ID-based machine group.md).

    ```
    [Input]:  please make sure your machine group's ip is same with : { 11.XX.XX.187 } or your machine group's userdefined-id is in : { XX-XXXXX } (y/N) :
    ```


## Log collection check

After you select the log collection check, the diagnostic tool checks the following items:

1.  Check the IP address configuration.

    Check whether the IP address of the server where Logtail is installed is configured in your machine group, and whether the heartbeat status is normal. If not, [modify the machine group](/intl.en-US/Data Collection/Logtail collection/Machine Group/Manage machine groups.md).

    ```
    [Input]:  please make sure your machine group's ip is same with : { 11.XX.XX.187 } (y/N) :
    ```

2.  Check the application of the Logtail configuration.

    Check whether your Logtail configuration is applied to the machine group. For more information about how to check the Logtail configurations that are applied to a machine group, see [Manage machine groups](/intl.en-US/Data Collection/Logtail collection/Machine Group/Manage machine groups.md).

    ```
    [Input]: please make sure you have applied collection config to the machine group (y/N) :Y
    ```

3.  Check a specified log file.

    Enter the full path of the log file to be checked. Check whether this log file can be matched in the log path of your Logtail configuration.

    If the Logtail configuration is incorrect, modify the configuration and save it. Run the script again one minute later to check this item for a second time.

    ```
    [Input]:  please input your log file's full path (eg. /var/log/nginx/access.log) :/disk2/logs/access.log
    [Info]:     Check specific log file
    [Info]:     Check if specific log file [ /disk2/logs/access.log ] is included by user config.
    [Warning]:  Specific log file doesnt exist.                                     [ Warning ]
    [Info]:     Matched config found:                                               [ OK ]
    [Info]:     [Project] -> sls-zc-xxxxxx
    [Info]:     [Logstore] -> release-xxxxxxx
    [Info]:     [LogPath] -> /disk2/logs
    [Info]:     [FilePattern] -> *.log
    ```


## Persisted exception after all checks

If the Logtail client passes all the checks but still fails to collect logs, enter `y` for the last option of the script and press Enter.

Add the output of the script as an attachment and submit a ticket to Alibaba Cloud after-sales engineers.

```
[Input]: please make sure all the check items above have passed. If the problem persists, please copy all the outputs and submit a ticket in the ticket system. : (y/N)y
```

## Quick check

The quick check runs without confirmation. You can encapsulate and customize a quick check script.

**Note:** During quick check, the diagnostic tool returns the **AliUid** and **custom ID that identifies the machine group** configured on the server where Logtail is installed. If either of them does not exist, no alert is reported. If an AliUid or a custom ID that identifies the machine group is configured for the Logtail client, check whether the configuration returned by the diagnostic tool is the same as that you configured. If not, modify the configuration as required. For more information, see [Configure an AliUid for an ECS instance under another Alibaba Cloud account or a server in an on-premises IDC](/intl.en-US/Data Collection/Logtail collection/Machine Group/Configure an account ID for a server.md) and [Create a machine group with a custom ID as its identifier](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create a custom ID-based machine group.md).

Procedure

Run the script `./checkingtool.sh --logFile [LogFileFullPath]` to perform the check. If an exception is detected, proceed as instructed by the script.

**Note:** If the Logtail client passes the specified log file check and the Logtail running environment is normal, we recommend that you log on to the Alibaba Cloud console to view the exception logs of the relevant Logtail configuration. For more information, see [Diagnose collection errors](/intl.en-US/Data Collection/FAQ/How do I view log collection error messages in the log service console?.md).

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9044982951/p30600.png)

## Common Logtail collection errors

After running the Logtail automatic diagnostic tool, you can identify the causes of Logtail collection errors, and then use an appropriate solution to resolve the error accordingly. The following table describes the causes of common Logtail collection errors and their solutions.

|Error|Solution|
|:----|:-------|
|Installation files are incomplete.|Reinstall Logtail.|
|Logtail is not running.|Run the `/etc/init.d/ilogtaild start` command to start Logtail.|
|Multiple Logtail processes exist.|Run the `/etc/init.d/ilogtaild stop` command to stop Logtail, and then run the `/etc/init.d/ilogtaild start` command to restart Logtail.|
|Port 443 is disabled.|Configure the firewall to enable port 443.|
|The configuration server cannot be found.|Check whether Logtail is properly installed on a Linux server. If not, uninstall and then reinstall Logtail. For more information, see [Install Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md).|
|The user configuration does not exist.|Check whether the following operations are performed: 1.  A Logtail configuration is created in the console.
2.  The server is included in a machine group.
3.  The Logtail configuration is applied to the machine group. |
|The specified log file cannot be matched.|Check whether the Logtail configuration is correct.|
|The specified log file is matched more than once.|Logtail selects a Logtail configuration randomly if you match the specified log file multiple times. We recommend that you keep only one Logtail configuration that matches the specified log file.|

## Common parameters for the diagnostic tool

|Parameter|Description|
|:--------|:----------|
|`--help`|Views the help documentation.|
|`--logFile [LogFileFullPath]`|Checks whether Logtail collects logs from `LogFileFullPath` and checks the basic running environment of Logtail, such as the integrity of installation files, running status, AliUid, and network connectivity.|
|`--logFileOnly [LogFileFullPath]`|Only checks whether Logtail collects logs from `LogFileFullPath`.|
|`--envOnly`|Only checks the running environment of Logtail.|

