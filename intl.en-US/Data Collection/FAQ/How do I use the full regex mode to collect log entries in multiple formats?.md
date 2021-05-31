# How do I collect various formats of logs in complete regular mode?

The complete regular mode requires format consistency among all logs. However, some logs may contain content in multiple formats. In this case, you can use the Schema-On-Write or Schema-On-Read mode to process the logs.

For example, a Java log is a program log that contains both correct information and error information \(such as information about abnormal stacks\), including:

-   Multi-line WARNING logs
-   Simple text INFO logs
-   Key-value DEBUG logs

```
[2018-10-01T10:30:31,000] [WARNING] java.lang.Exception: another exception happened
    at TestPrintStackTrace.f(TestPrintStackTrace.java:3)
    at TestPrintStackTrace.g(TestPrintStackTrace.java:7)
    at TestPrintStackTrace.main(TestPrintStackTrace.java:16)
[2018-10-01T10:30:32,000] [INFO] info something
[2018-10-01T10:30:33,000] [DEBUG] key:value key2:value2
```

You can use the following modes to process such logs:

-   Schema-On-Write: In this mode, Logtail applies multiple Logtail Configs with different regular settings to a log so that correct fields can be extracted.

    **Note:** Logtail cannot apply multiple Logtail Configs to a file. Therefore, you need to set up multiple symbolic links for the directory where the file is stored. Then, each Logtail Config works on a symbolic link, thereby allowing you to aggregate multiple Logtail Configs to collect the file at the same time.

-   Schema-on-read: In this mode, you need to use the common regular expressions of the multi-format logs.

    For example, for collection of a multi-line log, you can use the time and log level as line start regular expressions and the residual parts as the message field. If you want to analyze the message field, you can set up an index for it, extract the required content, and then analyze the content based on query and analysis functions provided by Log Service, such as regular expression extraction.

    **Note:** This mode is recommended only when you need to analyze at a small-scale \(for example, tens of millions\) of logs.


