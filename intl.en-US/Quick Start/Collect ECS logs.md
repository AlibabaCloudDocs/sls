# Collect ECS logs {#concept_tr2_2yr_xdb .concept}

This document describes the procedure for collecting Elastic Compute Service \(ECS\) logs by using Logtail in the ECS Log Service console.

## Configuration procedure {#section_jq2_pzr_xdb .section}

1.  Install Logtail on the ECS instance.
2.  Configure the Logtail machine group.
3.  Configure the collection, and apply the configuration to the machine group.

![](images/3871_en-US.png "Configuration procedure")

## Prerequisites {#section_f34_lys_xdb .section}

-   You have activated ECS and Log Service.
-   You have created the Project and Logstore in Log Service. For more information, see [Preparation](../../../../intl.en-US/User Guide/Preparation/Preparation.md).

    **Note:** Make sure that the Log Service Project and the ECS instance are located in the same region if the ECS instance runs in the classic network or virtual private cloud \(VPC\).

-   Your Log Service cannot automatically obtain owner information of the ECS if the Log Service and the ECS instance do not belong to the same account. You need to [Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account](../../../../intl.en-US/User Guide/Logtail collection/Machine Group/Collect logs from non-Alibaba Cloud ECS instances or ECS instances not in your account.md). 

## Step 1: Install Logtail. {#section_xqh_bxs_xdb .section}

1.  Run the installation command.

    Select the Logtail installation script according to the ECS instance region. For more information, see [Linux ](../../../../intl.en-US/User Guide/Logtail collection/Install/Linux .md)和[Install Logtail on Windows](../../../../intl.en-US/User Guide/Logtail collection/Install/Install Logtail on Windows.md).

    For example, to install Logtail for a Linux instance that operates in the classic network in the China \(Hangzhou\) region, run the following command:

    ```
    wget http://logtail-release.oss-cn-hangzhou-internal.aliyuncs.com/linux64/logtail.sh; chmod 755 logtail.sh; sh logtail.sh install cn_hangzhou
    ```
    If you are overseas replace download url the following instead [http://logtail-release-global.log-global.aliyuncs.com/linux64/logtail.sh](http://logtail-release-global.log-global.aliyuncs.com/linux64/logtail.sh)

2.  Check the operation status of Logtail.

    When you check the operation status of Logtail, the echo that displays `ilogtail is running` indicates that the installation is successful. You can run the following command to check the operation status of Logtail:

    ```
    /etc/init.d/ilogtaild status
    ```


![](images/3872_en-US.png "Installing Logtail")

## Step 2 Configure the machine group. {#section_uy3_vxs_xdb .section}

1.  On the homepage of the Log Service console, click the project name to enter the Logstore List page.
2.  In the left-side navigation pane, click **Logtail Machine Group** to enter the Machine Groups page.
3.  Click **Create Machine Group** in the upper-right corner, and enter the intranet IP or custom identifier of your ECS instance in the dialog box that appears, and then click **Confirm**.

    **Note:** 

    -   Log Service only supports ECS instances that run in the same region as the current Project.
    -   Windows and Linux ECS instances cannot exist in the same machine group at the same time.

![](images/3874_en-US.png "Configuring the machine group")

## Step 3: Configure collection. {#section_wt2_vzs_xdb .section}

1.  Enter the data access wizard.

    On the **Logstore List** page, click the icon in the **Data Import Wizard** column.

2.  Select the data type.

    At the Select Data Source step, click **Text**, and click Next.

3.  Set the data source.

    For more information about setting the data source, see [Collect text logs](../../../../intl.en-US/User Guide/Logtail collection/Data Source/Collect text logs.md). To set the data source in **Simple Mode**, follow these steps:

    Enter the path of the ECS logs, and click **Next**.

    ![](images/3873_en-US.png "Basic mode")

4.  Apply the configuration to the machine group.

    Select the machine group that you created in **step 2**, and click **Apply to Machine Group**.

    ![](images/3875_en-US.png "Apply to Machine Group")


Now you have configured the machine group to collect ECS logs by using Logtail. To create indexes for collected logs and configure log shipping, continue with the follow-up steps.

## View logs {#section_e43_vbt_xdb .section}

After the configuration, you can log on to the ECS instance again, or run echo "test message" \>\> `echo "test message" >> /var/log/message`, to generate logs in the local `/var/log/message` file. Then, Logtail collects these logs and stores them to Log Service.

On the Logstore List page, click **Search** or **Preview** to view logs collected by Logtail.

![](images/3876_en-US.png "Viewing logs")

![](images/3877_en-US.png "Previewing logs")

![](images/3878_en-US.png "Retrieving logs")

