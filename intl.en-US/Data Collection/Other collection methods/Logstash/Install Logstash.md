# Install Logstash

This topic describes how to install Logstash.

Logstash is an open source software application for data collection. You can use Logstash to collect logs and then use the logstash-output-logservice plug-in to upload the logs to Log Service. To download the logstash-output-logservice plug-in, visit [GitHub](https://github.com/aliyun/logstash-output-logservice).

1.  Install the JDK package.

    1.  Download the JDK package.

        Go to the [Java official website](http://www.oracle.com/technetwork/java/javase/downloads/index.html), download the JDK package, and then double-click the package to install the JDK package.

    2.  Set the environment variables.

        Choose **Control Panel** \> **System** \> **Advanced system settings** \> **Advanced** \> **Environment Variables**. In the window that appears, set the environment variables.

        -   PATH: C:\\Program Files\\Java\\jdk1.8.0\_73\\bin
        -   CLASSPATH: C:\\Program Files\\Java\\jdk1.8.0\_73\\lib;C:\\Program Files\\Java\\jdk1.8.0\_73\\lib\\tools.jar
        -   JAVA\_HOME: C:\\Program Files\\Java\\jdk1.8.0\_73
        Replace jdk1.8.0\_73 with your actual JDK version.

    3.  Verify that the JDK package is installed.

        Run the java -version command. If a result that is similar to the following example is returned, the JDK package is installed.

        ```
        PS C:\Users\Administrator> java -version
        java version "1.8.0_73"
        Java(TM) SE Runtime Environment (build 1.8.0_73-b02)
        Java HotSpot(TM) 64-Bit Server VM (build 25.73-b02, mixed mode)
        PS C:\Users\Administrator> javac -version
        javac 1.8.0_73
        ```

2.  Install Logstash.

    1.  [Download the Logstash installation package](https://www.elastic.co/downloads/logstash).

        **Note:** We recommend that you download Logstash 5.0 or later.

    2.  Decompress the downloaded package to the specified directory.

3.  Install Logstash plug-in.

    1.  Select the installation mode based on the network environment of the server.

        -   Online installation

            The plug-in is hosted in the RubyGems service. For more information, see [logstash-output-logservice](https://rubygems.org/gems/logstash-output-logservice).

            Run the following command to install Logstash:

            ```
            PS C:\logstash-6.4.3> .\bin\logstash-plugin install logstash-output-logservice
            ```

        -   Offline installation
            1.  Click [here](https://test-lichao.oss-cn-hangzhou.aliyuncs.com/logstash/logstash-offline-plugins-6.6.2.zip) to download the installation package.
            2.  Run the following command to install Logstash:

                ```
                bin/logstash-plugin install file:///root/logstash-offline-plugins-6.6.2.zip
                ```

    2.  Verify that the Logstash plug-in is installed.

        Run the following command. If the logstash-output-logservice plug-in is displayed in the returned plug-in list, the plug-in is installed.

        ```
        PS C:\logstash-6.4.3> .\bin\logstash-plugin list
        ```


