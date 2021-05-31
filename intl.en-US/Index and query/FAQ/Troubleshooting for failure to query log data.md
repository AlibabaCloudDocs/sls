# What can I do if no log data is found?

When using LogSearch of Log Service to query data, you can troubleshoot the problem in the following ways if no log data is found.

## 1. Log collection failure

If log data fails to be collected by Log Service, the target log cannot be queried. In this case, check whether log data is available on the consumption preview page of the target Logstore.

If it is available, log data is collected by Log Service.

If it is unavailable, possible causes are as follows:

-   The log source does not generate log data.

    In this case, no logs can be sent to Log Service. Check your log source.

-   Logtail has no heartbeat.

    On the Server Group Settings page, check whether the relevant server has a heartbeat in the Server Group Status section. If it has no heartbeat, troubleshoot the problem. For more information, see [What can I do if the Logtail client has no heartbeat?](/intl.en-US/Data Collection/FAQ/Troubleshooting for Logtail machine group without heartbeat in the log Service Console.md).

-   The monitoring file is not written in real time.

    If the monitoring file is written in real time, you can open /usr/local/ilogtail/ilogtail.LOG to view the error message. Common error messages are as follows:

    -   parse delimiter log fail: The error message returned because an error has occurred when Log Service collects logs in delimiter mode.
    -   parse regex log fail: The error message returned because an error has occurred when Log Service collects logs in regex mode.

## 2. Incorrect word-breaking settings

View the configured delimiters. Check whether you can use a keyword to search for a log after the log content is split by using a delimiter. For example, the delimiters are `, ; = ( ) [ ] { } ? @ & < > / : '` by default. If a log contains abc"defg,hij, it is split into abc"defg and hij. In this case, you cannot find this log by searching for abc.

Fuzzy search is also supported. For more information, see [Search syntax](/intl.en-US/Index and query/Query/Search syntax.md).

**Note:**

-   To save your indexing cost, Log Service has optimized the indexing feature. If you create an index on a field, full-text indexing is ineffective for the key of this field. For example, an index is created on the field whose key is message in a log, and a space is used as a delimiter. To use a space as a delimiter, enclose it in a word-breaking string. You can find the log that contains "message: this is a test message" by searching for the message:this keyword in the format of key:value. However, you cannot find the log by searching for the this keyword because an index is created on the message field and full-text indexing is ineffective.
-   You can create indexes or modify existing indexes. However, new or modified indexes take effect only for new data.

    You can click Index Attributes to check whether the configured delimiters meet the requirements.


## 3. Other causes

If log data is available, modify the time range of data query and try again. Log Service allows you to preview log data in real time. Due to a maximum latency of 1 minute, we recommend that you query log data at least 1 minute after logs are generated.

If your problem persists, submit a ticket.

