# 授予RAM用户告警操作权限

本文档介绍如何创建阿里云RAM用户并授予其告警操作权限。

您可以通过如下两种方式给RAM用户授予告警操作的权限。

-   极简授权：权限较大，操作简单。
    -   使用阿里云主账号登录[RAM控制台](https://ram.console.aliyun.com/)，为RAM用户授予全部管理权限（AliyunLogFullAccess）。更多信息，请参见[创建RAM用户及授权](/cn.zh-CN/开发指南/访问控制RAM/创建RAM用户及授权.md)。
    -   如果允许RAM用户使用内置角色或者自定义角色查询时序库或日志库，还需要给RAM授权角色对应的`ram:PassRole`权限，权限策略如下：

        ```
        {
             "Action": "ram:PassRole",
             "Effect": "Allow",
             "Resource": "acs:ram::主账号:role/角色名ARN"
         }
        ```

        创建内置角色或者自定义角色授权，请参见[配置访问控制]()。

-   自定义权限策略：权限精细，配置复杂。

    本文以此方式为例进行授权操作。


1.  登录[RAM 控制台](https://ram.console.aliyun.com/)。

2.  创建权限策略。

    1.  在左侧导航栏中，选择**权限管理** \> **权限策略管理**。

    2.  单击**创建权限策略**。

    3.  在新建自定义权限策略页面中，配置如下参数，并单击**确定**。

        |参数|描述|
        |--|--|
        |**策略名称**|配置策略名称。|
        |**配置模式**|选择**脚本配置**。|
        |**策略内容**|将配置框中的原有脚本替换为如下内容。         -   请根据实际情况替换Project名称。
        -   `sls-alert-*`为当前阿里云账号的全局告警中心Project，格式为`sls-alert-${uid}-${region}`，里面包含该账号下所有告警的规则排障报表。
        ```
{ 
    "Version": "1", 
    "Statement": [
   {
        "Effect": "Allow",
     "Action": [
       "log:CreateLogStore",
       "log:CreateIndex",
       "log:UpdateIndex"
     ],
     "Resource": [
       "acs:log:*:*:project/Project名称/logstore/internal-alert-history",
       "acs:log:*:*:project/sls-alert-*/logstore/internal-alert-center-log"
     ]   
     },
   {
     "Effect": "Allow",
     "Action": [
       "log:CreateDashboard",
       "log:CreateChart",
       "log:UpdateDashboard"
     ],
     "Resource": "acs:log:*:*:project/Project名称/dashboard/*"
   },
   {
     "Effect": "Allow",
     "Action": [
       "log:*"
     ],
     "Resource": "acs:log:*:*:project/Project名称/job/*"   
     },
   {
     "Effect": "Allow",
     "Action": [
       "log:CreateProject"
     ],
     "Resource": [
       "acs:log:*:*:project/sls-alert-*"
     ]
   },
   {
     "Action": "ram:PassRole",
     "Effect": "Allow",
     "Resource": "acs:ram::主账号:role/角色名ARN"
   }
 ]
}
        ``` |

3.  创建RAM用户。

    如何创建RAM用户，请参见[创建RAM用户](/cn.zh-CN/开发指南/访问控制RAM/创建RAM用户及授权.mdsection_wz1_e6j_bdy)。

4.  为RAM用户授权。

    1.  在左侧导航栏中，选择**人员管理** \> **用户**。

    2.  找到目标RAM用户，单击**添加权限**。

    3.  单击**系统策略**，选中**AliyunRAMReadOnlyAccess**。

    4.  单击**自定义策略**，选中[步骤2](#step_pzo_yvb_cmr)中创建的策略，单击**确定**。

    5.  单击**完成**。


