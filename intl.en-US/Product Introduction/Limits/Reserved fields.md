# Reserved fields

When you collect logs or ship logs to other cloud services, Log Service adds log sources and timestamps to the logs in the form of key-value pairs. These fields are reserved in Log Service. This topic describes the reserved fields of Log Service.

**Note:**

-   When you add Logtail configurations or call API operations to write log data, you cannot set the names of fields \(keys\) to be the same as those of reserved fields. If the names are the same, inaccurate queries or other issues may occur due to duplicate field names.
-   Log Service does not ship fields that are prefixed by `__tag__`.
-   You are charged for the fields that you add for logs based on the pay-as-you-go billing method. If you enable the indexing feature for the fields, you are also charged a small fee for index traffic and storage. For more information, see [Pay-as-you-go](/intl.en-US/Pricing/Pay-as-you-go.md).

|Field|Type|Index and log analysis settings|Description|
|:----|:---|:------------------------------|:----------|
|`__time__`|Integer \(UNIX timestamp\)|-   Index settings: The `__time__` field is specified by using the from and to parameters in API operations. You do not need to create an index for this field.
-   Log analysis settings: If you turn on the Enable Analytics switch for a column, the log analysis feature is enabled for the `__time__` field by default.

|The time when log data is written to a Logstore by using the API or an SDK. This field can be used to ship, query, and analyze logs.|
|`__source__`|String|-   Index settings: If you enable the indexing feature, Log Service creates an index for the `__source__` field by default. The index is of the text type. No delimiter is specified for the indexes. To query logs based on the index on this field, you can enter `source:127.0.0.1` or `__source__:127.0.0.1`.
-   Log analysis settings: If you turn on the Enable Analytics switch for a column, the log analysis feature is enabled for the `__source__` field by default.

|The machine from which logs are collected. This field can be used to ship, query, analyze, and consume logs.|
|`__topic__`|String|-   Index settings: If you enable the indexing feature, Log Service creates an index for the `__topic__` field by default. The index is of the text type. No delimiter is specified for the indexes. To query logs based on the index of this field, you can enter `__topic__:XXX`.
-   Log analysis settings: If you turn on the Enable Analytics switch for a column, the log analysis feature is enabled for the `__topic__` field by default.

|The topic of a log. If you specify a topic for a log, Log Service adds a topic field to the log. The key of the field is `__topic__` and the value of the field is the topic content. This field can be used to ship, query, analyze, and consume logs. For more information, see [Log topic](/intl.en-US/Data Collection/Logtail collection/Text logs/Configure a log topic.md).|
|`_extract_others_`|String, which can be deserialized into a JSON map|This field does not exist in logs. You do not need to create an index for this field.|This field is the same as the `__extract_others__` field. We recommend that you use the `__extract_others__` field.|
|`__tag__:__client_ip__`|String|-   Index settings: If you enable the indexing feature, Log Service creates indexes for all fields by default. The indexes are of the text type. No delimiter is specified for the indexes. Exact match and fuzzy match are supported for log queries that are based on the index field.
-   Log analysis settings: By default, the log analysis feature is disabled for the column indicated by this field. To enable the log analysis feature for this field, you must create an index for the `__tag__:__client_ip__` field and turn on the Enable Analytics switch.

|The public IP address of the machine from which logs are collected. This field is a system tag. If you enable the Log Public IP feature, Log Service adds this field to each raw log that is collected from the log source. This field can be used to query, analyze, and consume logs. When you execute SQL statements to analyze this field, you must enclose this field in double quotation marks \(""\). For more information, see [Tag](/intl.en-US/Product Introduction/Basic concepts/Log.md) and [Log Public IP](/intl.en-US/Preparation/Manage a Logstore.md).|
|`__tag__:__receive_time__`|String, which can be converted to an integer in the UNIX timestamp format|-   Index settings: If you enable the indexing feature, Log Service creates indexes for all tags by default. The indexes are of the text type. No delimiter is specified for the indexes. Exact match and fuzzy match are supported for log queries that are based on the index field.
-   Log analysis settings: By default, the log analysis feature is disabled for the column indicated by this field. To enable the log analysis feature for this field, you must create an index for the `__tag__:__receive_time__` field and turn on the Enable Analytics switch.

|The time when Log Service receives a log. This field is a system tag. If you enable the Log Public IP feature, Log Service adds this field to each raw log that is collected from the log source. This field can be used to query, analyze, and consume logs. For more information, see [Tag](/intl.en-US/Product Introduction/Basic concepts/Log.md) and [Log Public IP](/intl.en-US/Preparation/Manage a Logstore.md).|
|`__tag__:__path__`|String|-   Index settings: If you enable the indexing feature, Log Service creates an index for the `__tag__:__path__` field by default. The index is of the text type. No delimiter is specified for the index. To query logs based on the index of this field, you can enter `__tag__:__path__:XXX`.
-   Log analysis settings: By default, the log analysis feature is disabled for the column indicated by this field. To enable the log analysis feature for this field, you must create an index for the `__tag__:__path__` field and turn on the Enable Analytics switch.

|The path to the log files collected by Logtail. Logtail automatically adds this field to logs. This field can be used to query, analyze, and consume logs. When you execute SQL statements to analyze this field, you must enclose this field in double quotation marks \(""\).|
|`__tag__:__hostname__`|String|-   Index settings: If you enable the indexing feature, Log Service creates an index for the `__tag__:__hostname__` field by default. The index is of the text type. No delimiter is specified for the index. To query logs based on the index of this field, you can enter `__tag__:__hostname__:XXX`.
-   Log analysis settings: By default, the log analysis feature is disabled for the column indicated by this field. To enable statistics for this field, create an index on the `__tag__:__hostname__` field and enable the log analysis feature.

|The hostname of the machine from which Logtail collects logs. Logtail automatically adds this field to logs. This field can be used to query, analyze, and consume logs. When you execute SQL statements to analyze this field, you must enclose this field in double quotation marks \(""\).|
|`__raw_log__`|String|You must create and configure an index of the text type for this field and enable the log analysis feature based on your business requirements.|The raw logs that fail to be parsed. If you disable the Drop Failed to Parse Logs feature, Logtail uploads raw logs that fail to be parsed. The key of this field is `__raw_log__` and the value of this field is the log content. This field can be used to ship, query, analyze, and consume logs. For more information, see [Drop Failed to Parse Logs](/intl.en-US/Data Collection/Logtail collection/Text logs/Overview.md).|
|`__raw__`|String|You must create and configure an index of the text type for this field and enable the log analysis feature based on your business requirements.|The raw logs that are parsed. If you enable the Upload Raw Log feature, Logtail uploads the raw logs in the `__raw__` field together with the parsed logs. This field can be used in log audit and compliance check scenarios. This field can be used to ship, query, analyze, and consume logs. For more information, see [Upload raw log](/intl.en-US/Data Collection/Logtail collection/Text logs/Overview.md).|

