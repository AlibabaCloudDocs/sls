# Import data from OSS to Log Service

This topic describes how to import data from Object Storage Service \(OSS\) to Log Service. After you store logs in OSS, you can import these logs as OSS data to Log Service. Then, you can query, analyze, and transform the data in Log Service. Log Service can only import OSS objects that are less than 5 GB in size. The size of a compressed object is calculated based on the compressed size.

-   Logs are uploaded to an OSS bucket. For more information, see [Upload objects](/intl.en-US/Quick Start/OSS console/Upload objects.md).
-   A project and a Logstore are created. For more information, see [Create a project and a Logstore](/intl.en-US/Quick Start/Quick Start.md).
-   Log Service is authorized to use the AliyunLogImportOSSRole role to access your OSS resources. For more information, see [Cloud Resource Access Authorization](https://ram.console.aliyun.com/#/role/authorize?request=%7B%22Requests%22:%20%7B%22request1%22:%20%7B%22RoleName%22:%20%22AliyunLogImportOSSRole%22,%20%22TemplateId%22:%20%22ImportOSSRole%22%7D%7D,%20%22ReturnUrl%22:%20%22https:%2F%2Fsls.console.aliyun.com%2F%22,%20%22Service%22:%20%22Log%22%7D).

    If you use a RAM user, you must attach the PassRole policy to the RAM user. The following script shows an example. For more information, see [Create a custom policy](/intl.en-US/Policy Management/Custom policies/Create a custom policy.md) and [Grant permissions to a RAM user](/intl.en-US/RAM User Management/Grant permissions to a RAM user.md).

    ```
    {
       "Statement": [
        {
          "Effect": "Allow",
          "Action": "ram:PassRole",
          "Resource": "acs:ram:*:*:role/aliyunlogimportossrole"
        }
      ],
      "Version": "1"
    }        
    ```


## Create a data import configuration

**Note:** Normal objects support object-level marks. After the content of a normal object is modified, Log Service imports the modified object. Append objects support row-level marks. After new content is appended to an append object, Log Service reads and imports the appended content.

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, select **OSS - Object Storage Service**.

3.  In the Specify Logstore step, select the project and Logstore, and then click **Next**.

    You can also click **Create Now** to create a project and a Logstore. For more information, see [Step 1: Create a project and a Logstore](/intl.en-US/Quick Start/Quick Start.md).

4.  In the Configure Import Settings step, set the required parameters.

    1.  On the Specify Data Source tab, set the required parameters. The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |Config Name|The name of the data import configuration.|
        |OSS Region|The region where the OSS bucket resides.If the OSS bucket and the Log Service project reside in the same region, no Internet traffic is generated and the transfer speed is fast. |
        |Bucket|The bucket where the OSS objects reside.|
        |Folder Prefix|The prefix of the folder where the OSS objects to be imported reside. This prefix is used to identify the folder of the OSS objects. For example, if all the OSS objects to be imported are in the csv/ directory, you can set the prefix to csv/.If you do not specify the folder prefix, all objects in the OSS bucket are traversed. |
        |Regular Expression Filter|The regular expression that is used to filter OSS objects. Only the objects whose names match the regular expression are imported. The names include the paths of the objects. Default value: null. This value indicates that objects are not filtered.For example, if an OSS object is named testdata/csv/bill.csv, you can set the regular expression to **\(testdata/csv/\)\(. \*\)**. |
        |Data Format|The format into which the OSS objects are parsed. Valid values:         -   CSV: a delimited text file that uses commas \(,\) to separate values. You can specify the first line of the file as the line of all field names, or customize field names. Field values in all lines except the line of field names is parsed into the values of log fields.
        -   JSON: OSS objects are read line by line. Each line is parsed as a JSON object. The fields in JSON objects are log fields.
        -   Parquet: the Parquet format. By default, OSS objects are parsed into the Parquet format. You cannot preview Parquet-formatted data.
        -   Single-line Text: Each line of OSS objects is parsed into a log entry.
        -   Multi-line Text: Multiple lines of an OSS object are parsed into a log entry. You can specify a regular expression to match the first line or the last line of a log entry. |
        |Compression Format|The compression format of the OSS objects to be imported. Log Service decompresses the OSS objects based on the compression format and reads data from the objects.|
        |Encoding Format|The encoding format of the OSS objects to be imported.|
        |Restore Archived Files|If the OSS objects are archived objects, they must be restored. Otherwise, Log Service cannot read the objects. If you turn on this switch, archived objects are automatically restored.|

    2.  Click **Preview** to preview the objects.

    3.  Click **Next**.

    4.  On the Specify Data Type tab, set the required parameters. The following table describes the parameters.

        -   Related parameters of log time

            |Parameter|Description|
            |---------|-----------|
            |Use System Time|The following list describes the Use System Time parameter:            -   If you turn on the **Use System Time** switch, the time field of a parsed log entry is the system time when the log entry is imported.
            -   If you turn off the **Use System Time** switch, you must configure the time field and format.
**Note:** We recommend that you turn on the Use System Time switch. You can configure an index for the time field and use the index for log queries. If you import data generated earlier than the current time minus the data retention period of the Logstore, the data cannot be queried in the Log Service console. For example, if the data retention period is seven days and you import data that was generated seven days ago, no results can be found in the Log Service console. |
            |Regex to Extract Time|If you select Single-line Text or Multi-line Text for Data Format, you must specify a regular expression to extract the log time after you turn off the **Use System Time** switch.For example, if the sample log entry in a log file is 127.0.0.1 - - \[10/Sep/2018:12:36:49 0800\] "GET /index.html HTTP/1.1", you can set **Regex to Extract Time** to **\[0-9\]\{0,2\}\\/\[0-9 a-zA-Z\]+\\/\[0-9:,\]+**. |
            |Time Field|If you select CSV, Single-line JSON, or Parquet for Data Format, you must specify a time field after you turn off the **Use System Time** switch.For example, the following figure shows the details of a CVS file. You can set **Time Field** to **time\_local**.

![Extract time fields](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9734201161/p195894.png) |
            |Time Format|If you turn off the **Use System Time** switch, you must specify a time format by using the Java SimpleDateFormat class. The time format is used to parse time fields. For more information, see [Class SimpleDateFormat](https://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html). **Note:** The Java SimpleDateFormat class does not support UNIX timestamps. If you want to use UNIX timestamps, you can set the time format to epoch. |
            |Time Zone|If you turn off the **Use System Time** switch, you must specify a time zone to parse the time zone of the log time. This parameter is invalid if the time zone information already exists in the log format.|

        -   Other parameters
            -   Unique parameters for CSV files

                |Parameter|Description|
                |---------|-----------|
                |Delimiter|The delimiter that is used to delimit CSV files. By default, the delimiter is a comma \(,\).|
                |Quote|If a field contains a delimiter, the field must be enclosed in a quote. By default, the quote is double quotation marks \("\).|
                |Escape Character|The escape character that is used in CSV files. By default, the escape character is a backslash \(\\\).|
                |Max Lines for Multiline Logging|The maximum number of lines that you specify for a log entry. Default value: 1.|
                |First Line as Field Name|If you turn on the **First Line as Field Name** switch, the first line in a CSV file records field names. For example, the following figure shows the first line that is extracted to be the field name.![First line](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9734201161/p196139.png) |
                |Custom Field List|If you turn off the **First Line as Field Name** switch, you can customize field names based on your business requirements. Multiple field names are separated by commas \(,\).|
                |Lines to Skip|The number of lines that are skipped. For example, if you set the value to 1, the first line of a CVS file is skipped and the log collection starts from the second line.|

            -   Unique parameters for multi-line text logs

                |Parameter|Description|
                |---------|-----------|
                |Position to Match with Regex|The following list describes the Position to Match with Regex parameter:                -   If you select **Regex to Match First Line Only**, the regular expression that you specify is used to match the first line of a log entry. The unmatched lines are collected as part of the log entry until the specified maximum number of lines of the log entry is reached.
                -   If you select **Regex to Match Last Line Only**, the regular expression that you specify is used to match the last line of a log entry. The unmatched lines are collected as part of the next log entry until the specified maximum number of lines of the log entry is reached. |
                |Regular Expression|Specify a regular expression based on the log content. For more information, see [How do I modify a regular expression?]().|
                |Max Lines|The maximum number of lines in a log entry.|

    5.  After you set the required parameters, click **Test**.

    6.  After the test succeeds, click **Next**.

    7.  On the Specify Scheduling Interval tab, set the required parameters. The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |Import Interval|The interval at which the OSS objects are imported to Log Service.|
        |Import Now|If you turn on the **Import Now** switch, the OSS objects are immediately imported.|

    8.  Click **Next**.

5.  Preview the data and click **Next**.

    By default, Log Service enables the Full Text Index to query and analyze logs. For more information, see [Enable and configure the indexing feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

    **Note:**

    -   The index is applicable only to the log data that is newly written.
    -   To query and analyze logs, you must enable the Full Text Index or Field Search. If you enable both of them, the settings of Field Search prevail.
6.  Click **Next**.


## View the data import configuration

After you create a data import configuration, you can view the configuration details and related statistical report in the Log Service console.

1.  In the Projects section, click the destination project.

2.  In the left-side navigation pane, choose **Log Storage** \> **Logstores**. Find the Logstore to which the data import configuration belongs, choose **Data Import** \> **Data Import**, and then click the name of the data import configuration.

3.  On the Import Configuration Overview page, view the configuration details and statistical report.

    ![Import configuration overview](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9734201161/p74200.png)


## Related operations

On the Import Configuration Overview page, you can perform the following operations:

-   Modify the data import configuration.

    You can click **Modify Settings** to modify the data import configuration. For more information, see [Configure data import](#step_pz7_0v7_4pg).

-   Delete the data import configuration.

    You can click **Delete Configuration** to delete the data import configuration.

    **Warning:** After the data import configuration is deleted, it cannot be recovered.


## Common issues

|Issue|Cause|Solution|
|-----|-----|--------|
|You cannot select an OSS bucket when you create a data import configuration.|The AliyunLogImportOSSRole role is not authorized.|See the "Prerequisites" section in this topic to complete the authorization.|
|Data cannot be imported.|The object size exceeds 5 GB.|Reduce the size of each OSS object.|
|After the data is imported, you cannot query or analyze the data.|No indexes are configured or the indexes failed to take effect.|To make indexes take effect after data is imported, we recommend that you create indexes for the specified Logstore in advance. For more information, see [Enable and configure the indexing feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md). If the indexes have failed to take effect, you can reindex logs for the Logstore. For more information, see [Reindex logs for a Logstore](/intl.en-US/Index and query/Query/Reindex logs for a Logstore.md).|
|Archived objects cannot be imported.|The Restore Archived Files switch is not turned on.|-   Method 1: Modify the data import configuration and turn on the **Restore Archived Files** switch.
-   Method 2: Recreate a data import configuration and turn on the **Restore Archived Files** switch. |
|The **Regular Expression Filter** parameter is specified, but no data is collected.|-   The regular expression is incorrectly specified.
-   The number of objects is too large. The objects that match the specified regular expression cannot be traversed within a specified period.

|Reset the **Regular Expression Filter** parameter. If data still cannot be collected, the probable cause is that the number of objects is too large. You must specify a more specific directory to reduce the number of objects to be traversed.|
|The logs are imported, but no data can be found in the Log Service console.|The log time is not included in the data retention period of the Logstore. Expired data is deleted.|Check the query time range and the retention period of data in the Logstore.|
|The extracted log time is used to query data, but no data is queried at this time.|The time format is incorrectly specified.|Check whether the time format is Java SimpleDateFormat. For more information, see [Class SimpleDateFormat](https://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html).|
|Multi-line text logs are incorrectly parsed.|The regular expression that is used to match the first line or the last line of a log entry is incorrectly specified.|Check whether the regular expression that is used to match the first line or the last line of a log entry is correctly specified.|
|The import speed suddenly slows down.|-   No sufficient data can be imported.
-   Log Service requires a large amount of processing time to traverse OSS objects because the number of the OSS objects is too large.

|1.  Check whether the specified OSS bucket has sufficient data to be imported.
2.  Check whether Log Service requires a large amount of processing time to traverse OSS objects because the number of the OSS objects is too large. If the number of OSS objects is too large, you can set the **Folder Prefix** and **Regular Expression Filter** parameters to reduce the number of objects in a data import configuration. You can also migrate the imported objects to another directory or bucket in the OSS console. |

## Appendix

-   Sample time formats

    |Time format|Parsing syntax|Parsed value \(Unit: seconds\)|
    |-----------|--------------|------------------------------|
    |2020-05-02 17:30:30|yyyy-MM-dd HH:mm:ss|1588411830|
    |2020-05-02 17:30:30:123|yyyy-MM-dd HH:mm:ss:SSS|1588411830|
    |2020-05-02 17:30|yyyy-MM-dd HH:mm|1588411800|
    |2020-05-02 17|yyyy-MM-dd HH|1588410000|
    |20-05-02 17:30:30|yy-MM-dd HH:mm:ss|1588411830|
    |2020-05-02T17:30:30V|yyyy-MM-dd'T'HH:mm:ss'V'|1588411830|
    |Sat May 02 17:30:30 CST 2020|EEE MMM dd HH:mm:ss zzz yyyy|1588411830|

-   Time parsing syntax

    |Character|Description|Example|
    |---------|-----------|-------|
    |G|Era indicator|AD|
    |y|4-digit year number|2001|
    |M|Month name or number|July or 07|
    |d|Day of the month as a number|10|
    |h|Hour \(12-hour clock, 1 to 12\)|12|
    |H|Hour \(24-hour clock, 0 to 23\)|22|
    |m|Minute|30|
    |s|Second|55|
    |S|Millisecond|234|
    |E|Day of the week \(Sunday to Saturday\)|Tuesday|
    |D|Day of the year|360|
    |F|Day of the week in a month|2|
    |w|Week number of the year|40|
    |W|Week number of the month|1|
    |a|AM/PM|PM|
    |k|Hour \(24-hour clock, 1 to 24\)|24|
    |k|Hour \(12-hour clock, 0 to 11\)|10|
    |z|Time zone|Eastern Standard Time|
    |'|Text delimiter|Delimiter|
    |"|Single quotation mark|"|


