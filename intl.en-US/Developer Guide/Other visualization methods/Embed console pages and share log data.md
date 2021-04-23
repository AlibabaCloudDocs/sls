# Embed console pages and share log data

Log Service allows you to embed console pages in a self-managed website. These pages include query and analysis pages and dashboard pages. This feature allows you to view log data with ease.

After you configure the required features of Log Service, you can collect, query, share log data, and access dashboards by using a website. This prevents the high management costs that are incurred by multiple Resource Access Management \(RAM\) users. Log Service allows you to embed specified query and analysis pages and dashboard pages in a self-managed website. You can access these pages without the need to log on to the Log Service console. You can use RAM to control access to these pages. For example, you can grant the read-only permissions to a RAM user. For more information, see [Grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Create a RAM user and authorize the RAM user to access Log Service.md#section_kxp_1ok_zj4).

The following figure shows the access process.

![Access process](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5714330061/p6443.png)

## Procedure

1.  Log on to the self-managed website. Use Security Token Service \(STS\) to obtain a temporary AccessKey pair and an STS token.

    -   For information about STS, see [Use STS to enable cross-account access to Log Service resources](/intl.en-US/Developer Guide/API Reference/RAM/STS/Use STS to enable cross-account access to Log Service resources.md).
    -   For information about how to authorize access to a specific Logstore, see [Create a RAM user and authorize the RAM user to access Log Service](/intl.en-US/Developer Guide/Access control RAM/Create a RAM user and authorize the RAM user to access Log Service.md) and [Assign a RAM role to an Alibaba Cloud account](/intl.en-US/Developer Guide/Access control RAM/Assign a RAM role to an Alibaba Cloud account.md).
2.  Call the STS SDK to obtain a temporary AccessKey pair and STS Token.

    **Note:** The STS token may contain special characters. Before you use the token, encode the special characters by using URL encoding.

    Sample request:

    ```
    http://signin-intl.aliyun.com/federation?Action=GetSigninToken
                        &AccessKeyId=<Temporary AccessKey ID that is returned by STS>
                        &AccessKeySecret=<Temporary AccessKey secret that is returned by STS>
                        &SecurityToken=<Token that is returned by STS>
                        &TicketType=mini
    ```

3.  Modify the validity period of the STS token.

    By default, the validity period of an STS token ranges from 15 minutes to 60 minutes. You can perform the operations in this step to modify the maximum validity period of an STS token to 12 hours.

    1.  In the [RAM console](https://ram.console.aliyun.com), change the value of the Maximum Session Duration parameter to 12 hours for the specified RAM role.

        ![RAM role](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1506619161/p243934.png)

    2.  In the [RAM console](https://ram.console.aliyun.com), change the value of the Logon Session Valid For parameter to 12 hours for the specified RAM user.

        ![RAM user](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1506619161/p243936.png)

    3.  Change the value of the setDurationSeconds field in the logon-free code to 43200L.

        The following example shows the [JAVA code](https://samplecode.oss-cn-hangzhou.aliyuncs.com/slsconsole.java?spm=a2c4g.11186623.2.6.LewJJX&file=slsconsole.java):

        ```
        AssumeRoleRequest assumeRoleReq = new AssumeRoleRequest();
        assumeRoleReq.setRoleArn(roleArn);
        assumeRoleReq.setRoleSessionName(roleSession);
        assumeRoleReq.setMethod(MethodType.POST);
        assumeRoleReq.setDurationSeconds(43200L);
        ```

4.  Generate a logon-free URL.

    1.  Obtain a URL to access Log Service resources.

        -   Obtain a complete query and analysis page:

            ```
            https://sls4service.console.aliyun.com/lognext/project/<Project name>/logsearch/<Logstore name>?hideTopbar=true&hideSidebar=true
            ```

        -   Obtain a query and analysis page:

            ```
            https://sls4service.console.aliyun.com/lognext/project/<Project name>/logsearch/<Logstore name>?isShare=true&hideTopbar=true&hideSidebar=true
            ```

        -   Obtain a dashboard page:

            ```
            https://sls4service.console.aliyun.com/lognext/project/<Project name>/dashboard/<Dashboard ID>?isShare=true&hideTopbar=true&hideSidebar=true
            ```

            **Note:** The preceding Dashboard ID appears only in the URL of a dashboard page. This ID is not the name that appears on the dashboard.

    2.  Generate a logon-free URL by using the logon token and the URL of the Log Service page.

        Sample request:

        ```
        http://signin-intl.aliyun.com/federation?Action=Login
                                    &LoginUrl=<The URL to which you are redirected if your logon fails. This URL is set to the URL to which you are redirected if the HTTP status code 302 is returned. >
                                    &Destination=<The URL of a Log Service query and analysis page or dashboard page in the Log Service console. If parameters exist, call the encodeURL function to transcode the parameters. >
                                    &SigninToken=<Logon token>
        ```


## Examples

The following sample scripts are written in Java, PHP, and Python:

-   [JAVA](https://samplecode.oss-cn-hangzhou.aliyuncs.com/slsconsole.java?spm=a2c4g.11186623.2.6.LewJJX&file=slsconsole.java)

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

