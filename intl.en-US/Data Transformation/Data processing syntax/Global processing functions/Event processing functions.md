# Event processing functions

This topic describes the syntax of event processing functions and provides function examples.

## Functions

|Type|Function|Description|
|----|--------|-----------|
|Event processing|[e\_drop](#section_sn7_4pm_kly)|Drops log entries if the specified condition is met.|
|[e\_keep](#section_yq7_h7k_wgh)|Retains log entries if the specified condition is met.Both the e\_keep and e\_drop functions drop log entries. The difference is that the e\_keep function drops log entries if the specified condition is not met whereas the e\_drop function drops log entries if the specified condition is met.

```
# The following four transformation rules are equivalent:
e_if_else(e_search("f1==v1"), KEEP, DROP)
e_if_else(e_search("not f1==v1"), DROP) 
e_keep(e_search("f1==v1"))
e_drop(e_search("not f1==v1"))

# The following transformation rules are meaningless:
e_if(e_search("..."), KEEP)   
e_keep()
``` |
|Event splitting|[e\_split](#section_urg_dob_o79)|Splits a log entry to multiple log entries based on the value of a field. You can specify a JMESPath expression to extract fields from a log entry and then split the log entry into multiple log entries.|
|Event output|[e\_output, e\_coutput](#section_zi7_wtp_30c)|Sends log entries to a specified Logstore. You can specify the topic, source, and tags fields of the log entries. -   e\_output: sends log entries to a specified Logstore. If a log entry is sent to the specified Logstore, the following transformation rules are not executed for the log entry.
-   e\_coutput: sends log entries to a specified Logstore. If a log entry is sent to the specified Logstore, the following transformation rules are still executed for the log entry. |
|Event transformation to time series data|[e\_to\_metric](#section_u7i_ymg_jzp)|Transforms log data to the time series data and stores the data in a Metricstore.**Note:** After you transform log data to time series data, you must select the MetricStore as the destination Logstore to save the transformation results.

The following example shows typical time series data:

```
__labels__:host#$#myhost
__name__:rt
__time_nano__:1614739608000000000
__value__:123.0
``` |

## e\_drop

-   Syntax

    ```
    e_drop(condition=True)
    ```

    You can use the identifier DROP to drop a log entry. This identifier is equivalent to the e\_drop function.

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |condition|Bool|No|Default value: True. This parameter is typically set to a conditional expression.|

-   Response

    If the specified condition is met, log entries are dropped and None is returned. Otherwise, raw log entries are returned.

-   Examples
    -   Example 1: Evaluate log entries against the \_\_programe\_\_==access expression. If the value of the \_\_programe\_\_ field in a log entry is access, the log entry is dropped. Otherwise, the log entry is retained.
        -   Raw log entry

            ```
            __programe__: access
            age:  18
            content:  123
            name:  maki
            
            __programe__: error
            age:  18
            content:  123
            name:  maki
            ```

        -   Transformation rule

            ```
            e_if(e_search("__programe__==access"), DROP)
            ```

        -   Result

            The log entry whose \_\_programe\_\_ field value is access is dropped. The log entry whose \_\_programe\_\_ field value is error is retained.

            ```
            __programe__: error
            age:  18
            content:  123
            name:  maki
            ```

    -   Example 2: Evaluate a log entry against the k1==v1 expression. If the evaluation result of the conditional expression is True, the log entry is dropped.
        -   Raw log entry

            ```
            k1: v1
            k2: v2
            k3: k1
            ```

        -   Transformation rule

            ```
            e_drop(e_search("k1==v1"))
            ```

        -   Result

            This log entry is dropped because the evaluation result of the k1==v1 condition is True.

    -   Example 3: Evaluate a log entry against the not k1==v1 expression. If the evaluation result of the conditional expression is False, the log entry is retained.
        -   Raw log entry

            ```
            k1: v1
            k2: v2
            k3: k1
            ```

        -   Transformation rule

            ```
            e_drop(e_search("not k1==v1"))
            ```

        -   Result

            ```
            k1: v1
            k2: v2
            k3: k1
            ```

    -   Example 4: Specify no conditional expression in the e\_drop function and use the default value True to drop a log entry.
        -   Raw log entry

            ```
            k1: v1
            k2: v2
            k3: k1
            ```

        -   Transformation rule

            ```
            e_drop()    
            ```

        -   Result

            The log entry is dropped.


## e\_keep

-   Syntax

    ```
    e_keep(condition=True)
    ```

    You can use the identifier KEEP to retain a log entry. This identifier is equivalent to the e\_keep function.

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |condition|Bool|No|Default value: True. This parameter is typically set to a conditional expression.|

-   Response

    If the specified condition is met, the raw log entries are returned. If the specified condition is not met, log entries are dropped.

-   Examples
    -   Example 1: Evaluate log entries against the \_\_programe\_\_==access expression. If the value of the `__programe__` field in a log entry is access, the log entry is retained. Otherwise, the log entry is dropped.
        -   Raw log entry

            ```
            __programe__: access
            age:  18
            content:  123
            name:  maki
            __programe__: error
            age:  18
            content:  123
            name:  maki
            ```

        -   Transformation rule

            ```
            e_keep(e_search("__programe__==access"))
            # Equivalent to:
            e_if(e_search("not __programe__==access"), DROP) 
            # Equivalent to:
            e_if_else(e_search("__programe__==access"), KEEP, DROP)  
            ```

        -   Result

            The log entry whose \_\_programe\_\_ field value is access is retained. The log entry whose \_\_programe\_\_ field value is error is dropped.

            ```
            __programe__: access
            age:  18
            content:  123
            name:  maki
            ```

    -   Example 2: Evaluate a log entry against the k1==v1 expression. If the evaluation result of the conditional expression is True, the log entry is retained.
        -   Raw log entry

            ```
            k1: v1
            k2: v2
            k3: k1
            ```

        -   Transformation rule

            ```
            e_keep(e_search("k1==v1"))
            ```

        -   Result

            ```
            k1: v1
            k2: v2
            k3: k1
            ```

    -   Example 3: Evaluate a log entry against the not k1==v1 expression. If the evaluation result of the conditional expression is False, the log entry is dropped.
        -   Raw log entry

            ```
            k1: v1
            k2: v2
            k3: k1
            ```

        -   Transformation rule

            ```
            e_keep(e_search("not k1==v1"))
            ```

        -   Result

            The log entry is dropped.

    -   Example 4: Specify the False expression in the e\_keep function to drop a log entry.
        -   Raw log entry

            ```
            k1: v1
            k2: v2
            k3: k1
            ```

        -   Transformation rule

            ```
            e_keep(False)
            ```

        -   Result

            The log entry is dropped.


## e\_split

-   Syntax

    ```
    e_split(Field name, sep=',', quote='"', lstrip=True, jmes=None, output=None)
    ```

-   Splitting rules
    1.  If you specify the jmes parameter, the system converts the value of the field to a JSON list. The system also uses the JMESPath expression to extract values from the JSON list and uses these values in the next step. If you do not specify the jmes parameter, the system directly uses the value of the field in the next step.
    2.  If the value obtained from the previous step is a list or a string that represents a JSON list, the system splits the event based on this value. Otherwise, the system parses the value to multiple delimited values based on the sep, quote, and lstrip parameters. Then, the system splits the event based on these values.
-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Field name|String|Yes|The name of the field used to split the event. For more information about how to set special field names, see [Event structure and fields](/intl.en-US/Data Transformation/Data processing syntax/Data structures.md).|
    |sep|String|No|The delimiter used to separate values.|
    |quote|String|No|The character used to enclose a value.|
    |lstrip|String|No|Specifies whether to remove the spaces that are followed by a value. Default value: True.|
    |jmes|String|No|The JMESPath string used to convert the value of the field to a JSON object and extract values from the JSON object.|
    |output|String|No|The new field name, which overwrites the existing field name by default.|

-   Response

    A log list is returned. The values of fields in the list are all those from the source log.

-   Example
    -   Raw log entry

        ```
        __topic__:   
        age:  18
        content:  123
        name:  maki
        
        __topic__:   
        age:  18
        content:  123
        name:  maki
        ```

    -   Transformation rule

        ```
        e_set("__topic__", "V_SENT,V_RECV,A_SENT,A_RECV")
        e_split("__topic__")
        ```

    -   Result

        ```
        __topic__:  A_SENT
        age:  18
        content:  123
        name:  maki
        
        __topic__:  V_RECV
        age:  18
        content:  123
        name:  maki
        
        ...
        ```


## e\_output, e\_coutput

-   Syntax

    ```
    e_output(name=None, project=None, logstore=None, topic=None, source=None, tags=None)
    e_coutput(name=None, project=None, logstore=None, topic=None, source=None, tags=None)
    ```

    During preview, log entries are not written to the destination Logstore but to a Logstore named internal-etl-log. The internal-etl-log Logstore is a dedicated Logstore created by the system in the current project when you preview a data transformation task for the first time. You cannot modify the configurations of this Logstore or write other data to this Logstore. You are not charged for the Logstore.

-   Parameters

    **Note:** If you specify the name, project, and logstore parameters in the e\_output function or e\_coutput function and specify the destination project and Logstore in the Create Data Transformation Rule panel, the configurations in the e\_output and e\_coutput functions prevail. The following list shows the differences of the configurations:

    -   If you specify only the name parameter in the e\_output or e\_coutput function, the transformed data is stored in the destination Logstore that stores the storage target.
    -   If you specify only the project and logstore parameters in the e\_output function, the transformed data is stored in the Logstore specified in the function.

        If you use an AccessKey pair to authorize data transformation, the AccessKey pair of the current logon account is used for data transformation.

    -   If you specify the name, project, and logstore parameters in the e\_output function, the transformed data is stored in the Logstore specified in the function.

        If you use an AccessKey pair to authorize data transformation, the AccessKey pair that you specify when you configure the storage target is used for data transformation.

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |name|String|No|The name of the storage target. Default value: None.|
    |project|String|No|The existing project to which the log entries are written.|
    |logstore|String|No|The existing Logstore to which the log entries are written.|
    |topic|String|No|The new topic of the log entries.|
    |source|String|No|The source of the log entries.|
    |tags|Dict|No|The new tags of the log entries. The tags are in the dictionary format. **Note:** You do not need to prefix keywords with `__tag__:`. |

-   Default storage target

    To use the e\_output or e\_coutput function, you must configure a default storage target on the Create Data Transformation Rule pane. By default, Log Service uses the storage target labelled 1 as the default storage target. For example, transformed data is respectively sent to destination Logstores that store target\_01, target\_02, and target\_03. Data that is not dropped during transformation is stored in the Logstore that stores the default storage target \(target\_00\), as shown in the following figure.

    ![Default storage target](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7444836061/p133427.png)

-   Advanced parameter settings

    If the project or Logstore that you specify in the e\_output or e\_coutput function does not exist, you can set key-value pairs in the **Advanced Parameter Settings** section of the Create Data Transformation Rule panel. You can set a key to config.sls\_output.failure\_strategy and a value to \{"drop\_when\_not\_exists":"true"\} to skip a log entry. The skipped log entry is dropped and reported as a warning-level log entry. If you do not set key-value pairs in the **Advanced Parameter Settings** section, the data transformation tasks are suspended until the specified project or Logstore is created.

    **Warning:** If the specified project or Logstore does not exist and you set key-value pairs in **Advanced Parameter Settings** to skip a log entry, the log entry will be dropped. Proceed with caution.

    ![Advanced parameters](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2181813061/p133645.png)

-   Result
    -   e\_output: sends log entries to the specified Logstore. After a log entry is sent, the remaining transformation rules are not executed on the log entry.
    -   e\_coutput: sends log entries to the specified Logstore. After a log entry is sent, the remaining transformation rules are still executed on the log entry.
-   Example
    -   Raw log entry

        ```
        __topic__:  
        k1: v1
        k2: v2
        x1: v3
        x5: v4
        ```

    -   Transformation rule

        The e\_drop\(\) function deletes the data filtered by the e\_if\(\) function. If you do not add the e\_drop\(\) function, the filtered data is delivered to the default storage target.

        ```
        e_if(e_match("k2", r"\w+"), e_output(name="target2", source="source1", topic="topic1"))
        e_drop()
        ```

    -   Result

        ```
        __topic__:  topic1
        k1: v1
        k2: v2
        x1: v3
        x5: v4
        ```


## e\_to\_metric

-   Syntax

    ```
    e_to_metric(names=None, labels=None, time=None)
    ```

-   Parameters

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |names|String, string list, or tuple list|Yes|The metric names of time series data. The metric names can be a single string, string list, or tuple list. The values correspond to log field names.    -   String: contains a string, for example, "rt". A time series data record is returned. The data record contains `__name__:rt`.
    -   String list: contains multiple strings, for example, \["rt", "qps"\]. Two time series data records are returned. One data record contains `__name__:rt` and the other data record contains `__name__:qps`.
    -   Tuple list: contains multiple tuples, for example, \[\("rt","max\_rt"\),\("qps", "total\_qps"\)\]. The first element of the tuple is a raw log field. The second element is a metric name of the transformed time series data. Two time series data records are returned. One data record contains `__name__:max_rt` and the other data record contains `__name__:total_qps`. |
    |labels|String, string list, or tuple list|No|The labels of time series data. The labels can be a single string, string list, or tuple list. The values correspond to log field names.**Note:** In the following sample values, host and app are log field names, and hostvalue and appvalue are log field values.

    -   String: contains a string, for example, "host". A time series data record is returned. The data record contains `__label__:host#$#hostvalue`.
    -   String list: contains multiple strings, for example, \["host", "app"\]. Two time series data records are returned. One data record contains `__label__:host#$#hostvalue` and the other data record contains `__label__:app#$#appvalue`.
    -   Tuple list: contains multiple tuples, for example, \[\("host", "hostname"\),\("app", "appname"\)\]. The first element of the tuple is a raw log field. The second element is a label of the processed time series data. Two time series data records are returned. One data record contains `__label__:hostname#$#hostvalue` and the other data record contains `__label__:appname#$#appvalue`. |
    |time|String, integer, or float|No|The timestamp of time series data. Unit: seconds, milliseconds, microseconds, or nanoseconds. When you store the time series data, the timestamp is converted to the `__time_nano__` field and stored in nanoseconds by default.|

-   Response

    Time series data is returned.

-   Examples
    -   Example 1: Convert a log entry that contains the `rt` field is located to time series data.
        -   Raw log entry

            ```
            __time__: 1614739608
            rt: 123
            ```

        -   Transformation rule

            ```
            e_to_metric(names="rt")
            ```

        -   Result

            ```
            __labels__:
            __name__:rt
            __time_nano__:1614739608000000000
            __value__:123.0
            ```

    -   Example 2: Convert a log entry that contains the `rt` field to time series data and use the `host` field as the new labels field.
        -   Raw log entry

            ```
            __time__: 1614739608
            rt: 123
            host: myhost
            ```

        -   Transformation rule

            ```
            e_to_metric(names="rt", labels="host")
            ```

        -   Result

            ```
            __labels__:host#$#myhost
            __name__:rt
            __time_nano__:1614739608000000000
            __value__:123.0
            ```

    -   Example 3: Convert a log entry that contains the `rt` and `qps` fields to time series data, and use the `host` field as the new labels field.
        -   Raw log entry

            ```
            __time__: 1614739608
            rt: 123
            qps: 10
            host: myhost
            ```

        -   Transformation rule

            ```
            e_to_metric(names=["rt","qps"], labels="host")
            ```

        -   Result

            ```
            __labels__:host#$#myhost
            __name__:rt
            __time_nano__:1614739608000000000
            __value__:123.0
            
            __labels__:host#$#myhost
            __name__:qps
            __time_nano__:1614739608000000000
            __value__:10.0
            ```

    -   Example 4: Convert a log entry that contains the `rt` and `qps` fields to time series data. Replace the field names with `max_rt` and `total_qps`, and use the `host` field as the new labels field.
        -   Raw log entry

            ```
            __time__: 1614739608
            rt: 123
            qps: 10
            host: myhost
            ```

        -   Transformation rule

            ```
            e_to_metric(names=[("rt","max_rt"),("qps","total_qps")], labels="host")
            ```

        -   Result

            ```
            __labels__:host#$#myhost
            __name__:max_rt
            __time_nano__:1614739608000000000
            __value__:123.0
            
            __labels__:host#$#myhost
            __name__:total_qps
            __time_nano__:1614739608000000000
            __value__:10.0
            ```

    -   Example 5: Convert a log entry that contains the `rt` and `qps` fields to time series data. Replace the field names with `max_rt` and `total_qps`. Rename the `host` field to `hostname` and use hostname as the new labels field.
        -   Raw log entry

            ```
            __time__: 1614739608
            rt: 123
            qps: 10
            host: myhost
            ```

        -   Transformation rule

            ```
            e_to_metric(names=[("rt","max_rt"),("qps","total_qps")], labels=[("host","hostname")])
            ```

        -   Result

            ```
            __labels__:hostname#$#myhost
            __name__:max_rt
            __time_nano__:1614739608000000000
            __value__:123.0
            
            __labels__:hostname#$#myhost
            __name__:total_qps
            __time_nano__:1614739608000000000
            __value__:10.0
            ```

    -   Example 6: Convert a log entry that contains the `rt` and `qps` fields to a time series data. Replace the field names with `max_rt` and `total_qps`. Rename the `host` field to `hostname` and use hostname as the new labels field. Use the value of the `mytime` field as the value of the `__time_nano__` time series data.
        -   Raw log entry

            ```
            __time__: 1614739608
            mytime: 1614739608.123
            rt: 123
            qps: 10
            host: myhost
            ```

        -   Transformation rule

            ```
            e_to_metric(names=[("rt","max_rt"),("qps","total_qps")], labels=[("host","hostname")], time=v("mytime"))
            ```

        -   Result

            ```
            __labels__:hostname#$#myhost
            __name__:max_rt
            __time_nano__:1614739608123000000
            __value__:123.0
            
            __labels__:hostname#$#myhost
            __name__:total_qps
            __time_nano__:1614739608123000000
            __value__:10.0
            ```


