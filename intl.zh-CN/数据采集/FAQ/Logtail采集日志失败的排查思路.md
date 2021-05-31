# Logtail排查简介

配置Logtail采集日志后，如果预览页面为空，或查询页面显示无数据，可以根据本文步骤进行排查。

1.  确认机器组内Logtail心跳是否正常。

    在日志服务控制台查看Logtail机器组心跳状态，详细步骤请参考[查看状态](/intl.zh-CN/数据采集/Logtail采集/机器组/管理机器组.md)。

    如果心跳OK则直接进入下一步；如果心跳FAIL则需要进一步排查。详细排查步骤请参考[Logtail 机器无心跳]()。

2.  确认是否创建了Logtail采集配置。

    确认Logtail客户端状态正常后，则需要确认是否已创建Logtail配置。务必确认日志监控目录和日志文件名与机器上文件相匹配。目录结构支持完整路径和通配符两种模式。

3.  确认Logtail配置是否已应用到机器组。

    查看[Logtail机器组](/intl.zh-CN/数据采集/Logtail采集/机器组/管理机器组.md)，选择**管理配置**，确认目标配置是否已经应用到机器组。

4.  检查采集错误。

    确认Logtail配置正常后，您需要确认日志文件是否实时产生新数据。Logtail只采集增量数据，存量文件如果没有更新则不会被读取。如日志有更新但未在日志服务中查询到，您可以通过以下方式诊断。

    -   **诊断采集错误**

        进行[诊断采集错误]()，根据Logtail上报的错误类型处理。

    -   **查看Logtail日志**

        客户端日志会记录关键INFO以及所有WARNING、ERROR日志。如果想了解更为实时完整的错误，可以在以下路径查看客户端日志：

        -   Linux：/usr/local/ilogtail/ilogtail.LOG
        -   Linux：/usr/local/ilogtail/logtail\_plugin.LOG （http、mysql binlog、mysql 查询结果等输入源的日志记录）
        -   Windows x64 : C:\\Program Files \(x86\)\\Alibaba\\Logtail\\logtail\_\*.log
        -   Windows x32 : C:\\Program Files\\Alibaba\\Logtail\\logtail\_\*.log
    -   **用量超限**

        如果有大日志量或者大文件数据量的采集需求，可能需要修改Logtail的启动参数，以达到更高的日志采集吞吐量。参考[设置Logtail启动参数](/intl.zh-CN/数据采集/Logtail采集/安装/设置Logtail启动参数.md)。

    在完成以上三步检查后如问题仍未解决，请工单联系日志服务，并附上排查过程中发现的关键信息。


