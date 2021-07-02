# Data read and write

This topic describes the limits of data read and write in Log Service.

|Resource|Limit|Description|Note|
|:-------|:----|:----------|:---|
|Project|Write traffic|The maximum write traffic is 30 GB per minute.|If the limit is reached, the status code 403 and the "Inflow Quota Exceed" error message is returned. If you want to raise the limit, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm?spm=a2796.7919406.0.dcontactus3.676a2d23RjosdV#/ticket/add/?productId=1210).|
|Number of writes|The maximum number of write operations is 600,000 per minute.|If the limit is reached, the status code 403 and the error message "Write QPS Exceed" is returned. If you need to raise the limit, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm?spm=a2796.7919406.0.dcontactus3.676a2d23RjosdV#/ticket/add/?productId=1210).|
|Number of reads|The maximum number of read operations is 600,000 per minute.|If the limit is reached, the status code 403 and the error message "Read QPS Exceed" is returned. If you need to raise the limit, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm?spm=a2796.7919406.0.dcontactus3.676a2d23RjosdV#/ticket/add/?productId=1210).|
|Shard|Write traffic|The maximum write traffic is 5 MB/s.|Not required. If the limit is reached, Log Service continues to write data. However, the quality of the service may be degraded.|
|Number of writes|The maximum number of write operations is 500 per second.|Not required. If the limit is reached, Log Service continues to write data. However, the quality of the service may be degraded.|
|Read Traffic|The maximum read traffic is 10 MB/s.|Not required. If the limit is reached, Log Service continues to read data. However, the quality of the service may be degraded.|
|Number of reads|The maximum number of read operations is 100 per second.|Not required. If the limit is reached, Log Service continues to read data. However, the quality of the service may be degraded.|

