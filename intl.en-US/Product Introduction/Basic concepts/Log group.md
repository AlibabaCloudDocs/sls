# Log group

A log group is a group of logs that serves as a basic unit for read/write operations. The logs in a log group have the same metadata, such as the IP address and log source.

When you write logs to or read logs from Log Service, multiple logs are encapsulated into a log group. Then, you can write and read logs by log group. This method can reduce the number of read and write operations and improve business efficiency. The maximum length of a log group is 5 MB.

![Log group](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9248025261/p2377.png)

