# Troubleshoot collection errors

If the log collection fails or the collection status is abnormal when you use Logtail, follow these steps to troubleshoot the errors.

1.  Check whether the Logtail heartbeat in the machine group is normal

    Log on to the Log Service console and click Machine Status to view the status of the machine group.  For more information, see Manage a machine group. If the heartbeat status is normal, move to the next step. 

    If the heartbeat status is fail, see [Logtail heartbeat error for troubleshooting](https://help.aliyun.com/document_detail/48851.html?spm=a2c4g.11186623.2.5.I1H7Wj).

    If the heartbeat status is fail, see [Logtail heartbeat error for troubleshooting](https://www.alibabacloud.com/help/doc-detail/48851.htm?spm=a2c63.p38356.a3.3.a2946d19Z3wt8W).

2.  Check whether the collection configuration is created and applied to the machine group

    After you confirm that the Logtail client status is normal, check the following configurations. 

    1.  Check whether Logtail configuration is created

        For more information, see Logtail configuration.  Make sure that the log monitoring directory and the log file name match with the files on the machine. The directory does not support fuzzy match and must be set to an absolute path, while the log file name supports fuzzy match.

    2.  Check whether Logtail configuration is applied to the machine group

        See [Manage configurations](/intl.en-US/Data Collection/Logtail collection/Machine Group/Manage machine groups.md) in **Manage a machine group**. Check if the target configuration is applied to the machine group.

3.  Check for collection errors

    If Logtail is properly configured, check whether new data is generated in real time in the log file.  Logtail collects incremental data only, it does not read inventory files if the files are not updated.  If the log file is updated but the updates cannot be queried in Log Service, diagnose the problem in the following ways:

    -   **Diagnose collection errors**

        See [Diagnose collection errors]() to handle the errors according to the error type reported by Logtail.

    -   **View Logtail logs**

        Client logs include key INFO logs and all the WARNING and ERROR logs.  To see complete and real-time errors, view the client logs in the following paths:

        -   Linux: /usr/local/ilogtail/ilogtail.LOG
        -   Linux: /usr/local/ilogtail/logtail\_plugin.LOG \(logs of input  sources such as HTTP, MySQL binlog, and MySQL query results\)
        -   Windows x64: C:\\Program Files Program Files \(x86\)\\Alibaba\\Logtail\\logtail\_\*.log
        -   Windows x32:  C:\\Program Files\\Alibaba\\Logtail\\logtail\_\*.log
    -   **Usage exceeds the limit**

        -   To collect large volumes of logs, files, or data, you can modify the Logtail startup parameters for higher log collection throughput. For more information, see  [Configure the startup parameters of Logtail](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md).
    If the problem persists, open a ticket to contact Log Service engineers and attach the key information collected during troubleshooting to the ticket.


