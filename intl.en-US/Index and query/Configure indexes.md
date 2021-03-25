# Configure indexes

Indexes are used in a storage structure to sort one or more columns of log data. You can query and analyze data only after you configure indexes. Query and analysis results vary with index configurations. Therefore, you must configure indexes based on your business requirements.

Logs are collected. For more information, see [Log collection methods](/intl.en-US/Data Collection/Log collection methods.md).

## Index types

The following table describes the index types in Log Service.

|Index type|Description|
|:---------|:----------|
|Full-text index|Log Service splits an entire log entry into multiple words based on specified delimiters. In a query, specified field names \(keys\) or field values \(values\) are normal text. For example, the search statement `error` returns the log entries that contain the keyword `error`.|
|Field index|After you configure field indexes, you can query log entries. To query log entries, specify field name and field value as key-value pairs in the format of key:value. For example, the search statement `level:error` returns the log entries in which the value of the `level` field contains `error`.If you want to use the analysis feature, you must configure field indexes and turn on the Enable Analytics switch for the related fields. The analysis feature does not incur additional index traffic or occupy storage space. |

**Note:**

-   After you enable the indexing feature, index traffic and storage space occupied by indexes incur fees. For more information, see [Billing overview](/intl.en-US/Pricing/Billing overview.md).
-   The indexing feature is applicable only to the log data that is written to the current Logstore after you configure indexes. If you want to query and analyze historical data, you can use the reindexing feature. For more information, see [Reindex logs for a Logstore](/intl.en-US/Index and query/Query/Reindex logs for a Logstore.md).
-   If you configure full-text indexes and field indexes, the configurations of the field indexes take precedence.
-   By default, indexes are automatically configured for some reserved fields in Log Service. For more information, see [Reserved fields](/intl.en-US/Product Introduction/Limits/Reserved fields.md). By default, no delimiter is specified to split the values of the `__topic__` and `__source__` fields. Therefore, only exact match is supported for the two fields.

## Configure full-text indexes

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the project in which you want to query and analyze logs.

3.  On the **Log Storage** \> **Logstores** tab, click the Logstore where logs are stored.

4.  On the Search & Analysis page of the Logstore, choose **Index Attributes** \> **Attributes**.

    If the indexing feature is not enabled, click **Enable**.

5.  Configure indexes in the Search & Analysis panel.

    |Parameter|Description|
    |:--------|:----------|
    |**LogReduce**|If you turn on the **LogReduce** switch, Log Service automatically aggregates text logs that have the same pattern during log collection. This feature allows you to monitor the statuses of logs in real time. For more information, see [LogReduce](/intl.en-US/Index and query/Query/LogReduce.md).|
    |**Full Text Index**|If you turn on the **Full Text Index** switch, the full-text index feature is enabled.|
    |**Case Sensitive**|Specifies whether searches are case-sensitive.     -   If you turn on the **Case Sensitive** switch, searches are case-sensitive. For example, if a log entry contains `internalError`, you can search for the log entry only by using the keyword `internalError`.
    -   If you turn off the **Case Sensitive** switch, searches are not case-sensitive. For example, if a log entry contains `internalError`, you can search for the log entry by using the keyword `INTERNALERROR` or `internalerror`. |
    |**Include Chinese**|Specifies whether to distinguish between Chinese characters and English in searches.     -   After you turn on the **Include Chinese** switch, if a log entry contains Chinese characters, the Chinese content is split based on the Chinese syntax. The English content is split based on specified delimiters.

**Note:** When the Chinese content is split, the data write speed is reduced. Proceed with caution.

    -   If you turn off the **Include Chinese** switch, all content is split based on specified delimiters. |
    |**Delimiter**|The delimiters that are used to split the content of a log entry into multiple words. The delimiters include `,'";=()[]{}?@&>/:\n\t\r`, where `\n` indicates a line feed, `\t` indicates a tab character, and `\r` indicates a carriage return character.For example, the content of a log entry is `/url/pic/abc.gif`.

    -   If you do not specify a delimiter, the log entry is processed as single word in the `/url/pic/abc.gif` format. You can search for the log entry only by using the keyword `/url/pic/abc.gif` or by using `/url/pic/*` to perform a fuzzy search.
    -   If you set the delimiter to a forward slash \(/\), the raw log entry is split into the following three words: `url`, `pic`, and `abc.gif`. You can search for the log entry by using the keyword `url`, `abc.gif`, or `/url/pic/abc.gif`, or by using `pi*` to perform a fuzzy search.
    -   If you set the delimiter to a forward slash \(/\) and a period \(.\), the log entry is split into the following four words: `url`, `pic`, `abc`, and `gif`. |

6.  Click **OK**.

    The index configurations take effect within 1 minute.


## Configure field indexes

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the project in which you want to query and analyze logs.

3.  On the **Log Storage** \> **Logstores** tab, click the Logstore where logs are stored.

4.  On the Search & Analysis page of the Logstore, choose **Index Attributes** \> **Attributes**.

    If the indexing feature is not enabled, click **Enable**.

5.  Configure indexes in the Search & Analysis panel.

    |Parameter|Description|
    |:--------|:----------|
    |**Key Name**|The name of the log field, for example, client\_ip. **Note:**

    -   If you configure an index for a tag field \(for example, public IP address or UNIX timestamp\), you must set the **Key Name** parameter in the \_\_tag\_\_:KEY format. For example, you can set the parameter to \_\_tag\_\_:\_\_receive\_time\_\_. For more information, see [Reserved fields](/intl.en-US/Product Introduction/Limits/Reserved fields.md).
    -   In the Field Search section, you must set the **Type** parameter of a tag field to **text**. Numeric data types are not supported. |
    |**Type**|The data type of the log field value. Valid values: text, long, double, and json. For more information, see [Data types of indexes](/intl.en-US/Index and query/Data types of indexes.md). **Note:** The fields of the long or double type do not support the **Case Sensitive**, **Include Chinese**, and **Delimiter** parameters. |
    |**Alias**|The alias of the column, for example, ip. An alias is used only in analytic statements. You must use the original field name in search statements. For more information, see [Column aliases](/intl.en-US/Index and query/Analysis grammar/Column aliases.md). |
    |**Case Sensitive**|Specifies whether searches are case-sensitive.    -   If you turn on the **Case Sensitive** switch, searches are case-sensitive. For example, if a log entry contains `internalError`, you can search for the log entry only by using the keyword `internalError`.
    -   If you turn off the **Case Sensitive** switch, searches are not case-sensitive. For example, if a log entry contains `internalError`, you can search for the log entry by using the keyword `INTERNALERROR` or `internalerror`. |
    |**Delimiter**|The delimiters that are used to split the content of a log entry into multiple words. The delimiters include `,'";=()[]{}?@&>/:\n\t\r`, where `\n` indicates a line feed, `\t` indicates a tab character, and `\r` indicates a carriage return character.For example, the content of a log entry is `/url/pic/abc.gif`.

    -   If you do not specify a delimiter, the log entry is processed as single word in the `/url/pic/abc.gif` format. You can search for the log entry only by using the keyword `/url/pic/abc.gif` or by using `/url/pic/*` to perform a fuzzy search.
    -   If you set the delimiter to a forward slash \(/\), the raw log entry is split into the following three words: `url`, `pic`, and `abc.gif`. You can search for the log entry by using the keyword `url`, `abc.gif`, or `/url/pic/abc.gif`, or by using `pi*` to perform a fuzzy search.
    -   If you set the delimiter to a forward slash \(/\) and a period \(.\), the log entry is split into the following four words: `url`, `pic`, `abc`, and `gif`. |
    |**Include Chinese**|Specifies whether to distinguish between Chinese characters and English in searches.     -   After you turn on the **Include Chinese** switch, if a log entry contains Chinese characters, the Chinese content is split based on the Chinese syntax. The English content is split based on specified delimiters.

**Note:** When the Chinese content is split, the data write speed is reduced. Proceed with caution.

    -   If you turn off the **Include Chinese** switch, all content is split based on specified delimiters. |
    |**Enable Analytics**|After you turn on the **Enable Analytics** switch, you can use the analysis feature.|

6.  Click **OK**.

    The index configurations take effect within 1 minute.


