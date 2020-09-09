# Embed console pages into web pages

Log Service allows you to embed search and analysis pages, dashboard pages, and other pages into web pages. This allows you to view log data in a more efficient way.

After you configure log collection, search and analysis, and other features of Log Service, you may want to search and analyze log data and view visualized analysis results on dashboards. You may also want to share the log data with your colleagues. If your colleagues use RAM users to view the data, high management costs are incurred. If you embed specified search and analysis pages and dashboard pages into the self-built website of your enterprise, you and your colleagues can use the visualized analysis feature without logging on to Log Service.

-   You can embed search and analysis pages and dashboard pages into web pages.
-   You can use Security Token Service \(STS\) to generate a logon link and use RAM to grant permissions such as the read-only permission.

1.  Log on to the self-built website.

    Access Security Token Service to obtain an AccessKey pair and token.

    -   For more information about STS, see [Use STS to enable cross-account access to Log Service resources](/intl.en-US/Developer Guide/API Reference/RAM/STS/Use STS to enable cross-account access to Log Service resources.md).
    -   For more information about how to grant permissions on a specified Logstore, see [Create a RAM user and authorize the RAM user to access Log Service](/intl.en-US/Developer Guide/Access control RAM/Create a RAM user and authorize the RAM user to access Log Service.md).
2.  Access the logon service to obtain a logon token.

    Use the AccessKey pair and token to call the logon service API operation to obtain a logon token.

    **Note:**

    -   The STS token may contain special characters. Before you use the token, you must use the URL encoding method to encode the special characters.
    -   The STS token is valid for 3 hours. We recommend that you set a link on the self-built website to generate a new logon token for each request and redirect you to the Log Service pages by using the 302 status code.
    Sample request:

    ```
    http://signin.aliyun.comsignin-intl.aliyun.com/federation?Action=GetSigninToken
                        &AccessKeyId=<AccessKey ID returned by STS>
                        &AccessKeySecret=<AccessKey secret returned by STS>
                        &SecurityToken=<STS token>
                        &TicketType=mini
    ```

3.  Generate a logon-free link to access Log Service resources.

    1.  Obtain a link to access Log Service resources.

        -   Editable search and analysis page on which you can specify multiple parameters:

            ```
            https://sls4service.console.aliyun.com/next/project/<Project name>/logsearch/<Logstore name>?hideTopbar=true&hideSidebar=true
            ```

        -   Search and analysis page:

            ```
            https://sls4service.console.aliyun.com/next/project/<Project name>/logsearch/<Logstore name>?isShare=true&hideTopbar=true&hideSidebar=true
            ```

        -   Dashboard page:

            ```
            https://sls4service.console.aliyun.com/next/project/<Project name>/dashboard/<Dashboard name>?isShare=true&hideTopbar=true&hideSidebar=true
            ```

            **Note:** The Dashboard name in the preceding link is the name on the web page link. It is not the display name of the dashboard.

    2.  Generate a logon-free link from the logon token and Log Service web page link.

        Sample request:

        ```
        http://signin.aliyun.comsignin-intl.aliyun.com/federation?Action=Login
                                    &LoginUrl=<Logon URL on the self-built website that is configured to return the HTTP status code 302 to redirect to another page>
                                    &Destination=<The Log Service search and analysis page or dashboard page>
                                    &SigninToken=<Logon token>
        ```


The sample script in Java, PHP, and Python is as follows:

-   [Java](https://samplecode.oss-cn-hangzhou.aliyuncs.com/slsconsole.java?spm=a2c4g.11186623.2.6.LewJJX&file=slsconsole.java):

    ```
    <dependency>
        <groupId>com.aliyun</groupId>
        <artifactId>aliyun-java-sdk-sts</artifactId>
        <version>3.0.0</version>
    </dependency>
    <dependency>
        <groupId>com.aliyun</groupId>
        <artifactId>aliyun-java-sdk-core</artifactId>
        <version>3.5.0</version>
    </dependency>
    <dependency>
        <groupId>org.apache.httpcomponents</groupId>
        <artifactId>httpclient</artifactId>
        <version>4.5.5</version>
    </dependency>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.47</version>
    </dependency>
    ```

-   [PHP](https://samplecode.oss-cn-hangzhou.aliyuncs.com/slsconsole.php?spm=a2c4g.11186623.2.7.LewJJX)
-   [Python](https://samplecode.oss-cn-hangzhou.aliyuncs.com/slsconsole.py?spm=a2c4g.11186623.2.8.LewJJX&file=slsconsole.py)

