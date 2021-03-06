# Topic

A topic is a basic management unit in Log Service. When you collect logs, you can specify topics to identify the logs.

You can use topics to identify logs generated by different services, users, and instances. For example, if system A consists of an HTTP request processing module, a cache module, a logic processing module, and a storage module. You can set the log topic of the HTTP request processing module to http\_module, the log topic of the cache module to cache\_module, the log topic of the logic processing module to logic\_module, and the log topic of the storage module to store\_module. When the logs of the preceding modules are collected to the same Logstore, you can identify logs based on the topics.

If you do not need to identify logs in a Logstore, set the topic to Null-Do not generate topic when you collect logs. A topic can be an empty string, which indicates that the value of the topic is an empty string.

The following figure shows the relationships between Logstores, topics, and shards.

![Topic](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9548275851/p2389.png)

