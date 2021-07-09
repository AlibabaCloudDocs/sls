# 使用Terraform配置日志审计

本文介绍如何使用Terraform调用接口配置日志审计服务。

已安装和配置Terraform。具体操作，请参见[在Cloud Shell中使用Terraform]()、[在本地安装和配置Terraform]()。

Terraform是一种开源工具，用于安全高效地预览、配置和管理云基础架构和资源。Terraform的命令行接口（CLI）提供了一种简单机制，用于将配置文件部署到阿里云或其他任意支持的云上，并对其进行版本控制。

阿里云是中国国内第一家与Terraform集成的云厂商。目前[terraform-provider-alicloud](https://www.terraform.io/docs/providers/alicloud/index.html)已经提供了超过163个Resource和113个Data Source，覆盖计算、存储、网络、负载均衡、CDN、容器服务、中间件、访问控制和数据库等阿里云产品，满足大量大客户的自动化上云需求。

## 使用Terraform的优势

-   将基础结构部署到多个云

    Terraform适用于多云方案，将类似的基础结构部署到阿里云、其他云厂商或者本地数据中心。开发人员能够使用相同的工具和相似的配置文件同时管理不同云厂商的资源。

-   自动化管理基础结构

    您可以使用Terraform创建配置文件模板，用于重复、可预测的方式定义、预配和配置ECS资源，减少因人为因素导致的部署和管理错误。您可以多次部署同一模板，创建相同的开发、测试和生产环境。

-   基础架构即代码（Infrastructure as Code）

    Terraform支持通过代码来管理、维护资源，允许保存基础设施状态，从而使您能够跟踪对系统（基础设施即代码）中不同组件所做的更改，并与其他人共享这些配置 。

-   降低开发成本

    您通过按需创建开发和部署环境来降低成本。并且，您可以在系统更改之前进行评估。


## 步骤一：配置身份信息以及日志审计服务的中心化地域

在环境变量中配置用户身份信息以及日志审计服务的中心Project所在地域。

```
export ALICLOUD_ACCESS_KEY="LTAIUrZCw3****"
export ALICLOUD_SECRET_KEY="zfwwWAMWIAiooj14GQ2****"
export ALICLOUD_REGION="cn-huhehaote"
```

|参数|说明|
|--|--|
|ALICLOUD\_ACCESS\_KEY|阿里云访问密钥AccessKey ID。更多信息，请参见[访问密钥](/intl.zh-CN/开发指南/API参考/访问密钥.md)。|
|ALICLOUD\_SECRET\_KEY|阿里云访问密钥AccessKey Secret。更多信息，请参见[访问密钥](/intl.zh-CN/开发指南/API参考/访问密钥.md)。|
|ALICLOUD\_REGION|日志审计服务的中心Project所在地域。目前支持如下地域：-   中国：华北2（北京）、华北5（呼和浩特）、华东1（杭州）、华东2（上海）、华南1（深圳）
-   海外：新加坡、日本（东京） |

## 步骤二：RAM授权

使用Terraform完成RAM授权。具体操作，请参见[alicloud\_ram\_policy](https://www.terraform.io/docs/providers/alicloud/r/ram_policy.html)。在授权中所涉及的权限策略信息请参见[手动授权日志采集与同步](/intl.zh-CN/应用中心（App）/日志审计服务/手动授权日志采集与同步.md)。

## 步骤三：配置日志采集

1.  创建一个Terraform工作目录sls，并在该目录下创建一个名为terraform.tf的文件。

2.  在terraform.tf文件中，添加如下内容。

    ```
    resource "alicloud_log_audit" "example" {
      display_name = "tf-audit-test"
      aliuid       = "12345678"
    }
    ```

    相关参数说明如下：

    |参数|说明|
    |--|--|
    |example|Resource名称。自定义配置。|
    |display\_name|采集配置名称。自定义配置。|
    |aliuid|阿里云账号ID。|

3.  在sls目录下，执行如下命令，初始化terraform工作目录。

    ```
    terraform init
    ```

    如果返回结果中提示`Terraform has been successfully initialized!`，表示初始化成功。

    ![初始化](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6431465261/p292179.png)

4.  编辑terraform.tf文件，配置日志审计服务相关参数。

    配置示例如下。Terraform中日志审计采集配置的完整参数说明，请参见[Terraform-Aliyun Log Audit](https://registry.terraform.io/providers/aliyun/alicloud/latest/docs/resources/log_audit#example-usage)。

    -   单账号采集

        ```
        resource "alicloud_log_audit" "example" {
          display_name = "tf-audit-test"
          aliuid       = "12345678"
          variable_map = {
            "actiontrail_enabled" = "true",
            "actiontrail_ttl" = "180"
          }
        }
        ```

    -   多账号采集

        ```
        resource "alicloud_log_audit" "example" {
          display_name = "tf-audit-test"
          aliuid       = "12345678"
          variable_map = {
            "actiontrail_enabled" = "true",
            "actiontrail_ttl" = "180"
          }
          multi_account = ["123456789123", "12345678912300123"]
        }
        ```

    |参数|说明|
    |--|--|
    |actiontrail\_enabled|是否开启操作审计（Actiontrail）日志的采集，取值：    -   true：开启。
    -   false：关闭。 |
    |actiontrail\_ttl|设置操作审计日志的存储时间。|
    |multi\_account|多账号采集时，需配置多个阿里云账号ID。|

5.  使terraform.tf文件中的采集配置生效。

    1.  执行如下命令。

        ```
        terraform apply
        ```

    2.  输入yes。

        如果返回结果中提示`Apply complete!`，表示应用采集配置成功，日志审计服务将按照采集配置进行日志采集和存储。

        ![配置生效](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2166365261/p292102.png)


## 相关操作

您还可以通过Terraform完成如下相关操作。

-   导入已有审计配置。

    ```
    terraform import alicloud_log_audit.exampletf-audit-test
    ```

    其中，example和tf-audit-test，请根据实际情况替换。

    ![导入配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2166365261/p292108.png)

    执行完毕后，您可以查看terraform工作目录下的terraform.tfstate文件内容。terraform.tfstate文件内容即为导入的采集配置。

    **说明：**

    -   如果您想要将导入的采集配置迁移到terraform.tf文件中，需要手动拷贝，并对格式做适当调整，满足terraform.tf文件的格式要求。
    -   如果您已经在当前的terraform工作目录执行过terrraform apply或者terraform import命令，则此时再次执行terraform import命令会失败。您需要删除当前目录下的terraform.tfstate文件后再重新执行terraform import命令。
-   查看当前采集配置。

    ```
    terraform show
    ```

    ![查看审计配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2166365261/p292111.png)

-   查看当前terraform工作目录下的terraform.tf文件与已生效的采集配置的差异。

    ```
    terraform plan
    ```

    ![配置文件](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7431465261/p292113.png)


