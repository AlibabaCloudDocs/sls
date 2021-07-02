# Milian Technology

Log Service helps Milian Technology aggregate disperse data and improve troubleshooting efficiency. Log Service also provides Milian Technology with multiple data analysis methods. Milian Technology has improved its capabilities in IT O&M, data operations, and risk management by using Log Service.

## Company profile

Yidui is a brand of Beijing Milian Technology Co., Ltd. that was established in 2015. Milian Technology was awarded "National High Tech Enterprise" and "Zhongguancun New High-tech Enterprise" certifications. In 2019, Milian Technology reported a revenue of nearly CNY 1 billion. In 2020, Milian Technology completed Series B financing that is worth almost USD 100 million, led by Xiaomi and Shunwei Capital. Yidui is an online dating app that is committed to providing real-time Internet dating services. Yidui integrates real-time video calls, live streams, and matchmakers to provide single users with an innovative dating experience. Yidui has broken new ground for the video dating community and has become a highlight of the online matchmaking industry in 2019. For more information, visit [the official website of Yidui](https://www.520yidui.com/schooling/index.html?tab=introduce).

## Scenarios

Yidui is a real-time video dating platform that helps single users build relationships. Yidui provides various methods to help its users interact with each other by using text messages, voice calls, photos, and videos. Yidui can push the best matching live stream to its new users. The platform also provides the search feature to help users find love. In the early stage of its development, Yidui launched the search and recommender features based on Elasticsearch and MySQL. However, these features can no longer meet the increasing demands on data collection, processing, and analysis due to the growth of business and data volume and the upgrade of the service architecture. Therefore, Yidui requires a powerful platform to analyze large amounts of data and satisfy the requirements of increasing number of users.

![Milian Technology_Scenarios](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4392125261/p271799.png)

## Challenges

Yidui faces the following challenges:

-   Disperse data sources

    Yidui uses multiple compute and storage engines, such as databases, big data platforms, and third-party services. In this case, Yidui requires unified data planning and management to prevent data silos and improve the efficiency of development and management.

-   Traffic surges

    The growth of business and the increasing number of users and livestream matchmaking activities increase system complexity and generate more logs. However, the data query feature of the self-managed Elasticsearch platform is unavailable during traffic peaks. In addition, system errors cannot be identified or fixed with high efficiency, resulting in poor user experience.

-   Poor data analysis capabilities

    Yidui is dedicated to offering data-driven services, which cover statistical reports, algorithm-based recommendations, risk management, interactive operational queries, and behavior analysis. However, Yidui does not have powerful data analysis.


## Solution

To enable Yidui to address the preceding challenges, Alibaba Cloud provides Log Service to help Yidui aggregate disperse data from multiple sources, query data during traffic peaks, and analyze large amounts of data.

-   Unified log collection
    -   Website logs can be collected and then sent to Log Service by using the web tracking feature.
    -   App logs can be collected and then sent to Log Service by using Log Service SDK for iOS or Android.
    -   Server logs can be collected and then sent to Log Service by using Logtail.
-   Intelligent O&M platform

    Yidui has built an intelligent O&M platform based on Log Service. Yidui has built a platform to quickly analyze and fix exceptions to ensure the stability of its service system. Log Service provides unified log collection capabilities to help Yidui collect access logs from applications, systems, servers, databases, and network security services. Log Service helps Yidui query data within seconds. Log Service also provides the LogReduce feature and the AIOps-based anomaly detection feature, and supports multiple methods for alert notifications. The intelligent O&M platform is also used to improve user experience. Log Service provides the analysis feature for multidimensional metrics and enables Yidui to render analysis results into visualized charts.

-   Associated services

    To provide a unified data analysis service, Log Service connects with other Alibaba Cloud services, such as MaxCompute, StreamCompute, and Machine Learning Platform for AI.

    -   API Gateway: provides associated services with an egress gateway.
    -   Quick BI: supports drag-and-drop operations and provides various visualized charts for interactive reports.
    -   DataV: provides interactive data dashboards for data analysis.

![Milian Technology_Solution](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2744405261/p271800.png)

