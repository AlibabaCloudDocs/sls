# 经典网络切换为VPC后，是否需要更新Logtail配置

经典网络切换为VPC后，需要重启Logtail，并更新Logtail机器组设置。

安装Logtail后，如果您的ECS网络类型由经典网络切换为VPC，请参考以下步骤更新配置。

1.  以管理员身份重启Logtail。
    -   **Linux**：

        ```
        sudo /etc/init.d/ilogtaild stop
        sudo /etc/init.d/ilogtaild start
        ```

    -   **Windows**：

        打开**控制面板**中的**管理工具**，打开**服务**，找到LogtailWorker并右键单击**重新启动**。

2.  更新机器组配置。
    -   **自定义标识**

        若机器组中配置了自定义标识，则无需手动更新机器组配置，可以直接正常使用VPC网络。

    -   **IP地址**

        若机器组中配置了ECS云服务器IP地址，则需将机器组内的IP更换为重启Logtail后获取的IP地址，即app\_info.json中的ip字段。

        app\_info.json文件地址：

        -   Linux：/usr/local/ilogtail/app\_info.json
        -   Windows x64：C:\\Program Files \(x86\)\\Alibaba\\Logtail\\app\_info.json
        -   Windows x32：C:\\Program Files\\Alibaba\\Logtail\\app\_info.json

