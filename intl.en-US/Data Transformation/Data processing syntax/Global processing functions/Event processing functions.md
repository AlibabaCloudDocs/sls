# Event processing functions

This topic describes the syntax of event processing functions and provides parameter description and function examples.

## Functions

|Type|Function|Description|
|----|--------|-----------|
|Event processing|[e\_drop](#section_sn7_4pm_kly)|Drops log entries if the specified condition is met.|
|[e\_keep](#section_yq7_h7k_wgh)|Retains log entries if the specified condition is met.Both the e\_keep and e\_drop functions drop log entries. The difference is that the e\_keep function drops log entries if the specified condition is not met while the e\_drop function drops log entries if the specified condition is met.

```
# The following four transformation rules are equivalent.
e_if_else(e_search("f1==v1"), KEEP, DROP)
e_if_else(e_search("not f1==v1"), DROP) 
e_keep(e_search("f1==v1"))
e_drop(e_search("not f1==v1"))

# The following transformation rules are meaningless.
e_if(e_search("..."), KEEP)   
e_keep()
``` |
|Event splitting|[e\_split](#section_urg_dob_o79)|Splits a log entry to multiple log entries based on the value of a field. You can specify a JMESPath expression to extract fields from a log entry and then split the log entry into multiple log entries.|
|Event output|[e\_output, e\_coutput](#section_zi7_wtp_30c)|Sends log entries to a specified Logstore. You can specify the topic, source, and tags fields of log entries sent to the specified Logstore. -   e\_output: sends log entries to a specified Logstore. If a log entry is sent to the specified Logstore, the following transformation rules are not executed for the log entry.
-   e\_coutput: sends log entries to a specified Logstore. If a log entry is sent to the specified Logstore, the following transformation rules are still executed for the log entry. |

## e\_drop

-   Syntax

    ```
    e_drop(condition=True)
    ```

    You can use the identifier DROP to drop a log entry. This identifier is equivalent to the e\_drop function.

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |condition|Bool|No|Default value: True. This parameter is typically set to a conditional expression.|

-   Result

    If the specified condition is met, log entries are dropped and None is returned. Otherwise, the raw log entries are returned.

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

        -   Result:

            The log entry whose \_\_programe\_\_ field value is access is dropped while the log entry whose \_\_programe\_\_ field value is error is retained.

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

        -   Result:

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

        -   Result:

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

        -   Result:

            The log entry is dropped.


## e\_keep

-   Syntax

    ```
    e_keep(condition=True)
    ```

    You can use the identifier KEEP to retain a log entry. This identifier is equivalent to the e\_keep function.

-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
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

        -   Result:

            The log entry whose \_\_programe\_\_ field value is access is retained while the log entry whose \_\_programe\_\_ field value is error is dropped.

            ```
            __programe__: access
            age:  18
            content:  123
            name:  maki
            ```

    -   Example: Evaluate a log entry against the k1==v1 expression. If the evaluation result of the conditional expression is True, the log entry is retained.
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

        -   Result:

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

        -   Result:

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

        -   Result:

            The log entry is dropped.


## e\_split

-   Syntax

    ```
    e_split(Field name, sep=',', quote='"', lstrip=True, jmes=None, output=None)
    ```

-   Splitting rules
    1.  If you specify the jmes parameter, the system converts the value of the field to a JSON list, and uses the JMESPath expression to extract values from the JSON list, which will be used in the next step. If you do not specify the jmes parameter, the system directly uses the value of the field in the next step.
    2.  If the value obtained from the previous step is a list or a string that represents a JSON list, the system splits the event based on this value. Otherwise, the system parses the value to multiple delimited values based on the sep, quote, and lstrip parameters. Then, the system splits the event based on these values.
-   Parameters

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |Log field|String|Yes|The name of the field used to split the event. For more information about how to set special field names, see [Event structure and fields](/intl.en-US/Data Transformation/Data processing syntax/Data structures.md).|
    |sep|String|No|The delimiter used to separate values.|
    |quote|String|No|The character used to enclose a value.|
    |lstrip|String|No|Specifies whether to remove the spaces before a value. Default value: True.|
    |jmes|String|No|The JMESPath string used to convert the value of the field to a JSON object and extract values from the JSON object.|
    |output|String|No|The new field name, which overwrites the existing field name by default.|

-   Result

    A log list is returned. The values of fields in the list are all those from the source log.

-   Examples
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

    -   Result:

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

    During preview, log entries are not written to the destination Logstore but to the Logstore named internal-etl-log. The internal-etl-log Logstore is a dedicated Logstore created by the system in the current project when you preview a data transformation task for the first time. You cannot modify the configuration of this Logstore or write other data to this Logstore. It is free of charge.

-   Description

    **Note:** If you specify the name, project, and logstore parameters in the e\_output function or e\_coutput function and specify the destination project and Logstore on the Create Data Transformation Rule pane, the configurations in the e\_output and e\_coutput functions prevail. The following list shows the differences of the configurations:

    -   If you specify only the name parameter in the e\_output or e\_coutput function, the transformed data is stored in the destination Logstore that stores the storage target.
    -   If you specify only the project and logstore parameters in the e\_output function, the transformed data is stored in the Logstore specified in the function.

        If you use an AccessKey pair to authorize data transformation, the AccessKey pair of the current logon account is used for data transformation.

    -   If you specify the name, project, and logstore parameters in the e\_output function, the transformed data is stored in the Logstore specified in the function.

        If you use an AccessKey pair to authorize data transformation, the AccessKey pair that you specify when you configure the storage target is used for data transformation.

    |Parameter|Data type|Required|Description|
    |---------|---------|--------|-----------|
    |name|String|No|The name of the storage target. Default value: None.|
    |project|String|No|The existing project to which the log entries are written.|
    |logstore|String|No|The existing Logstore to which the log entries are written.|
    |topic|String|No|The new topic of the log entries.|
    |source|String|No|The source of the log entries.|
    |tags|Dict|No|The new tags of the log entries, in the dictionary format. **Note:** You do not need to prefix keywords with `__tag__:`. |

-   Set default storage target

    To use the e\_output or e\_coutput function, you must configure a default storage target on the Create Data Transformation Rule pane. By default, Log Service uses the storage target labelled 1 as the default storage target. For example, transformed data is respectively sent to destination Logstores that store target\_01, target\_02, and target\_03. Data that is not dropped during transformation is stored in the Logstore that stores the default storage target \(target\_00\), as shown in the following figure.

    ![Default storage target](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2181813061/p133427.png)

-   Advanced parameter settings

    If the project or Logstore that you specify in the e\_output or e\_coutput function does not exist, you can set key-value pairs in the **Advanced Parameter Settings** section of the Create Data Transformation Rule pane. You can set a key to config.sls\_output.failure\_strategy and a value to \{"drop\_when\_not\_exists":"true"\} to skip a log entry. The skipped log entry is dropped and reported as a WARNING-level log entry. If you do not set key-value pairs in the **Advanced Parameter Settings** section, the data transformation tasks are suspended until the specified project or Logstore is created.

    **Warning:** If the specified project or Logstore does not exist and you set key-value pairs in the **Advanced Parameter Settings** section to skip a log entry, the log entry will be dropped. Proceed with caution.

    ![Advanced parameter](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2181813061/p133645.png)

-   Result:
    -   e\_output: sends log entries to the specified Logstore. After a log entry is sent, the following transformation rules are not executed on the log entry.
    -   e\_coutput: sends log entries to the specified Logstore. After a log entry is sent, the following transformation rules are still executed on the log entry.
-   Examples
    -   Raw log entry

        ```
        __topic__:  
        k1: v1
        k2: v2
        x1: v3
        x5: v4
        ```

    -   Transformation rule

        ```
        e_if(e_match("k2", r"\w+"), e_output(name="target2", source="source1", topic="topic1"))
        ```

    -   Result:

        ```
        __topic__:  topic1
        k1: v1
        k2: v2
        x1: v3
        x5: v4
        ```


