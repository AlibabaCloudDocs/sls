# **Diagnose collection errors** {#task_njf_nkb_ry .task}

Errors may occur during log collection by Logtail, such as regular expression parsing failures, incorrect file paths, and traffic exceeding the shard service capability. Currently, the diagnosis function is provided in the Log Service console for diagnosing log collection errors.

1.  Log on to the [Log Service console](https://partners-intl.console.aliyun.com/#/sls), and then click the target project name. 
2.  On the Logstores page, click **Diagnose** in the Log Collection Mode column. 

    ![](images/5337_en-US.png "Diagnosis")

3.  Check log collection errors. In the displayed dialog box, view the list of log collection errors. To view error details, move your cursor to the **Error Type** column.

    For more information, see [Log collection error types](reseller.en-US/FAQ/Log collection/Log collection error types.md).

    ![](images/5338_en-US.png "View collection errors")

4.  Query log collection errors of a specified machine 

    To query all log collection errors occurred to a specific machine, enter the IP address of the machine in the search box on the query page. Logtail reports errors every 5 minutes.

    After fixing these errors and resuming business, check if the errors persist based on the timeframe. Historical error reports are still displayed before they expire. Ignore the historical error reports and only check whether new errors occurred after these historical errors are fixed.

    **Note:** To view all the complete log lines that are discarded because of parsing failure, you can log on to the machine to view the /usr/local/ilogtail/ilogtail.LOG file.


