# Application operation and management

This topic provides an overview of the COVID-19 pandemic analysis application, including the free resources, resource limits, data description, usage instructions, FAQ, and references.

## Overview

-   Before you can use the application for the first time, you must initialize this application. The initialization process takes about two minutes.
-   Data is automatically updated every day.
-   Data sources: Novel Coronavirus \(COVID-19\) Cases, provided by JHU CSSE.
-   Update frequency: once a day around 23:59 \(UTC\).
-   Data is collected from multiple sources that update at different times and may not always align.
-   If you do not use the free resources of the application for a long time, the resources may be reclaimed. To continue using the free resources, you must re-initialize this application.
-   Technical support is provided.

    Powered by Alibaba Cloud Log Service. For more information, scan the QR code.

    ![Pandemic analysis - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0706498951/p81882.png)


## Free resources

The following free resources are provided in the application:

-   Project: ncp-\{Alibaba Cloud account UID\}-cn-chengdu
-   Logstore: ncp
-   Dashboard: covid-19\_global and covid-19\_detail.

## Limits

-   You cannot modify or delete the Logstore ncp, the indexes created for the Logstore, or the data written to the Logstore. Other than these limits, you can perform the same operations on the Logstore as on normal Logstores.
-   You can create Logstores in the automatically created project in the application and write data to the Logstores. However, you are charged with the created Logstores.
-   Modifications to the automatically created dashboards are overwritten in case of updates. Therefore, we recommend that you do not modify the dashboards. You can copy the automatically created dashboards and modify the copies. For more information, see [How do I copy an existing dashboard to create a dashboard?](#section_arl_m0e_xg3)

## Dashboard

The pandemic analysis application provides multiple built-in dashboards. They are automatically created based on the log data in the Logstore ncp. You can also create dashboards based on the data in Logstores.

|Dashboard|ID|Description|
|---------|--|-----------|
|COVID-19 Global|covid-19\_global|Various indicators, trend charts, and lists of the COVID-19 pandemic situation in countries and regions across the globe are displayed.|
|COVID-19 Detail|covid-19\_detail|Various indicators, trend charts, and lists of the COVID-19 pandemic situation in provinces or states of the countries and regions across the globe are displayed.|

## Log data

-   Data updates and instructions

    COVID-19 pandemic datasets are stored in the Logstore ncp. Data updates are automatically synchronized to the Logstore every day. Every update is indicated by the version field, for example, v2020-01-26T12:30:00.

    Every update contains full data. Therefore, you can search and analyze the latest update to retrieve data that you want.

    You can specify the version field to search and analyze data in the specified update, as shown in the following statement.

    ```
    Version: "v2020-01-26T12:30:00" and Type : "Province/State Cases" | select .... from log
    ```

    We recommend that you use the following statement to perform search and analysis. This makes sure that you can query the latest update.

    ```
    Type : "Province/State Cases" | select .... from log l right join (select max(Version) as Version from log) r on  l.Version =  r.Version
    ```

    **Note:**

    -   The code before \| is a search statement. In most cases, you can specify the Type field to filter specific types of log data. For more information about the search syntax, see [Search syntax](/intl.en-US/Index and query/Query/Search syntax.md).
    -   The code after \| is a statement of the standard SQL92 syntax. The FROM LOG clause specifies to query log data from the current Logstore. You can also specify in the statement to join multiple Logstores. In addition, you can query data from external data sources such as geographical location databases of IP addresses and OSS and MySQL external tables. For more information, see [Analytics syntax](/intl.en-US/Index and query/Analysis grammar/General aggregate functions.md).
    -   Data is automatically updated every day. You can select 1 Day\(Relative\) to perform queries.
-   Overview

    COVID-19 pandemic datasets are stored in the Logstore ncp. They are identified based on the Type field values, including Global Cases, Country/Region Cases, and Province/State Cases.

-   Global Cases

    **Note:** The Hist related data is displayed in mini-charts in the table. The Trend related data is displayed in trend charts.

    |Field name|Description|Value|
    |----------|-----------|-----|
    |Type|The type of the datasets.|Valid value: Global Cases.|
    |Version|The version of the data update.|v2020-01-26T12:30:00|
    |Last Update|The time when the latest update was released.|2020-01-26 18:23|
    |Confirmed|The latest total number of confirmed cases.|1058|
    |Confirmed Hist|The total number of confirmed cases of COVID-19 since Jan 23, 2020, in the format of an array.|\[270, 444, 444, 549, 729, 1058\]|
    |Confirmed Trend|The total number of confirmed cases of COVID-19 since Jan 23, 2020, in the format of a dictionary.|\{"2020-01-21": 1, "2020-01-22": 1, "2020-01-23": 1, "2020-01-24": 2, "2020-01-25": 2, "2020-01-26": 3\}|
    |Recovered|The latest total number of recovered cases.|42|
    |Recovered Hist|The total number of recovered cases of COVID-19 since Jan 23, 2020, in the format of an array.|\[0, 28, 28, 31, 32, 42\]|
    |Recovered Trend|The total number of recovered cases of COVID-19 since Jan 23, 2020, in the format of a dictionary.|\{"2020-01-21": 1, "2020-01-22": 1, "2020-01-23": 1, "2020-01-24": 2, "2020-01-25": 2, "2020-01-26": 3\}|
    |Deaths|The latest total number of deaths.|52|
    |Deaths Hist|The total number of deaths from COVID-19 since Jan 23, 2020, in the format of an array.|\[3, 17, 17, 24, 39, 52\]|
    |Deaths Trend|The total number of deaths from COVID-19 since Jan 23, 2020, in the format of a dictionary.|\{"2020-01-21": 1, "2020-01-22": 1, "2020-01-23": 1, "2020-01-24": 2, "2020-01-25": 2, "2020-01-26": 3\}|
    |New Confirmed Hist|The total number of suspected cases of COVID-19 since Jan 23, 2020, in the format of an array.|\[11, 0, 41, 0, 56, 127\]|
    |New Confirmed Trend|The total number of suspected cases of COVID-19 since Jan 23, 2020, in the format of a dictionary.|\{"2020-01-21": 1, "2020-01-22": 1, "2020-01-23": 1, "2020-01-24": 2, "2020-01-25": 2, "2020-01-26": 7\}|

-   Country/Region Cases

    **Note:** The Hist related data is displayed in mini-charts in the table. The Trend related data is displayed in trend charts.

    |Field|Description|Value|
    |-----|-----------|-----|
    |Type|The type of the datasets.|Valid value: Country/Region Cases.|
    |Version|The version of the data update.|v2020-01-26T12:30:00|
    |Last Update|The time when the latest update was released.|2020-01-26 18:23|
    |Country/Region|The name of the country or region.|China, US|
    |LatLng|The longitude and latitude of the country or region, in the format of a string <lat\>,<lng\>.|51.7283857,-2.2085499|
    |Confirmed|The latest total number of confirmed cases.|1058|
    |Confirmed Hist|The total number of confirmed cases of COVID-19 since Jan 23, 2020, in the format of an array.|\[270, 444, 444, 549, 729, 1058\]|
    |Confirmed Trend|The total number of confirmed cases of COVID-19 since Jan 23, 2020, in the format of a dictionary.|\{"2020-01-21": 1, "2020-01-22": 1, "2020-01-23": 1, "2020-01-24": 2, "2020-01-25": 2, "2020-01-26": 3\}|
    |Recovered|The latest total number of recovered cases.|42|
    |Recovered Hist|The total number of recovered cases of COVID-19 since Jan 23, 2020, in the format of an array.|\[0, 28, 28, 31, 32, 42\]|
    |Recovered Trend|The total number of recovered cases of COVID-19 since Jan 23, 2020, in the format of a dictionary.|\{"2020-01-21": 1, "2020-01-22": 1, "2020-01-23": 1, "2020-01-24": 2, "2020-01-25": 2, "2020-01-26": 3\}|
    |Deaths|The latest total number of deaths.|52|
    |Deaths Hist|The total number of deaths from COVID-19 since Jan 23, 2020, in the format of an array.|\[3, 17, 17, 24, 39, 52\]|
    |Deaths Trend|The total number of deaths from COVID-19 since Jan 23, 2020, in the format of a dictionary.|\{"2020-01-21": 1, "2020-01-22": 1, "2020-01-23": 1, "2020-01-24": 2, "2020-01-25": 2, "2020-01-26": 3\}|
    |New Confirmed Hist|The total number of suspected cases of COVID-19 since Jan 23, 2020, in the format of an array.|\[11, 0, 41, 0, 56, 127\]|
    |New Confirmed Trend|The total number of suspected cases of COVID-19 since Jan 23, 2020, in the format of a dictionary.|\{"2020-01-21": 1, "2020-01-22": 1, "2020-01-23": 1, "2020-01-24": 2, "2020-01-25": 2, "2020-01-26": 7\}|

-   Province/State Cases

    **Note:**

    -   If the source data does not specify the province or state information, the Province/State field value is Unspecified \*.
    -   The Hist related data is displayed in mini-charts in the table. The Trend related data is displayed in trend charts.
    |Field|Description|Value|
    |-----|-----------|-----|
    |Type|The type of the datasets.|Valid value: Province/State Cases.|
    |Version|The version of the data update.|v2020-01-26T12:30:00|
    |Last Update|The time when the latest update was released.|2020-01-26 18:23|
    |Country/Region|The name of the country or region.|China, US|
    |Province/State|The name of the province or state.|Shanghai, New York|
    |LatLng|The longitude and latitude of the province or state, in the format of a string <lat\>,<lng\>.|51.7283857,-2.2085499|
    |Confirmed|The latest total number of confirmed cases.|1058|
    |Confirmed Hist|The total number of confirmed cases of COVID-19 since Jan 23, 2020, in the format of an array.|\[270, 444, 444, 549, 729, 1058\]|
    |Confirmed Trend|The total number of confirmed cases of COVID-19 since Jan 23, 2020, in the format of a dictionary.|\{"2020-01-21": 1, "2020-01-22": 1, "2020-01-23": 1, "2020-01-24": 2, "2020-01-25": 2, "2020-01-26": 3\}|
    |Recovered|The latest total number of recovered cases.|42|
    |Recovered Hist|The total number of recovered cases of COVID-19 since Jan 23, 2020, in the format of an array.|\[0, 28, 28, 31, 32, 42\]|
    |Recovered Trend|The total number of recovered cases of COVID-19 since Jan 23, 2020, in the format of a dictionary.|\{"2020-01-21": 1, "2020-01-22": 1, "2020-01-23": 1, "2020-01-24": 2, "2020-01-25": 2, "2020-01-26": 3\}|
    |Deaths|The latest total number of deaths.|52|
    |Deaths Hist|The total number of deaths from COVID-19 since Jan 23, 2020, in the format of an array.|\[3, 17, 17, 24, 39, 52\]|
    |Deaths Trend|The total number of deaths from COVID-19 since Jan 23, 2020, in the format of a dictionary.|\{"2020-01-21": 1, "2020-01-22": 1, "2020-01-23": 1, "2020-01-24": 2, "2020-01-25": 2, "2020-01-26": 3\}|
    |New Confirmed Hist|The total number of suspected cases of COVID-19 since Jan 23, 2020, in the format of an array.|\[11, 0, 41, 0, 56, 127\]|
    |New Confirmed Trend|The total number of suspected cases of COVID-19 since Jan 23, 2020, in the format of a dictionary.|\{"2020-01-21": 1, "2020-01-22": 1, "2020-01-23": 1, "2020-01-24": 2, "2020-01-25": 2, "2020-01-26": 7\}|


## Instructions

1.  Log on to the [Log Service console](https://sls.console.aliyun.com/).

2.  In the Log Application section, click **Start** on the **COVID-19 Pandemic Analysis** card.

    ![Pandemic analysis - 010](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0706498951/p81955.png)

3.  Complete the initialization configuration and use the COVID-19 pandemic analysis application.

    You are required to initialize this application in your first attempt to use the application.


## FAQ

-   How do I delete the automatically created project?

    If you want to delete the automatically created project in the application, you can use the Cloud Shell to run the following command.

    ```
    aliyunlog log delete_project --project_name=ncp-$ALICLOUD_ACCOUNT_ID-cn-chengdu --region-endpoint=cn-chengdu.log.aliyuncs.com
    ```

    **Note:** If you delete the automatically created project, Logstores that you create in the project are also deleted.

-   How do I copy an existing dashboard to create a dashboard?
    1.  Click the Cloud Shell icon in the upper-right corner of the [Alibaba Cloud console](https://homenew.console.aliyun.com/).
    2.  Copy the following configurations of the dashboard to the local host.

        ```
        aliyunlog log get_dashboard --project=ncp-$ALICLOUD_ACCOUNT_ID-cn-chengdu --entity=covid-19_global --region-endpoint=cn-chengdu.log.aliyuncs.com > covid-19_global.json
        aliyunlog log get_dashboard --project=ncp-$ALICLOUD_ACCOUNT_ID-cn-chengdu --entity=covid-19_detail --region-endpoint=cn-chengdu.log.aliyuncs.com > covid-19_detail.json
        sed -i "s/\"dashboardName\": \"/\"dashboardName\": \"v2/g" covid-19_global.json
        sed -i "s/\"description\": \"\", \"displayName\": \"/\"description\": \"\", \"displayName\": \"v2/g" covid-19_global.json
        sed -i "s/\"dashboardName\": \"/\"dashboardName\": \"v2/g" covid-19_detail.json
        sed -i "s/\"description\": \"\", \"displayName\": \"/\"description\": \"\", \"displayName\": \"v2/g" covid-19_detail.json
        aliyunlog log create_dashboard --project=ncp-$ALICLOUD_ACCOUNT_ID-cn-chengdu --detail=file://./covid-19_global.json --region-endpoint=cn-chengdu.log.aliyuncs.com
        aliyunlog log create_dashboard --project=ncp-$ALICLOUD_ACCOUNT_ID-cn-chengdu --detail=file://./covid-19_detail.json --region-endpoint=cn-chengdu.log.aliyuncs.com
        ```

    3.  View the created dashboard.

        In the left-side navigation pane of the COVID-19 Pandemic Analysis page, click Setup. On the page that appears, click **Go to Overview Page**. In left-side navigation pane of the page that appears, click **Dashboard** to view the created dashboard.


## References

-   [Overview of Log Service](/intl.en-US/Product Introduction/What is Log Service?.md)
-   [Overview of dashboard](/intl.en-US/Query and visualization/Dashboard/Overview.md)

