# Configure preview modes

You can use the preview feature to debug data transformation scripts. This feature includes the Quick preview mode and Advanced preview mode. This topic describes how to configure these two preview modes.

-   A project and a Logstore are created. For more information, see [Create a project and a Logstore](/intl.en-US/.md).
-   Log data is collected. For more information, see [Data collection](/intl.en-US/Data Collection/Log collection methods.md).

In Quick preview mode, you can check whether the syntax of a data transformation script is valid and whether data is transformed as expected. You can also use raw logs of a Logstore or custom data as test data. In Quick preview mode, the res\_local, res\_rds\_mysql, res\_log\_logstore\_pull, or res\_oss\_file resource function that you specify does not access real data. You can preview data on the Dimension Table tab.

In Advanced preview mode, you are charged for data traffic. We recommend that you check whether data is transformed as expected in Quick preview mode and check whether a resource function is correctly configured in Advanced mode.

## Quick preview

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project.

3.  Enable the data transformation mode.

    You can enable the data transformation mode by using one of the following two methods:

    -   Choose **Log Storage** \> **Logstores**, and then click the source Logstore. On the search and analysis page that appears, turn on the **Data Transformation** switch.
    -   Choose **Log Storage** \> **Logstores**. On the Logstores tab, click the **\>** icon of the source Logstore, choose **Data Transformation** \> **Data Transformation**, and then click the plus sign \(+\) next to **Data Transformation**. The data transformation mode is enabled.
4.  Enter a transformation rule in the text box.

    For more information, see [Data processing syntax](/intl.en-US/Data Transformation/Data processing syntax/Language introduction.md).

5.  Preview data.

    1.  In the upper-right corner of the page, click **Quick**.

    2.  Click the Data Testing tab.

    3.  On the Data Testing tab, enter test data.

        Test data includes basic data and dimension table data.

        ![Data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8498280061/p132513.png)

        -   On the Data tab, you can enter or specify basic data.

            You can click the Raw Logs tab, find a log entry, and click **Import Test Data**. The log entry is added as test data. You can enter a test log entry.

            **Note:**

            -   The size of the test data for a single preview cannot exceed 1 MB.
            -   Each test log entry is separated by a blank line.
            -   The Markdown syntax is used to indicate cross-line field values. Triple backticks \(\`\`\`\) are used to indicate a whole field.
            -   If you enter or specify test data on the Data tab, the test data must be in the key-value pair or JSON format. Colons \(:\) are used to connect field names and field values in key-value pair formatted data.
            -   Example 1: The test data includes two log entries. The first log entry is in the key-value pair format. This log entry has a cross-line field named traceback. The second log entry is in the JSON format.

                ```
                time_local: 25/May/2020:01:56:22
                user agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US) AppleWebKit/534.18 (KHTML, like Gecko) Chrome/11.0.661.0 Safari/534.18
                "request:method": GET
                ```    
                traceback: Traceback (most recent call last):
                  File "traceback_print_exc.py", line 20, in <module>
                    produce_exception()
                  File "/home/user/code/test.py", line 16, in produce_exception
                    produce_exception(recursion_level-1)
                  File "/home/user/code/test.py", line 18, in produce_exception
                    raise RuntimeError()
                
                RuntimeError
                ```
                
                {
                  "time_local": "25/May/2020:01:56:22",
                  "user agent": "Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US) AppleWebKit/534.18 (KHTML, like Gecko) Chrome/11.0.661.0 Safari/534.18",
                  "request:method": "GET",
                  "remote user": "john"
                }
                ```

            -   Example 2: The test data includes three log entries in the JSON format.

                ```
                [
                  {
                    "time_local": "25/May/2020:01:56:22",
                    "user agent": "Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US) AppleWebKit/534.18 (KHTML, like Gecko) Chrome/11.0.661.0 Safari/534.18",
                    "request:method": "GET",
                    "remote user": "john"
                  },
                  {
                    "time_local": "25/May/2020:01:56:22",
                    "user agent": "Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US) AppleWebKit/534.18 (KHTML, like Gecko) Chrome/11.0.661.0 Safari/534.18",
                    "request:method": "GET",
                    "remote user": "john"
                  },
                  {
                    "time_local": "25/May/2020:01:56:22",
                    "user agent": "Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US) AppleWebKit/534.18 (KHTML, like Gecko) Chrome/11.0.661.0 Safari/534.18",
                    "request:method": "GET",
                    "remote user": "john"
                  }
                ]
                ```

        -   On the Dimension Table tab, you can enter or specify dimension table data.

            You can use dimension tables to preview the resources that resource functions access. You can enter sample dimension table data for preview and debugging.

            **Note:** If you enter test data on the Dimension Table tab and specify the res\_rds\_mysql or res\_log\_logstore\_pull function to transform the data, the data must be in the CSV format. If you specify the res\_oss\_file or res\_local function, the data must be in the CSV or JSON format.

            Example: The test data includes two log entries. The first log entry is in the CSV format, and the second log entry is in the JSON format.

            ```
            ip,country,province
            127.0.0.1,China,Shanghai
            192.168.0.0,China,Zhejiang
            
            [
              {
                "ip": "127.0.0.1",
                "country": "China",
                "province": "Shanghai"
              },
              {
                "ip": "192.168.0.0",
                "country": "China",
                "province": "Zhejiang"
              }
            ]
            ```

    4.  Click **Preview Data**.

        **Note:** A maximum of 100 log entries can be returned for each data transformation test.

        -   If data fails to be transformed because the syntax of the transformation rule or permissions are invalid, troubleshoot the failure as prompted.
        -   If data is transformed as expected, you can save the transformation script. For more information, see [Create a data transformation rule](/intl.en-US/Data Transformation/Create a data transformation rule.md).

## Advanced preview

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project.

3.  Enable the data transformation mode.

    You can enable the data transformation mode by using one of the following two methods:

    -   Choose **Log Storage** \> **Logstores**, and then click the source Logstore. On the search and analysis page that appears, turn on the **Data Transformation** switch.
    -   Choose **Log Storage** \> **Logstores**. On the Logstores tab, click the **\>** icon of the source Logstore, choose **Data Transformation** \> **Data Transformation**, and then click the plus sign \(+\) next to **Data Transformation**. The data transformation mode is enabled.
4.  On the data transformation page, select a time range for the raw logs.

    Make sure that log data exists on the Raw Logs tab.

5.  Enter a transformation rule in the text box.

    For more information, see [Data processing syntax](/intl.en-US/Data Transformation/Data processing syntax/Language introduction.md).

6.  Preview data.

    1.  Click the **Advanced** tab.

    2.  Click **Preview Data**.

    3.  On the Add Preview Settings page, set the parameters, and then click **OK**. The following table describes the parameters.

        **Note:** When you preview data for the first time, you must set the parameters. After you set the parameters, you can click **Modify Preview Settings** to modify the parameters.

        |Parameter|Description|
        |---------|-----------|
        |Mode|You can use one of the following methods to authorize Log Service to read data from the source Logstore:        -   Default role: Authorize Log Service to assume the system role AliyunLogETLRole to read data from the source Logstore.

Click **Authorize the system role AliyunLogETLRole** and authorize Log Service as prompted.

**Note:**

            -   If you use a RAM user to log on to Log Service, you must grant read/write permissions on Log Service to the RAM user by using an Alibaba Cloud account.
            -   If you have authorized Log Service to assume the system role, skip this step.
        -   Custom role: Authorize Log Service to assume a custom role to read data from the source Logstore.

In the **Role ARN** field, enter the ARN of a custom role. For more information, see [Authorize Log Service to assume a custom role](/intl.en-US/Data Transformation/Grant permissions/Authorize Log Service/Authorize Log Service to assume a custom role.md).

        -   AccessKey pair: Log Service uses the AccessKey pair of an Alibaba Cloud account or RAM user to read data from the source Logstore. The Alibaba Cloud account or RAM user must have read permissions on the source Logstore.

Enter an AccessKey ID in the **AccessKey** ID field and the corresponding AccessKey secret in the **AccessKey Secret** field. For more information, see [Configure an AccessKey pair for a RAM user to access a source Logstore and a destination Logstore](/intl.en-US/Data Transformation/Grant permissions/Authorize Log Service/Configure an AccessKey pair for a RAM user to access a source Logstore and a destination
         Logstore.md). |
        |Advanced Parameter Settings|Set the passwords that are required in the transformation rule in the key-value pair format. For example, you can set a password that is used to connect to a database in the key-value pair format. Passwords are referenced in the transformation rule syntax by using the `${key}` variable.You can click the plus sign \(**+**\) to create multiple key-value pairs. For example, config.vpc.vpc\_id.test1:vpc-uf6mskb0b\*\*\*\*n9yj indicates the ID of the VPC where an RDS instance resides.

![Advanced parameter settings](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7888280061/p130800.png) |

    4.  View the transformation results.

        -   If data fails to be transformed because the syntax of the transformation rule or permissions are invalid, troubleshoot the failure as prompted.
        -   If data is transformed as expected, you can save the transformation script. For more information, see [Create a data transformation rule](/intl.en-US/Data Transformation/Create a data transformation rule.md).

[Create a data transformation rule](/intl.en-US/Data Transformation/Create a data transformation rule.md)

