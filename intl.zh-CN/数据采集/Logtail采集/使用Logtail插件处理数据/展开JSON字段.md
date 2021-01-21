# 展开JSON字段

您可以使用processor\_json插件展开JSON字段。本文介绍processor\_json插件的参数说明和配置示例。

## 参数说明

配置type为processor\_json，detail说明如下表所示。

**说明：** Logtail 0.16.28及以上版本支持该插件。

|参数|类型|是否必选|说明|
|:-|:-|:---|:-|
|SourceKey|String|是|原始字段名。|
|NoKeyError|Boolean|否|无匹配字段时是否报错。如果未添加该参数，则默认使用true，表示报错。|
|ExpandDepth|Int|否|JSON展开的深度。如果未添加该参数，则默认为0，表示不限制。1表示当前层级，以此类推。|
|ExpandConnector|String|否|JSON展开时的连接符，可以为空。如果未添加该参数，则默认使用下划线（\_）。|
|Prefix|String|否|JSON展开时对字段附加的前缀。如果未添加该参数，则默认为空。|
|KeepSource|Boolean|否|是否保留原始字段。如果未添加该参数，则默认使用true，表示保留。|
|UseSourceKeyAsPrefix|Boolean|否|是否将原始字段名作为所有JSON展开字段名的前缀。如果未添加该参数，则默认使用false，表示否。|

## 配置示例

对s\_key字段进行JSON展开，并使用j和原始字段名s\_key作为JSON展开后字段名的前缀。配置示例如下：

-   原始日志

    ```
    "s_key":"{\"k1\":{\"k2\":{\"k3\":{\"k4\":{\"k51\":\"51\",\"k52\":\"52\"},\"k41\":\"41\"}}})"
    ```

-   Logtail插件处理配置

    ```
    {
      "processors":[
        {
          "type":"processor_json",
          "detail": {
            "SourceKey": "s_key",
            "NoKeyError":true,
            "ExpandDepth":0,
            "ExpandConnector":"-",
            "Prefix":"j",
            "KeepSource": false,
            "UseSourceKeyAsPrefix": true
          }
        }
      ]
    }
    ```

-   处理结果

    ```
    "s_key":"{\"k1\":{\"k2\":{\"k3\":{\"k4\":{\"k51\":\"51\",\"k52\":\"52\"},\"k41\":\"41\"}}})"
    "js_key-k1-k2-k3-k4-k51":"51"
    "js_key-k1-k2-k3-k4-k52":"52"
    "js_key-k1-k2-k3-k41":"41"
    ```


