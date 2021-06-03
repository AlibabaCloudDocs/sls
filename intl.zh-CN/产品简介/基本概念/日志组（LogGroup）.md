# 日志组（LogGroup）

日志组（LogGroup）是一组日志的集合，是写入与读取日志的基本单元。一个日志组中的数据包含相同Meta（IP地址、Source等信息）。

写入日志到日志服务或从日志服务读取日志时，多条日志被打包为一个日志组，以日志组为单元进行写入与读取。该方式可减少读写次数，提高业务效率。每个日志组最大长度为5 MB。

![日志组](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5287751261/p2377.png)

