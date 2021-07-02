# Chanjet

The O&M and development team of Chanjet uses Log Servcie to prevent frequent false alerts, detect suspicious sites, and identify errors. Log Service helps Chanjet improve the O&M efficiency and reduce the costs of O&M and communication. Log Service supports the healthy and stable operation of all cloud services of Chanjet and builds a benchmark in the field of IT O&M.

## Company profile

Chanjet Information Technology Co., Ltd. is a member enterprise of Yonyou. Chanjet aims to provide social, personalized, service-oriented, and small-scale business management support for micro and small enterprises. To help micro and small enterprises to fulfill digital transformation in terms of finance and management, Chanjet empowers these enterprises with advanced technologies. Chanjet helps them promote online business, change traditional business models, and maintain sustainable profit growth. To meet the various requirements of micro and small enterprises on cloud services, Chanjet utilizes the high-frequency interactions between Software as a Service \(SaaS\) business and customers to extract more value for the customers. Chanjet plans to expand its business from the SaaS market to the Backend as a Service \(BaaS\) market. Chanjet aims to become the largest one-stop service platform for micro and small enterprises in China. For more information, visit [the official website of Chanjet](https://www.chanjet.com/).

## Scenarios

The IT O&M and development team of Chanjet is responsible for developing cloud services, such as Haokuaiji, Haoshengyi, and Yidaizhang. This process includes the O&M, deployment, and release of these services. The team built a MIDAS intelligent O&M platform that provides data import, data processing, and scenario analysis features.

![Chanjet_Scenarios](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0443694261/p271777.png)

## Challenges

In the early development stage of the MIDAS intelligent O&M platform, Chanjet used the self-managed Elasticsearch, Logstash, and Kibana \(ELK\) stack to perform data analysis for O&M. In the process of business growth, an increasing number of applications and systems are connected to the MIDAS intelligent O&M platform. As a result, Chanjet faces various challenges, such as platform issues and service stability issues.

-   High concurrency

    Tens of thousands of sites send data at the same time. As a result, terabytes of logs and messages are generated per day. The self-managed ELK stack delivers poor performance, and a large number of development resources are required to improve the performance.

-   Heterogeneous data

    Different types of data such as access logs, system logs, application logs, notifications, and messages are collected in different formats. As a result, data cleansing is difficult.

-   Multiple sources

    Logs are collected from different data sources, such as networks, servers, mobile apps, websites, and Docker containers. The APIs that are used to import data require high real-time performance. In this case, unified data management is required.

-   Diversified requirements

    Each product department of Chanjet has specific requirements for the collected data. For example, a department needs to monitor alerts, troubleshoot issues, analyze or mine data, perform data mining, and subscribe to data analysis reports. In addition, the department needs to consume the data by using different methods.


## Solution

To resolve the preceding issues, Chanjet used Log Service to build the MIDAS intelligent O&M platform.

-   Efficient data collection and transmission

    Log Service has powerful data import capabilities. Chanjet uses Log Service to import different types of data that is in different formats from the networks, servers, mobile apps, and containers in its hybrid cloud architecture. The data includes access logs, system logs, application logs, and messages. Terabytes of data can be efficiently written to Log Service per day.

-   Flexible data processing and storage

    Chanjet has deployed a configuration management database \(CMDB\) and defined the relationships between the resources and each department. Chanjet uses Log Service to segment and serialize raw logs, and then analyzes the logs based on specific scenarios. Chanjet ships data to external services based on specific shipping policies. Then, the external services use Ansible or forward messages to manage the shipped data in a centralized manner and support subsequent scenario analysis.

-   Intelligent anomaly detection and location

    Chanjet built the MIDAS intelligent O&M platform by using the time series data analysis feature and supported functions of Log Service. For example, Chanjet uses the interval-valued comparison and periodicity-valued comparison functions to obtain the latest value of a metric in real time. After the specific values are obtained, the related alerts that are triggered based on the values have higher accuracy than the alerts that are triggered based on the original threshold. Chanjet uses the prediction and anomaly detection functions of Log Service to quickly locate exceptions from a large number of metrics, mark the exceptions, and identify system failures in a short time. To predict system failures, Chanjet uses Log Service to collect data from all modules of the MIDAS intelligent O&M platform, marks the collected data, and integrates application configurations with the data. Then, Chanjet performs root cause analysis on the system failures based on the time series.


The following figure shows the architecture of the MIDAS intelligent O&M platform that is built based on Log Service by Chanjet.

![Chanjet_Solution](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0443694261/p271779.png)

