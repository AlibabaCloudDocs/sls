# Obtain the IP2Location library from OSS and enrich IP address data

You can use the data transformation feature of Log Service to obtain the IP2Location library from Object Storage Service \(OSS\) and use the library to identify the city, state, and country to which an IP address belongs.

-   AccessKey pairs are created to access an OSS bucket. For more information, see [Create an AccessKey pair for a RAM user](/intl.en-US/Security Settings/AccessKey pairs/Create an AccessKey pair for a RAM user.md).

    We recommend that you create an AccessKey pair that has read-only permissions on the OSS bucket and an AccessKey pair that has write-only permissions on OSS. This way, you can use the first AccessKey pair to read data from the OSS bucket and use the second AccessKey pair to write data to the OSS bucket. For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Access and control/RAM Policy/Overview.md).

-   The IP2Location library is downloaded from the IPIP.NET website and uploaded to OSS. For more information, see [Upload objects](/intl.en-US/Quick Start/OSS console/Upload objects.md).

    We recommend that you use an AccessKey pair that has write-only permissions on OSS to upload the library.


IP2Location provides a global IP address library that allows you to identity geographic locations of IP addresses around the world. You can download the IP address library from the IP2Location website and upload the library to OSS. If you want to identify the city, state, and country to which an IP address belongs during data transformation, you can use the [res\_oss\_file](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.md) function to obtain the IP2Location library from OSS. Then you can use the [geo\_parse](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Structured data functions.md) function to parse IP addresses and use the [e\_set](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value assignment function.md) function to add new fields to log entries.

## Examples

-   Raw log entry:

    ```
    ip: 19.0.0.0
    ```

-   Transformation rule:

    ```
    e_set("geo", geo_parse(v("ip"), ip_db=res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',
                                                        ak_id=res_local("AK_ID"),
                                                        ak_key=res_local("AK_KEY"),
                                                        bucket='test', 
                                                        file='your ip2location bin file', 
                                                        format='binary', change_detect_interval=20),
                            provider="ip2location"))
    e_json("geo")
    ```

    The following table describes the fields in the res\_oss\_file function.

    |Field|Description|
    |-----|-----------|
    |endpoint|The endpoint of OSS. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).|
    |ak\_id|The AccessKey ID that has read permissions on OSS.For security concerns, we recommend that you set the value to res\_local\("AK\_ID"\) in the Advanced Parameter Settings field. For information about how to set the Advanced Parameter Settings field, see [Create a data transformation rule](/intl.en-US/Data Transformation/Create a data transformation rule.md).

![AccessKey](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0552813061/p136966.png) |
    |ak\_key|The AccessKey secret that has read permissions on OSS.For security concerns, we recommend that you set the value to res\_local\("AK\_KEY"\) in the Advanced Parameter Settings field. |
    |bucket|The OSS bucket used to store the IP address library.|
    |file|The name of the IP address library that you have uploaded.|
    |format|The format of the data obtained from the IP address library. Set the value to binary.|

-   Result:

    ```
    ip: 19.0.0.0
    city: Dearborn
    province: Michigan
    country: United States
    ```


