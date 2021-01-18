# 安装Logstash

本文介绍Logstash的安装步骤。

Logstash是一款开源的数据采集软件，您可以通过logstash-output-logservice插件，将Logstash采集到的日志上传到日志服务。logstash-output-logservice插件的GitHub项目地址：[Logstash插件](https://github.com/aliyun/logstash-output-logservice)。

1.  安装Java。

    1.  下载安装包。

        请进入[Java 官网](http://www.oracle.com/technetwork/java/javase/downloads/index.html)下载JDK并双击进行安装。

    2.  设置环境变量。

        在**控制面板** \> **系统** \> **高级系统设置** \> **高级** \> **环境变量**中，设置环境变量。

        -   PATH：C:\\Program Files\\Java\\jdk1.8.0\_73\\bin
        -   CLASSPATH：C:\\Program Files\\Java\\jdk1.8.0\_73\\lib;C:\\Program Files\\Java\\jdk1.8.0\_73\\lib\\tools.jar
        -   JAVA\_HOME：C:\\Program Files\\Java\\jdk1.8.0\_73
        其中，请根据实际的JAVA版本替换jdk1.8.0\_73。

    3.  验证JAVA安装结果。

        执行java -version命令，显示如下类似结果表示安装JAVA完成。

        ```
        PS C:\Users\Administrator> java -version
        java version "1.8.0_73"
        Java(TM) SE Runtime Environment (build 1.8.0_73-b02)
        Java HotSpot(TM) 64-Bit Server VM (build 25.73-b02, mixed mode)
        PS C:\Users\Administrator> javac -version
        javac 1.8.0_73
        ```

2.  安装Logstash。

    1.  [下载安装包](https://www.elastic.co/downloads/logstash)。

        **说明：** 建议下载Logstash 5.0及以上版本。

    2.  解压安装包到指定目录。

3.  安装Logstash插件。

    1.  根据服务器所处网络环境选择安装模式。

        -   在线安装

            该插件托管于RubyGems。更多信息，请参见[logstash-output-logservice](https://rubygems.org/gems/logstash-output-logservice) 。

            进入Logstash安装目录，执行如下命令安装Logstash插件。

            ```
            PS C:\logstash-6.4.3> .\bin\logstash-plugin install logstash-output-logservice
            ```

        -   离线安装
            1.  [下载离线安装包](https://test-lichao.oss-cn-hangzhou.aliyuncs.com/logstash/logstash-offline-plugins-6.6.2.zip)。
            2.  进入Logstash安装目录，执行如下命令安装Logstash插件。

                ```
                bin/logstash-plugin install file:///root/logstash-offline-plugins-6.6.2.zip
                ```

    2.  验证Logstash插件安装结果。

        执行如下命令，如果返回的已安装的插件列表中有logstash-output-logservice，则表示安装成功。

        ```
        PS C:\logstash-6.4.3> .\bin\logstash-plugin list
        ```


