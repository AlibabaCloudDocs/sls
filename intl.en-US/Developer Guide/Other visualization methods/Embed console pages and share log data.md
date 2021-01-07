# Embed console pages and share log data

Log Service allows you to embed console pages in a user-created website. These pages include query and analysis pages and dashboard pages. This allows you to view log data with ease.

After you configure the required features of Log Service, you can collect, query, share log data, and access dashboards by using a website. This avoids the high management costs that are incurred by multiple RAM users. Log Service allows you to embed specified query and analysis pages and dashboard pages in a user-created website. You can access these pages without the need to log on to the Log Service console. You can use Resource Access Management \(RAM\) to control access to these pages. For example, you can grant the read-only permission to a RAM user. For more information, see [Grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Create a RAM user and authorize the RAM user to access Log Service.md#section_kxp_1ok_zj4).

The following figure shows the access process.

![Access process](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5714330061/p6443.png)

## Procedure

1.  Log on to the user-created website. Use Security Token Service \(STS\) to obtain a temporary AccessKey pair and an STS token.

    -   For information about STS, see [Use STS to enable cross-account access to Log Service resources](/intl.en-US/Developer Guide/API Reference/RAM/STS/Use STS to enable cross-account access to Log Service resources.md).
    -   For information about how to authorize access to a specified Logstore, see [Create a RAM user and authorize the RAM user to access Log Service](/intl.en-US/Developer Guide/Access control RAM/Create a RAM user and authorize the RAM user to access Log Service.md).
2.  Call the STS SDK to obtain a token and log on to the Log Service console.

    **Note:**

    -   The STS token may contain special characters. Before you use the token, use the URL encoding method to encode the special characters.
    -   A token is valid for a maximum of 1 hour and a minimum of 15 minutes. We recommend that you specify a URL that points to the query and analysis page of Log Service for the user-created website. After you log on to the user-created website, use STS to obtain a logon token. Then, a 302 response is returned and you are redirected to the page based on the specified URL in the 302 response.
    Sample request:

    ```
    http://signin-intl.aliyun.com/federation? Action=GetSigninToken
                        &AccessKeyId=<Temporary AccessKey ID that is returned by STS>
                        &AccessKeySecret=<Temporary AccessKey secret that is returned by STS>
                        &SecurityToken=<Token that is returned by STS>
                        &TicketType=mini
    ```

3.  Generate a logon-free URL.

    1.  Obtain a URL to access Log Service resources.

        -   Obtain a complete query and analysis page:

            ```
            https://sls4service.console.aliyun.com/lognext/project/<Project name>/logsearch/<Logstore name>?hideTopbar=true&hideSidebar=true
            ```

        -   Obtain query and analysis page:

            ```
            https://sls4service.console.aliyun.com/lognext/project/<Project name>/logsearch/<Logstore name>?isShare=true&hideTopbar=true&hideSidebar=true
            ```

        -   Obtain a dashboard page:

            ```
            https://sls4service.console.aliyun.com/lognext/project/<Project name>/dashboard/<Dashboard ID>?isShare=true&hideTopbar=true&hideSidebar=true
            ```

            **Note:** The preceding Dashboard ID appears only in the URL of a dashboard page. This ID does not match the name that appears on the dashboard.

    2.  Generate a logon-free URL by using the logon token and the URL of Log Service page.

        Sample request:

        ```
        http://signin-intl.aliyun.com/federation? Action=Login
                                    &LoginUrl=<The URL to which you are redirected if your logon fails. This URL is set to the URL to which you are redirected if the HTTP status code 302 is returned for access the pages of query and analysis.> >
                                    &Destination=<The URL of a Log Service query and analysis page or dashboard page> If parameters are available, call the encodeURL function to transcode the parameters. >
                                    &SigninToken=<Logon token>
        ```


## Examples

The following sample scripts are written in Java, PHP, and Python:

-   [Java](https://samplecode.oss-cn-hangzhou.aliyuncs.com/slsconsole.java?spm=a2c4g.11186623.2.6.LewJJX&file=slsconsole.java)

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

