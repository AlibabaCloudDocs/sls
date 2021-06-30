# 接入Zabbix告警

Zabbix作为常用的开源监控系统，提供了丰富的告警规则用于系统监控，同时支持多种告警通知渠道。您可以将日志服务告警系统设为Zabbix的一个通知渠道，由日志服务告警系统完成告警降噪、通知等处理。

-   已创建开放告警应用。更多信息，请参见[配置开放告警对外接口](/cn.zh-CN/告警/告警（新版）/开放告警/配置开放告警对外接口.md)。
-   [下载alibaba\_cloud\_sls.yml](https://cloud-product-share.oss-cn-beijing.aliyuncs.com/open_event/alibaba_cloud_sls.yml)。

## Zabbix配置

**说明：** 只支持Zabbix 4.4及以上版本。

1.  登录Zabbix控制台。

2.  配置全局变量ZABBIX.SERVER.URL。

    配置后，全局变量ZABBIX.SERVER.URL将作为告警内容的一部分发送给日志服务。你可以在日志服务的告警通知中，单击对应的信息，跳转至Zabbix控制台，查看更多信息。

    如果您不设置全局变量ZABBIX.SERVER.URL的值，则在告警消息中显示默认值127.0.0.1。

    1.  在左侧导航栏中，选择**Administration** \> **General** \> **Macros**。

    2.  在Macros页面中，单击**Add**。

    3.  添加全局变量ZABBIX.SERVER.URL，值为Zabbix控制台的访问地址。

        ![Zabbix告警](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6802344261/p286457.png)

    4.  单击**Update**。

3.  添加**Alibaba Cloud SLS \(Log Service\)**通知渠道。

    1.  在左侧导航栏中，选择**Administration** \> **Media types**。

    2.  在Media types页面的右上角，单击**Import**。

    3.  在Import对话框中，选择您已下载的alibaba\_cloud\_sls.yml文件，选中**Update existing**，然后单击**Import**。

        ![导入alibaba_cloud_sls.yml](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8061684261/p286546.png)

    4.  在Media types页面中，单击**Alibaba Cloud SLS \(Log Service\)**。

    5.  在Parameters配置项中，修改**hook\_url**字段的值，然后单击**Update**。

        将**hook\_url**字段的值修改为您在日志服务中创建开放告警服务和应用后生成的接口信息（完整URL）。如何获取，请参见[获取接口信息](/cn.zh-CN/告警/告警（新版）/开放告警/配置开放告警对外接口.mdsection_098_gul_glw)。

        **说明：** 如果您的Zabbix运行在阿里云ECS上，则建议您在选择Zabbix告警消息接入地域时，选择ECS所在地域，并使用局域网或VPC域名。否则您选择任一地域的公网接口即可。

        ![hook_url](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8061684261/p286566.png)

4.  为目标用户设置通知渠道。

    1.  在左侧导航栏中，选择**Administration** \> **Users**。

    2.  在用户列表中，单击目标用户。

        您也可以单击**Create user**，创建一个新用户。

    3.  在Media页签中，单击其中一个Media对应的**Edit**。

        您也可以单击**Add**，创建一个新的Media。

    4.  在Media面板中，选择**Type**为Alibaba Cloud SLS \(Log Service\)，然后单击**Update**。

        ![通知渠道](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8061684261/p286615.png)

    5.  单击**Update**。

5.  配置触发器。

    1.  在左侧导航栏中，选择**Configuration** \> **Actions** \> **Trigger actions**。

    2.  在Trigger actions页面中，单击您已创建的触发器。

    3.  在Operations页签中，单击Operations区域中的Add。

        您也可以单击目标Operation对应的**Edit**。

        ![operation](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8061684261/p289681.png)

    4.  在Opetation details对话框中，选择您的目标用户或用户组，以及配置Send only to为Alibaba Cloud SLS \(Log Service\)，然后单击**Add**。

        ![添加operation](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8061684261/p289683.png)

    5.  单击**Update**。


## 告警消息解析

Zabbix告警消息中包含100多种变量。更多信息，请参见[Zabbix官方文档](https://www.zabbix.com/documentation/current/manual/appendix/macros/supported_by_location#indexed_macros)。日志服务只选取其中几十个消息变量组成告警消息。Zabbix告警消息内容示例如下：

|Zabbix Macro名称|示例值|
|--------------|---|
|\{TRIGGER.ID\}|19006|
|\{TRIGGER.NAME\}|test used|
|\{EVENT.UPDATE.STATUS\}|0|
|\{EVENT.VALUE\}|1|
|\{DATE\}|2021.06.10|
|\{TIME\}|12:44:23|
|\{EVENT.DATE\}|2021.06.10|
|\{EVENT.TIME\}|19:23:01|
|\{EVENT.RECOVERY.DATE\}|""|
|\{EVENT.RECOVERY.TIME\}|""|
|\{HOST.NAME\}|zabbix-agent|
|\{HOST.IP\}|192.0.2.0|
|\{TRIGGER.HOSTGROUP.NAME\}|Linux servers|
|\{EVENT.DURATION\}|20h 1m 31s|
|\{TRIGGER.DESCRIPTION\}|The system is running out of free memory.|
|\{EVENT.OPDATA\}|73.22 %|
|\{EVENT.TAGS\}|Application:Memory|
|\{NSEVERITY\}|2|
|\{EVENT.ID\}|1036|

## 字段映射

日志服务告警内容中的字段与Zabbix侧的映射关系如下：

|日志服务|Zabbix|说明|
|----|------|--|
|aliuid|无|用于接入告警的开放告警应用所属的阿里云账号ID|
|alert\_id|\{TRIGGER.ID\}|告警监控规则ID|
|alert\_type|无|告警类型，固定为sls\_pub。|
|alert\_name|\{TRIGGER.NAME\}|告警监控规则名称|
|status|\{EVENT.UPDATE.STATUS\}、\{EVENT.VALUE\}|告警状态。如果Zabbix中的\{EVENT.UPDATE.STATUS\}值和\{EVENT.VALUE\}值都为0，则表示resolved（告警恢复）。其余值都表示firing（触发告警）。 |
|next\_eval\_interval|无|告警评估时间间隔，固定为0。|
|alert\_time|无|告警本次评估时间。通过计算\{DATE\}和\{TIME\}所得。 |
|fire\_time|无|告警首次触发时间。通过计算\{EVENT.DATE\}和\{EVENT.TIME\}所得。 |
|resolve\_time|无|告警恢复时间。-   如果告警状态是firing，则值为0。
-   如果告警状态为resolved，则值为具体恢复时间。通过计算\{EVENT.RECOVERY.DATE\}和\{EVENT.RECOVERY.TIME\}所得。 |
|labels|\{HOST.NAME\}|告警标签信息。如果您在创建开放告警应用时 ，在**信息加工**中添加了标签信息，则此标签信息将被添加到labels字段中。

**说明：** 当您在**信息加工**中配置的标签的Key与Zabbix告警消息的tags字段中的子字段重复时，映射结果以您在**信息加工**中配置的为准。 |
|annotations|\{EVENT.TAGS\}|Zabbix告警被接入到日志服务后，日志服务会将\{EVENT.TAGS\}字段展开为多个键值对，添加到annotations字段中。-   \{HOST.IP\}映射为\_\_host\_ip\_\_。
-   \{TRIGGER.HOSTGROUP.NAME\}映射为\_\_host\_group\_name\_\_。
-   \{EVENT.DURATION\}映射为event\_duration。
-   \{EVENT.NAME\}映射为title。
-   \{TRIGGER.DESCRIPTION\}映射为desc。
-   \{EVENT.OPDATA\}映射为event\_opdata。

除以上字段外，还会额外添加如下字段。

-   \_\_config\_app\_\_: "sls\_pub\_alert"
-   \_\_pub\_alert\_service\_\_: \{开放告警服务id\}
-   \_\_pub\_alert\_app\_\_: \{开放告警应用id\}
-   \_\_pub\_alert\_protocol\_\_: "zabbix"
-   \_\_pub\_alert\_region\_\_: \{告警消息发送的endpoint对应的region\}

如果您在创建开放告警应用时 ，在**信息加工**中添加了标注信息，则此标注信息将被添加到annotations字段中。 |
|severity|\{NSEVERITY\}|告警严重度。更多信息，请参见[表 2](#table_p5k_cd7_maq)。|
|policy|无|您在开放告警应用中配置的告警策略。更多信息，请参见[Policy结构](/cn.zh-CN/告警/告警（新版）/通知管理/通知渠道/内容模板变量说明.md)。|
|project|无|告警中心所属的Project。更多信息，请参见[项目（Project）](/cn.zh-CN/产品简介/基本概念/项目（Project）.md)。|
|drill\_down\_query|\{$ZABBIX.SERVER.URL\}、\{TRIGGER.ID\}、\{EVENT.ID\}|链接形式，单击后可跳转到Zabbix的告警消息管理页面。|

|Severity（Zabbix）|Severity（日志服务）|
|----------------|--------------|
|Not Classified|report|
|Information|low|
|Warning|medium|
|Average|medium|
|High|high|
|Disaster|critical|

## 常见问题

如何查看操作日志？

1.  登录Zabbix控制台。

2.  在左侧导航栏中，选择**Reports** \> **Action log**。

3.  查看操作日志。

    ![操作日志](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1320205261/p290285.png)


