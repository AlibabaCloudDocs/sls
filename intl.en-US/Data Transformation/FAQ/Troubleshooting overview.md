# Troubleshooting overview

After a data transformation task is started, the data transformation engine sends the transformation results to the destination Logstores based on routing rules. This topic provides troubleshooting methods that you can use if a data transformation task fails. For example, no log is generated in the destination Logstores or a long delay occurs during the data transformation process.

## Analyze errors

If an error occurs, you can first locate the error in the data transformation process.

Each data transformation task consists of four steps based on [Data transformation basics](/intl.en-US/Data Transformation/Data transformation basics.md). The following figure shows the steps.

![Data transformation process](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8453749951/p59263.png)

Errors may occur in each of these steps. The causes, impacts, and troubleshooting methods vary depending on the steps.

-   Start the data transformation engine.

    -   Errors may occur in this step if domain-specific language \(DSL\) rules fail the security check in the data transformation engine.
    -   If an error occurs in this step, the data transformation task stops. You must modify the DSL rules and restart the data transformation task. If the retry succeeds, the transformation task continues and no data loss or redundancy occurs.
    For more information about the troubleshooting methods in this step, see [Startup errors of the data transformation engine](/intl.en-US/Data Transformation/FAQ/Startup errors of the data transformation engine.md).

-   Read data from the source Logstore.

    -   Errors may occur in this step due to a failure to access the source Logstore. The failure may be caused by invalid configurations of the source Logstore, network errors, or an update of the source Logstore.
    -   If an error occurs in this step, the data transformation task keeps retrying until data reading succeeds or is manually stopped. If the retry succeeds, the transformation task continues and no data loss occurs.
    -   If an error occurs after some data is read, the data transformation task saves the checkpoint and keeps retrying. After the retry succeeds, it continues to read data from the checkpoint and no data loss or redundancy occurs. If the task is stopped during the retry process, no data loss or redundancy occurs.
    For more information about the troubleshooting methods in this step, see [Errors in reading data from the source Logstore](/intl.en-US/Data Transformation/FAQ/Errors in reading data from the source Logstore.md).

-   Transform log events.

    -   Errors may occur in this step if the transformation rules do not apply to all or some log events during the data transformation process.
    -   During the transformation process, log events that conflict with the transformation rules cause errors. These errors are divided into WARNING and ERROR levels. The error levels are identified by the logging.levelname field.
        -   For ERROR-level errors, the relevant log events are dropped. The transformation result does not contain these log events.
        -   For WARNING-level errors, data is not transformed in the current DSL. For example, if the log events do not match the specified regular expression, the next step is performed.
    For more information about the troubleshooting methods in this step, see [How can I fix errors related to data transformation rules?](/intl.en-US/Data Transformation/FAQ/How can I fix errors related to data transformation rules?.md).

-   Export the transformation results to the destination Logstores.
    -   Errors may occur in this step due to a failure to access the destination Logstore. The failure may be caused by invalid configurations of the destination Logstore, network errors, or an update of the destination Logstore.
    -   If an error occurs in this step, the data transformation task keeps retrying until data reading succeeds or is manually stopped. If the retry succeeds, the transformation task continues and no data loss occurs.
    -   If an error occurs after some data is exported, the transformation task keeps retrying. For example, assume that two destination Logstores are specified. Data export from one Logstore succeeds, but data from the other Logstore fails. In this case, the transformation task saves the breakpoint and keeps retrying. After the retry succeeds, no data loss or redundancy occurs. If the data transformation task is stopped and then restarted when the error occurs, the data transformation task continues from the breakpoint. In this case, no data is lost, but data redundancy may occur.

## Troubleshoot common errors

1.  Check whether data has been written to a destination Logstore.

    To check whether data has been written to a destination Logstore recently, you can view the data on the Consumption Preview page of the destination Logstore.

    ![Consumption preview](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8453749951/p59268.png)

    **Note:** The consumption preview data may be inaccurate due to the following reasons:

    -   In Log Service, logs are transformed based on the time when logs are received. In scenarios where historical logs are being transformed, the time when the logs are written may not be within the time range specified for querying historical logs.
    -   You can query log data based on the indexes of historical logs. However, a delay of several minutes may occur. If historical logs are being written in a data transformation task, the log data may not be immediately queried.
2.  View the status of a data transformation task.
    -   Check whether the current task is started. For more information, see [View task status](/intl.en-US/Data Transformation/Manage a data transformation task.md). Tasks with a fixed time range automatically stop at the specified end time.
    -   Check whether the consumer group in the current task is enabled and updated.

        ![Consumption status](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9453749951/p59270.png)

    -   Check whether errors occur. For more information, see [View error logs](#section_739_ltj_adz). If an error occurs, identify the error cause and fix the error. For more information, see [Analyze errors](#section_27d_0q8_c2q).
3.  Check whether data is generated in the source Logstore.

    Check whether logs exist in the source Logstore within the time range of the current data transformation task.

    -   If the end time of the time range is not specified, check whether new logs are generated in the source Logstore. If no new logs are generated and no historical logs exist within the specified time range, the data transformation task cannot be performed.
    -   If you select a historical time range, check whether logs exist in the source Logstore within the time range.
    Click [Modify a rule](/intl.en-US/Data Transformation/Manage a data transformation task.md) of the data transformation task, select a time range, and then check whether raw logs exist in the specified time range.

4.  Check whether the transformation rules are valid.

    Check whether code errors exist in the transformation rule. Example:

    -   The log time is modified. As a result, no logs can be found in the specified time range.
    -   Logs are dropped based on the transformation rule under specific conditions.

        For example, assume that the transformation rule contains the following code. If a log does not contain the `name` field or the value of this field is null, the log is dropped. The pre-logic in the code is used to construct the `name` field. If the `name` field is not constructed due to a pre-logic issue, no log is generated.

        ```
        # .... The pre-logic.
        #  .... Build the name field...
        
        e_keep(e_search('name: "?"'))
        ```

    -   If the task pulls data from a third party for data enrichment, check whether the data size of the third party is too large. If so, the data transformation task cannot immediately start to consume data and may stay in the initializing state for a long time. Example:

        ```
        e_dict_map(res_rds_mysql(..database="userinfo", table="user"), "username", ["city", "school", "age"])
        ```

    Click [Modify a rule](/intl.en-US/Data Transformation/Manage a data transformation task.md) of the data transformation task, select a time range, and then click Preview Data to view the result.

    If logs are queried, comment out the specific statement that causes the error and preview the result again.

5.  Check whether the number of shards is as expected.

    If data transformation is too slow, check whether the source and destination Logstores meet your performance expectations. We recommend that you adjust the number of shards in the source or destination Logstore.


## View error logs

You can view error logs by using the following methods:

-   View error logs in the `internal-etl-log` Logstore.

    Logs generated by a data transformation task are stored in the `internal-etl-log` Logstore. This Logstore is automatically generated after Log Service performs the data transformation task.

    -   The `internal-etl-log` Logstore is a dedicated Logstore that is provided free of charge. You cannot modify its configurations or write data to it.
    -   In the `internal-etl-log` Logstore, the `__topic__` field of each log event indicates the status of the data transformation task. You can check whether an error occurs in the data transformation task based on this field.
    -   You can check the `message` and `reason` fields of each log event to view the detailed error information. The following figure shows the fields.

        ![Error information in the Logstore](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9453749951/p59276.png)

-   View error logs on the dashboard.

    Click a data transformation task and check the dashboard in the **Status** section on the **Data Transformation Overview** page.

    The error information is shown in the `reason` column in the Exception detail section.

    ![Error information on the dashboard](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9453749951/p59277.png)

-   View error logs in the console.

    Error logs in the preview phase are displayed in the Log Service console. In the preview phase, Log Service simulates the operations specified by the transformation rule and check whether the data is transformed as expected. No changes are made to the source or destination Logstore. Therefore, errors that occur in the preview phase do not affect the source log events.


## Preview limits

Compared with real data transformation tasks, data transformation in the preview phase has the following limits:

-   No error occurs even if a RAM user uses an invalid AccessKey pair to access the source Logstore.

    In the preview phase, no consumer groups are created to consume data. Therefore, Log Service does not check the permissions of consumer groups.

-   No error occurs even if a name error of the destination Logstore occurs in the transformation rule.

    Data is not written to the destination Logstore in the preview phase. Therefore, the system does not check whether the destination Logstore is correctly configured in the transformation rule.

-   No error occurs even if configuration errors of the destination Logstore exist.
    -   The configuration errors include invalid destination project, destination Logstore, and AccessKey pair configurations.
    -   Data is not written to the destination Logstore in the preview phase. Therefore, the system does not check whether the configurations of the destination Logstore are correct.
-   Only partial data is pulled in the preview phase.
    -   By default, only 1,000 data records are pulled from the source Logstore for data transformation during preview.
    -   If no transformation result is generated after the first 1,000 data records are transformed, Log Service continues to pull data for 5 minutes until a transformation result is generated.

