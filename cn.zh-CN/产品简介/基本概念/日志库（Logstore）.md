# 日志库（Logstore）

日志库（Logstore）是日志服务中日志数据的采集、存储和查询单元。

每个Logstore隶属于一个Project，每个Project中可创建多个Logstore。您可以根据实际需求在目标Project中创建多个Logstore，一般是为同个应用中不同类型的日志创建独立的Logstore。例如您要采集App A所涉及的操作日志（operation\_log）、应用程序日志（application\_log）以及访问日志（access\_log），您可以创建一个名为app-a的Project，并在该Project下创建名为operation\_log、application\_log和access\_log的Logstore，用于分别存储操作日志、应用程序日志和访问日志。

您在执行写入日志、查询和分析日志、加工日志、消费日志、投递日志等操作时，都需要指定Logstore。具体说明如下：

-   以Logstore为采集单元，采集日志。
-   以Logstore为存储单元，存储日志以及执行加工、消费、投递等操作。
-   在Logstore中建立索引，用于查询和分析日志。

