# Replicate and distribute data

Log Service allows you to replicate data from a source Logstore and distribute the data to multiple destination Logstores. To replicate the data, you can create a data transformation rule for the source Logstore. This topic describes how to replicate data from a source Logstore and distribute the data to multiple destination Logstores in a typical scenario.

## Scenario

In this example, a data analytics company wants to replicate all data in a Logstore and distribute the data to two Logstores based on the content. To meet this requirement, the company can use the replication and distribution features in Log Service. The company can use the e\_set function to set tags, and use the e\_split function to split data based on the tags. Then, the company can use the e\_output function to distribute the split data to different Logstores. The following figure shows the basic logic.

![copy](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0643437161/p260237.png)

To use the replication and distribution features, you must make sure that the following requirements are met:

-   The performance of the target-a and target-b projects is evaluated, for example, the number of shards. For more information, see [Performance guide](/intl.en-US/Data Transformation/Performance guide.md).
-   A project named target-a and a Logstore named logstore-a are created. A project named target-b and a Logstore named logstore-b are created. For more information, see [Manage a Logstore](/intl.en-US/Data Collection/Preparation/Manage a Logstore.md).

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the source project.

3.  On the **Log Storage** \> **Logstores** tab, click the source Logstore.

4.  On the Search & Analysis page, click **Data Transformation** in the upper-right corner to enable the data transformation mode.

5.  Enter the following transformation statement in the editor:

    ```
    e_set("tags","target-a","target-b")
    e_split("tags")
    e_if(op_eq(v("tags"), "target-a"), e_output("target-a"))
    e_if(op_eq(v("tags"), "target-b"), e_output("target-b"))
    e_drop()
    ```

    -   Set the target-a and target-b tags for raw log entries by using the e\_set function. For more information, see [e\_set](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value assignment function.md).
    -   Split the log entries by using the e\_split function. For more information, see [e\_split](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Event processing functions.md).
    -   Distribute the log entries to the target-a and target-b storage targets by using the e\_output function. For more information, see [e\_output](t947540.dita#concept_1180783/section_zi7_wtp_30c).
    -   e\_drop\(\) indicates that the log entries that do not meet the specified conditions are dropped and not distributed. For more information, see [e\_drop](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Event processing functions.md).
6.  Click **Preview Data**.

    On the Transformation Results tab, the specified tags are added to the raw log entries. Log entries with the target-a tag are distributed to the target-a storage target. Log entries with the target-b tag are distributed to the target-b storage target.

    ![copy](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0643437161/p260249.png)

7.  On the page that appears, click **Save as Transformation Rule**.

8.  In the Create Data Transformation Rule panel, set the required parameters.

    1.  Set the basic parameters.

        |Parameter|Description|
        |---------|-----------|
        |Rule Name|The name of the transformation rule. Enter test.|
        |Authorization Method|Authorizes Log Service to read data from the source Logstore. The default role is used as an example. Select Default Role.|

    2.  Set the parameters in the Storage Target section for target-a.

        |Parameter|Description|
        |---------|-----------|
        |Target Name|The name of the storage target. Enter target-a.|
        |Target Region|The region where the destination project resides. Select China \(Hangzhou\).|
        |Target Project|The name of the destination project. Enter target-a.|
        |Target Logstore|The name of the destination Logstore. Enter logstore-a.|
        |Authorization Method|Authorizes Log Service to read data from and write data to the logstore-a Logstore of the target-a project. The default role is used as an example. Select Default Role. |

    3.  Set the parameters in the Storage Target section for target-b.

        |Parameter|Description|
        |---------|-----------|
        |Target Name|The name of the storage target. Enter target-b.|
        |Target Region|The region where the destination project resides. Select China \(Hangzhou\).|
        |Target Project|The name of the destination project. Enter target-b.|
        |Target Logstore|The name of the destination Logstore. Enter logstore-b.|
        |Authorization Method|Authorizes Log Service to read data from and write data to the lostore-b Logstore of the target-b project. The default role is used as an example. Select Default Role. |

    4.  Set the parameters in the Processing Range section.

        |Parameter|Description|
        |---------|-----------|
        |Time Range|The time when the logs are received. Select All. This value indicates that the data in the source Logstore is transformed from the first log entry until the transformation task is manually stopped.|

9.  Click **OK**.


-   In the Projects section, click the target-a project. On the **Log Storage** \> **Logstores** tab, select the logstore-a Logstore. You can view the distributed data.

    ![a](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0643437161/p260253.png)

-   In the Projects section, click the target-b project. On the **Log Storage** \> **Logstores** tab, select the logstore-b Logstore. Then, you can view the distributed data.

    ![b](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0643437161/p260251.png)


