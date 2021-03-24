# Data transformation basics

The data transformation feature uses consumer groups to consume log data and uses transformation rules to transform log data. More than 200 built-in functions are available for you to orchestrate transformation rules. This topic describes how log data consumption is scheduled during data transformation and how the rules engine for data transformation works.

## Scheduling basics

The data transformation feature of Log Service uses a consumer group to consume log data from the source Logstore in the streaming mode, transforms each log entry based on the specified transformation rule, and then writes the transformed log data to one or more destination Logstores.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7943749951/p54657.png)

-   **Scheduling mechanism**

    For each transformation rule, the data transformation scheduler starts one or more running instances. Each running instance behaves as a consumer to consume data from one or more shards of the source Logstore. The scheduler determines the number of concurrent running instances based on the memory and CPU resources that are used by running instances. The maximum number of running instances that the scheduler can start is the same as the number of shards in the source Logstore.

-   **Running instance**

    Running instances read source log data from the shards that are allocated to them based on your configurations. The data is transformed based on the transformation rule and is then written to one or more destination Logstores. You can configure transformation rules to enrich log data by using external resources. Based on the consumer group mechanism, running instances record data consumption checkpoints in shards. The checkpoints are useful if consumption is unexpectedly interrupted. After an interruption ends, running instances can continue to consume data from the last checkpoint.

-   **Task termination**
    -   By default, if you do not set the end time of a transformation task, running instances do not exit and the task does not stop.
    -   If you set the end time of a transformation task, running instances consume log data until the end time. When the task reaches the end time, the instances exit and the task stops.
    -   By default, if a task is stopped and then restarted, running instances continue to consume data from the last recorded checkpoint.

## Rules engine basics: basic operations

You can use the built-in functions that are written in the [domain specific language \(DSL\) for Log Service](/intl.en-US/Data Transformation/Data processing syntax/Syntax introduction.md) to orchestrate transformation rules. Each function is a transformation step. The rules engine calls the functions of a transformation rule in sequence. For example, the following transformation rule is orchestrated by using four functions. The functions are four steps that are used to transform data:

```
e_set("log_type", "access_log")
e_drop_fields("__action")
e_if(e_search("ret: pass"), e_set("result", "pass"))
e_if(e_search("ret: unknown"), DROP)
```

The following figure shows the transformation logic.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7943749951/p54660.png)

-   **Basic logic**

    The rules engine calls each function that is defined in the rule in sequence. Each function processes and modifies each log entry, and returns a transformed log entry.

    For example, the `e_set("log_type", "access_log")` function adds the log\_type field to each log entry. The value of the field is `access_log`. Then, the next function receives each transformed log entry that contains the `log_type` field.

-   **Conditional expressions**

    You can set conditions in steps. If a log entry does not meet a condition in a step, this step is skipped for the log entry.

    For example, the `e_if(e_search("ret: pass"), e_set("result", "pass"))` function first checks whether the value of the `ret` field in a log entry contains pass. If no, this step is skipped for the log entry. If yes, the function sets the value of the `result` field in the log entry to pass.

-   **Transformation termination**

    If a function does not return a transformed log entry, the log entry is discarded.

    For example, the `e_if(e_search("ret: unknown"), DROP)` function discards a log entry in which the value of the `ret` field is unknown. After the log entry is discarded, the rules engine no longer calls subsequent functions to transform this log entry, and starts to transform the next log entry.


## Rules engine basics: data output, duplication, and splitting

The rules engine also supports log output, duplication, and splitting. For example, the following transformation rule is orchestrated by using four functions. The functions are four steps that are used to transform data:

```
e_output("archive_Logstore") )
e_split("log_type")
e_if(e_search("log_type: alert"), e_output("alert_Logstore") )
e_set("result", "pass")
```

The following example is a sample log entry to be transformed:

```
log_type: access,alert
content: admin login to database.
```

The following figure shows the transformation logic.

![Diagram for rules engine basics](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7943749951/p54676.png)

-   **Log output**

    Log output can be considered as a special way to stop transforming a log entry. As shown in the preceding figure, if the value of the `log_type` field in a log entry is alert, the `e_output("alert_Logstore")` function in step 3 is called to write the log entry to the specified destination Logstore. Then the log entry is discarded and the subsequent function is not called.

-   **Log duplication and output**

    The `e_output` function duplicates a log entry and writes the duplicated log entry to one or more destination Logstores. Then, the rules engine continues to call subsequent functions to transform the log entry. As shown in the preceding figure, log entries that are duplicated in step 1 are written to the destination Logstore named `archive_Logstore`.

-   **Log splitting**

    If the values of the `log_type` field in a log entry are access and alert, the `e_split("log_type")` function in step 2 is called to split the log entry into two log entries. The two log entries are the same except the value of the `log_type` field. The value of the field is access in a log entry and is alert in the other.

    The log entries that are generated after the splitting are transformed by the subsequent functions.


