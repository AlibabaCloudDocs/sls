# Pull a CSV file from OSS to enrich data

This topic describes how to use resource functions to pull data from Object Storage Service \(OSS\) and use mapping functions to map the data fields to data fields in Log Service. This way, you can enrich log data in Log Service.

-   AccessKey pairs are created to access an OSS bucket. For more information, see [Create an AccessKey pair for a RAM user](/intl.en-US/Security Settings/AccessKey pairs/Create an AccessKey pair for a RAM user.md).

    We recommend that you create an AccessKey pair that has read-only permissions on the OSS bucket and an AccessKey pair that has write-only permissions on OSS. This way, you can use the first AccessKey pair to read data from the OSS bucket and use the second AccessKey pair to write data to the OSS bucket. For more information, see [Implement access control based on RAM policies](/intl.en-US/Developer Guide/Data security/Access and control/RAM Policy/Implement access control based on RAM policies.md).

-   A comma-separated values \(CSV\) file is uploaded to the OSS bucket. For more information, see [Upload objects](/intl.en-US/Quick Start/Upload objects.md).

    We recommend that you use the AccessKey pair that has write-only permissions on the OSS bucket to upload the file.


OSS provides secure, cost-effective, and highly reliable services for storing large amounts of data in the cloud. We recommend that you store infrequently updated data in OSS buckets. You are charged only for data storage. If your log data is distributed and incomplete, you can enrich your data by using OSS. The data transformation feature of Log Service provides multiple functions to transform data. You can use the [res\_oss\_file](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.md) function to pull data from OSS and use the [tab\_parse\_csv](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Table functions.md)function to create a table. Then, you can use the [e\_table\_map](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md) function to match the specified fields, return the specified fields and field values, and generate new log entries.

## Examples

-   Raw log entry

    ```
    account :  Sf24asc4ladDS
    ```

-   CSV data in OSS

    |id|account|nickname|
    |--|-------|--------|
    |1|Sf24asc4ladDS|Doflamingo|
    |2|Sf24asc4ladSA|Kaido|
    |3|Sf24asc4ladCD|Roger|

-   Transformation rule

    Log Service maps the account field in the specified Logstore to the account field in the specified CSV file. If the value of the account field in the specified OSS bucket and Logstore equals each other, the two fields are matched and the nickname field and field value in the CSV file are concatenated with the fields in the specified Logstore.

    ```
    e_table_map(tab_parse_csv(res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',
                                            ak_id=res_local("AK_ID"),
                                            ak_key=res_local("AK_KEY"), 
                                            bucket='test',
                                            file='account.csv',change_detect_interval=30)),
                "account","nickname")
    ```

    The following table describes the fields in the res\_oss\_file function.

    |Field|Description|
    |-----|-----------|
    |endpoint|The endpoint of OSS. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).|
    |ak\_id|The AccessKey ID that has read permissions on OSS.For security concerns, we recommend that you set the value to res\_local\("AK\_ID"\). This value indicates that the AcessKey ID is obtained from the Advanced Parameter Settings field that you set in Log Service. For information about how to set the Advanced Parameter Settings field, see [Create a data transformation rule](/intl.en-US/Data Transformation/Create a data transformation rule.md).

![AccessKey](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0552813061/p136966.png) |
    |ak\_key|The AccessKey secret that has read permissions on OSS.For security concerns, we recommend that you set the value to res\_local\("AK\_KEY"\). This value indicates that the AcessKey secret is obtained from the Advanced Parameter Settings field that you set in Log Service. |
    |bucket|The OSS bucket that is used to store the CSV file.|
    |file|The name of the uploaded CSV file.|

-   Result

    ```
    account :  Sf24asc4ladDS
    nickname: Doflamingo
    ```


