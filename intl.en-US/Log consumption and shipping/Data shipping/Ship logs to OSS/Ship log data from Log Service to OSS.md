# Ship log data from Log Service to OSS

You can use Log Service to collect log data and ship the log data to Object Storage Service \(OSS\) for storage and analysis. This topic describes how to ship log data from Log Service to OSS.

-   A project and a Logstore are created. For more information, see [Create a project and a Logstore](/intl.en-US/Quick Start/Quick start.md).
-   Log data is collected. For more information, see [Log collection](/intl.en-US/Data Collection/Log collection methods.md).
-   OSS is activated. A bucket is created in the region where the Log Service project resides. For more information, see [Activate OSS](/intl.en-US/Console User Guide/Sign up for OSS.md).
-   Log Service is authorized to access cloud resources in OSS. You can authorize Log Service on the [Cloud Resource Access Authorization](https://ram.console.aliyun.com/#/role/authorize?request=%7B%22Requests%22%3A%20%7B%22request1%22%3A%20%7B%22RoleName%22%3A%20%22AliyunLogDefaultRole%22%2C%20%22TemplateId%22%3A%20%22DefaultRole%22%7D%7D%2C%20%22ReturnUrl%22%3A%20%22https%3A//sls.console.aliyun.com/%22%2C%20%22Service%22%3A%20%22Log%22%7D) page.

    To ship log data across Alibaba Cloud accounts or by using a RAM user, see [Authorization in the RAM console](/intl.en-US/Log consumption and shipping/Data shipping/Ship logs to OSS/Authorization in the RAM console.md).

    ![Cloud Resource Access Authorization](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8257549951/p112344.png)


Log Service supports automatic shipping of log data from a Logstore to OSS.

-   You can set a custom retention period for log data in OSS. Permanent retention is supported.
-   You can use data processing platforms such as E-MapReduce and Data Lake Analytics \(DLA\) or use custom programs to consume log data in OSS.

## Ship log data

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the destination project.

3.  On the page that appears, choose **Log Storage** \> **Logstores**. In the Logstore list, click the **\>** icon of the destination Logstore, and then choose **Data Transformation** \> **Export** \> **Object Storage Service \(OSS\)**.

4.  On the OSS Shipper page, click **Enable**.

5.  In the Shipping Notes dialog box, click **Ship**.

6.  Configure a rule for shipping log data to OSS.

    The following table describes the parameters.

    |Parameter|Description|
    |:--------|:----------|
    |OSS Shipper Name|The name of the shipping rule. The name can contain only lowercase letters, digits, hyphens \(-\), and underscores \(\_\). The name must start and end with a lowercase letter or digit and must be 2 to 128 characters in length. |
    |OSS Bucket|The name of the OSS bucket to which you want to ship log data. **Note:** You must specify the name of an existing OSS bucket. The specified OSS bucket must reside in the same region as the Log Service project. |
    |OSS Prefix|The directory to which log data is shipped in the OSS bucket.|
    |Shard Format|The partition format of the directory of a shipping task. The directory is generated based on the time when the shipping task is created. The default format is %Y/%m/%d/%H/%M. The value of this parameter cannot start with a forward slash \(/\). For information about partition format examples, see [Partition format](#section_gzu_5la_tzu). For information about the parameters of partition formats, see [strptime API](http://man7.org/linux/man-pages/man3/strptime.3.html).|
    |RAM Role|The Alibaba Cloud Resource Name \(ARN\) of the RAM role. The RAM role is the identity that the OSS bucket owner creates for access control. For example, acs:ram::45643:role/aliyunlogdefaultrole. For more information, see [Obtain the ARN of a RAM role](#section_6y4_v2h_1n4).|
    |Shipping Size|The maximum size of raw log data that can be shipped to the OSS bucket in a shipping task. Valid values: 5 to 256. Unit: MB. If the size of shipped data exceeds the specified value, a new shipping task is created. |
    |Storage Format|The storage format of log data in OSS. Data shipped to OSS can be in the JSON, CSV, or Parquet format. For more information, see [CSV-formatted data](/intl.en-US/Log consumption and shipping/Data shipping/Ship logs to OSS/CSV-formatted data.md).|
    |Compress|Specifies whether to compress log data that is shipped to OSS.     -   No Compress: Log data that is shipped to OSS is not compressed.
    -   Compress \(snappy\): The [Snappy](https://google.github.io/snappy/) utility is used to compress the log data that is shipped to OSS. This releases more of the storage space of the OSS bucket. |
    |Ship Tags|Specifies whether to ship log tags.|
    |Shipping Time|The time period during which a shipping task runs. Valid values: 300 to 900. Default value: 300. Unit: seconds. If the specified time is reached, another shipping task is created. |

7.  Click **OK**.

    **Note:**

    -   After you configure a shipping rule, multiple shipping tasks can concurrently run. Another task is created if the size of data shipped from a shard reaches the specified threshold or the specified shipping time is reached.
    -   After you create a shipping task, you can check whether the shipping rule satisfies your business requirements based on the task status and the data shipped to OSS.

## View OSS data

After log data is shipped to OSS, you can view the log data in the OSS console by using an API, SDK, or another method. For more information, see [OSS bucket management](/intl.en-US/Console User Guide/Upload, download, and manage objects/Overview.md).

The following script shows a sample OSS directory:

```
oss://OSS-BUCKET/OSS-PREFIX/PARTITION-FORMAT_RANDOM-ID
```

OSS-BUCKET specifies the name of the destination OSS bucket. OSS-PREFIX specifies the prefix of the directory in the destination OSS bucket. PARTITION-FORMAT specifies the partition format of the directory of a shipping task. The partition format is calculated based on the time when the shipping task is created. For more information, see [strptime API](http://man7.org/linux/man-pages/man3/strptime.3.html). RANDOM-ID is the unique identifier of a shipping task.

**Note:** A directory in an OSS bucket is created based on the creation time of a shipping task. For example, a shipping task is created at 00:00:00 on June 23, 2016. The task ships the data that is written to Log Service after 23:55:00 on June 22, 2016 to OSS. The shipping interval is 5 minutes. To retrieve all logs shipped on June 22, 2016, you must check all objects in the 2016/06/22 directory. You must also check the 2016/06/23/00/ directory for objects that are generated in the first 10 minutes of June 23, 2020.

## Partition format

For each shipping task, log data is written to a directory of an OSS object. The directory is in the oss://OSS-BUCKET/OSS-PREFIX/PARTITION-FORMAT\_RANDOM-ID format. A partition format is obtained by formatting the time when a shipping task is created. The following table describes the partition formats and directories that are obtained when a shipping task is created at 19:50:43 on January 20, 2017.

|OSS Bucket|OSS Prefix|Partition format|OSS directory|
|:---------|:---------|:---------------|:------------|
|test-bucket|test-table|%Y/%m/%d/%H/%M|oss://test-bucket/test-table/2017/01/20/19/50\_1484913043351525351\_2850008|
|test-bucket|log\_ship\_oss\_example|year=%Y/mon=%m/day=%d/log\_%H%M%s|oss://test-bucket/log\_ship\_oss\_example/year=2017/mon=01/day=20/log\_195043\_1484913043351525351\_2850008.parquet|
|test-bucket|log\_ship\_oss\_example|ds=%Y%m%d/%H|oss://test-bucket/log\_ship\_oss\_example/ds=20170120/19\_1484913043351525351\_2850008.snappy|
|test-bucket|log\_ship\_oss\_example|%Y%m%d/|oss://test-bucket/log\_ship\_oss\_example/20170120/\_1484913043351525351\_2850008|
|test-bucket|log\_ship\_oss\_example|%Y%m%d%H|oss://test-bucket/log\_ship\_oss\_example/2017012019\_1484913043351525351\_2850008|

To use the partition information when you use Hive, MaxCompute, or Data Lake Analytics \(DLA\) to analyze OSS data, you can set PARTITION-FORMAT in the key=value format. For example, you can set the partition format to oss://test-bucket/log\_ship\_oss\_example/year=2017/mon=01/day=20/log\_195043\_1484913043351525351\_2850008.parquet. In this example, year, mon, and day are specified as three partition keys.

## Related operations

After shipping tasks are created, you can modify the shipping rules, disable the tasks, view the status and error messages of the tasks, and retry failed tasks on the OSS Shipper page.

-   Modify the shipping rules of tasks

    Click **Settings** to modify the shipping rules of tasks. For information about parameters, see [Ship log data](#section_gz3_etl_ezq).

-   Disable shipping tasks

    Click **Disable**. All tasks are disabled.

-   View the status of tasks and error messages

    You can view the status of all log shipping tasks of the last two days.

    -   Status of a shipping task

        |Status|Description|
        |:-----|:----------|
        |Succeeded|The shipping task succeeded.|
        |Running|The shipping task is running. Check whether the task succeeds later.|
        |Failed|The shipping task failed. If the task failed and cannot be restarted because of external causes, troubleshoot the failure based on the error message and retry the task.|

    -   Error message

        If a shipping task failed, an error message is returned for the task.

        |Error message|Description|Solution|
        |:------------|:----------|:-------|
        |UnAuthorized|The error message returned because the AliyunLogDefaultRole role does not have the required permissions.|Troubleshoot the error by checking the following configurations:         -   Check whether the AliyunLogDefaultRole role is created by the OSS bucket owner.
        -   Check whether the Alibaba Cloud account ID configured in the permission policy is valid.
        -   Check whether the AliyunLogDefaultRole role is granted write permissions on the destination OSS bucket.
        -   Check whether the ARN of the AliyunLogDefaultRole role that you entered in the RAM Role field is correct. |
        |ConfigNotExist|The error message returned because the task does not exist.|Check whether the task is disabled. Enable the data shipping feature, configure a shipping rule for the task, and then retry the task.|
        |InvalidOssBucket|The error message returned because the destination OSS bucket does not exist.|Troubleshoot the error by checking the following configurations:         -   Check whether the destination OSS bucket resides in the same region as the source project.
        -   Check whether the specified bucket name is correct. |
        |InternalServerError|The error message returned because an internal error occurs in Log Service.|Retry the failed task.|

    -   Retry a task

        If a task fails, Log Service retries the task based on the retry policy by default. You can also manually retry the task. By default, Log Service retries all tasks of the past two days. The minimum interval between retries is 15 minutes. If a task fails for the first time, Log Service retries the task 15 minutes later. If the task fails for the second time, Log Service retries the task 30 minutes later. If the task fails for the third time, Log Service retries the task 60 minutes later, and so on.

        To immediately retry a failed task, you can click **Retry All Failed Tasks** or **Retry** on the right of the task that you want to retry. You can also use an API or SDK to retry a task.


## FAQ

How do I obtain the ARN of a RAM role?

1.  To obtain the ARN of a RAM role, log on to the [RAM console](https://ram.console.aliyun.com/).

2.  In the left-side navigation pane, click RAM Roles.

3.  In the RAM Role Name list, click the role named AliyunLogDefaultRole.

4.  On the page that appears, obtain the ARN of the role in the Basic Information section.

    ![Obtain the ARN of a RAM role](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5257549951/p113301.png)


