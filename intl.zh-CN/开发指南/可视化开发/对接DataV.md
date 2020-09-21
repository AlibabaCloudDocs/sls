# 对接DataV

DataV是阿里云的可视化产品，能帮助您通过图形化的界面轻松搭建专业水准的可视化应用，丰富表现日志分析数据。本文档介绍如何通过日志服务对接DataV进行大屏数据展示。

-   已采集日志数据，详情请参见[数据采集](/intl.zh-CN/数据采集/采集方式.md)。
-   已开启并配置索引，详情请参见[开启并配置索引](/intl.zh-CN/查询与分析/开启并配置索引.md)。

实时大屏广泛应用于大型在线促销活动。实时大屏基于流式计算架构，该架构包含以下模块：

-   数据采集：将来自各源头数据实时采集。
-   中间存储：利用类Kafka Queue进行生产系统和消费系统解耦。
-   实时计算：数据处理关键环节，订阅实时数据，通过计算规则对窗口中数据进行运算。
-   结果存储：计算结果数据存入SQL和NoSQL。
-   可视化：通过API调用结果数据进行展示。

在阿里集团内，有大量成熟的产品可以完成此类工作，一般可供选型的产品如下：

![大屏展示相关产品](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4227617951/p5861.png)

日志服务支持通过日志服务查询分析API直接对接DataV进行大屏数据展示。

![日志服务对接DataV](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4227617951/p5862.png)

## 功能特点

计算方式根据数据量、实时性和业务需求会分为以下两种。

-   实时计算（流计算）：固定的计算+变化的数据
-   离线计算（数据仓库+离线计算）：变化的计算+固定的数据

在对实时性有要求的日志分析场景中，日志服务为您提供实时索引LogHub中数据机制，可通过LogSearch/Anlaytics直接进行查询分析。这种方式具有以下优势：

-   快速：一秒内查询（5个条件），可处理10亿级数据。一秒内分析（5个维度聚合+GroupBy），可聚合亿级别数据，无需等待和预计算结果。
-   实时：99.9%情况下可做到日志产生1秒内反馈到大屏。
-   动态：无论修改统计方法还是补数据，支持实时刷新显示结果，无需等待重新计算。

这种方式具有以下限制：

-   数据量：单次计算数据量限制为百亿行，当超过百亿行，需要限定时间段。
-   计算灵活度：计算限于SQL92语法，不支持自定义UDF。

## DataV配置步骤

1.  创建DataV数据源。

    1.  登录[DataV数据可视化控制台](https://datav.aliyun.com/)。

    2.  在我的数据页签，单击**添加数据**，具体参数配置如下：

        |配置项|说明|
        |:--|:-|
        |类型|在下拉菜单中选择**简单日志服务 SLS**。|
        |自定义数据源名称|为数据源配置一个自定义名称。例如log\_service\_api。|
        |AppKey|主账号的AccessKey ID，或者有权限读取日志服务的RAM用户 AccessKey ID。|
        |AppSecret|主账号的AccessKey Secret，或者有权限读取日志服务的RAM用户AccessKey Secret。|
        |Endpoint|填写日志服务Project所在地域的Endpoint，请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。|

2.  打开可视化画布。

    -   在我的可视化页签，选择已存在目标可视化项目，单击**编辑**。
    -   在我的可视化页签，单击**新建可视化**，创建步骤请参见[使用模板创建PC端可视化应用](/intl.zh-CN/管理可视化应用/创建可视化应用.md)。
3.  创建折线图并添加过滤器。

    1.  创建一个折线图。

        在左侧组件列表中，单击**折线图** \> **基本折线图**，在右侧数据页签**数据源**项单击**配置数据源**修改组件样式配置，具体配置如下：

        |配置项|说明|
        |---|--|
        |数据源类型|选择**简单日志服务 SLS**。|
        |选择已有数据源|选择[步骤1](#step_yam_6sd_nm5)创建的数据源log\_service\_api。|
        |查询|查询示例如下：        ```
{
 "projectName": "dashboard-demo",
 "logStoreName": "access-log",
 "topic": "",
 "from": ":from",
 "to": ":to",
 "query": "*| select approx_distinct(remote_addr) as uv ,count(1) as pv , date_format(from_unixtime(date_trunc('hour',__time__) ) ,'%Y/%m/%d %H:%i:%s')   as time group by time  order by time limit 1000" ,
 "line": 100,
 "offset": 0
}
        ``` |

        查询示例参数说明：

        |配置项|说明|
        |:--|:-|
        |projectName|Project名称。|
        |logstoreName|Logstore名称。|
        |topic|日志主题，如果您没有设置日志主题，此处请留空。|
        |from、to|from和to分别是日志的起始和结束时间。**说明：** 示例中填写的是`:from`和`:to`。在测试时，您可以先填写unix time，例如1509897600。发布之后换成`:from`和`:to`，然后您可以在URL参数里控制这两个数值的具体时间范围。例如，预览时的URL是`http://datav.aliyun.com/screen/86312`，打开`http://datav.aliyun.com/screen/86312?from=1510796077&to=1510798877`后，会按照指定的时间进行计算。 |
        |query|查询条件。query的语法请参见[实时分析简介](/intl.zh-CN/查询与分析/实时分析简介.md)。**说明：** query中的时间格式，一定要是2017/07/11 12:00:00这种，所以采用以下方式把时间对齐到整点，再转化成目标格式。

        ```
date_format(from_unixtime(date_trunc('hour',__time__) ) ,'%Y/%m/%d%H:%i:%s')
        ``` |
        |line|请填写默认值100。|
        |offset|请填写默认值0。|

    2.  配置完成后，单击**查看数据响应结果**。

    3.  新建过滤器。

        选中**数据过滤器**，然后单击**添加过滤器**后的加号（+）新建一个过滤器。

        过滤器内容请按照以下格式填写。

        ```
        return Object.keys(data).map((key) => {
        let d= data[key];
        d["pv"] = parseInt(d["pv"]);
        return d;
        });
        ```

        -   在过滤器中，要把y轴用到的结果显示为int类型，上述样例中，y轴为pv，所以需要转换pv列。
        -   可以看到在结果中有t和pv两列，您可以将x轴配置为t，y轴配置成pv。
4.  创建饼状图并添加过滤器。

    1.  新建**轮播饼图**。

        在左侧组件列表中，单击选择**饼图** \> **轮播饼图**，在右侧数据页签**数据源**项单击**配置数据源**修改组件样式配置，具体配置如下：

        |配置项|说明|
        |---|--|
        |数据源类型|选择**简单日志服务 SLS**。|
        |选择已有数据源|选择[步骤1](#step_yam_6sd_nm5)创建的数据源log\_service\_api。|
        |查询|查询示例如下：        ```
{
 "projectName": "dashboard-demo",
 "logStoreName": "access-log",
 "topic": "",
 "from": 1509897600,
 "to": 1509984000,
 "query": "*| select count(1) as pv ,method group by method" ,
 "line": 100,
 "offset": 0
}
        ```

示例参数说明请参见折线图参数说明。|

    2.  配置完成后，单击**查看数据响应结果**。

    3.  添加一个过滤器。

        选中**数据过滤器**，然后单击**添加过滤器**后的加号（+）新建一个过滤器。

        过滤器内容请按照以下格式填写。

        ```
        return Object.keys(data).map((key) => {
        let d= data[key];
        d["pv"] = parseInt(d["pv"]);
        return d;
        })
        ```

        饼图的type配置为method，value配置为pv。

5.  回调ID动态获取时间范围，以下步骤演示如何动态的显示15分钟的日志。

    1.  创建一个静态数据源，使用值默认即可，添加一个过滤器。代码示例如下：

        ```
        return [{
          value:Math.round(Date.now()/1000)
        }];
        ```

        ```
        return [{
          value:Math.round((Date.now() - 24 * 60 * 60 * 1000)/1000)
        }];
        ```

    2.  在交互页签，选中**启用**对应的响应事件，并将value绑定到变量中。

        ![001](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6124843951/p111293.png)

    3.  在数据视图，通过`:from`和`:to`引用回调ID，示例如下：

        ```
        {
         "projectName": "dashboard-demo",
         "logStoreName": "access-log",
         "topic": "",
         "from": ":from",
         "to": ":to",
         "query": "*| select count(1) as pv ,referer group by pv desc limit 30" ,
         "line": 100,
         "offset": 0
        }
        ```

6.  预览和发布。

    1.  单击**预览**图标对已编辑的视图进行预览。

    2.  单击**发布**图标即完成视图发布。


## 案例：调整不同统计口径下的云栖大会网站访问实时大屏

云栖大会期间有一个临时需求，需要您统计大会网站的全国各地访问量以在实时大屏上显示。此前您已配置采集全量日志数据，并且在日志服务中打开了查询分析，所以只要输入查询分析Query即可。在此过程中，需求是不断调整的，如下所列：

-   原始需求：在云栖大会的第一天，您需要统计UV（当日点击用户数量）。

    您要查询所有访问日志中nginx下forward字段的数据（该字段记录访问用户的一个或多个IP，每条日志一个forward字段），通过`approx_distinct(forward)`计算去重后的IP地址数量，获取从云栖大会首日零时到当前时刻的点击UV数，可以使用如下语句。

    ```
    * | select approx_distinct(forward) as uv
    ```

-   需求第一次调整：云栖大会的第二天，需求调整为您需要统计yunqi.aliyun.com这个域名下的用户访问量数据。

    您可以增加一个过滤条件host进行实时查询，使用如下语句。

    ```
    host:yunqi.aliyun.com | select approx_distinct(forward) as uv
    ```

-   需求第二次调整：在统计过程中，您发现Nginx访问日志forward字段存在多个IP，您默认只要第一个IP。

    使用如下语句。

    ```
    host:yunqi.aliyun.com | select approx_distinct(split_part(forward,',',1)) as uv
    ```

-   需求第三次调整：云栖大会的第三天，需求被加上限制条件，您需要剔除通过UC浏览器访问并点击该浏览器广告而来的用户访问量，统计非UC浏览器广告导流、不重复IP的全国各地用户访问量。

    此时您可以加上一个过滤条件not，使用如下语句。

    ```
    host:yunqi.aliyun.com not URL:uc-iflow  | select approx_distinct(split_part(forward,',',1)) as uv
    ```


