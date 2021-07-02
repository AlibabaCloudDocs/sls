# miHoYo

Log Service teams up with miHoYo to support the online game named Genshin Impact from the internal test and public preview to the commercial release. Log Service helps miHoYo manage logs from tens of millions of players. Log Service is highly recognized and praised by miHoYo for outstanding performance and stability.

## Company profile

miHoYo, founded in 2012, focuses on mobile games and comics that are originated from Chinese animation. miHoyo is a video game development and animation studio that has successively released excellent and widely favored mobile games based on Chinese animations, such as Zombiegal Kawaii, Guns GirlZ, and Honkai Impact 3. In 2020, miHoYo released Genshin Impact, which is the first open-world mobile game of anime, comics, and games \(ACG\) subculture. Only one month after its release, Genshin Impact has became the most profitable mobile game around the world compared with its counterparts. For more information, visit [the official website of miHoYo](https://www.mihayo.com/company.html).

## Scenarios

Genshin Impact is an open-world adventure game that is developed and released by miHoYo. Genshin Impact was available for public preview on September 28, 2020. After Genshin Impact was released, it turned to be a phenomenal success around the world. Genshin Impact has become a popular game with a large number of players even though the game is a new comer in the market. Therefore, miHoYo requires a logging platform with high stability, elastic scalability, and high performance to meet its growing requirements on data analysis. This way, miHoYo can fulfill intensive operations for Genshin Impact.

## Challenges

The Genshin Impact team faces the following challenges after Genshin Impact was released:

-   Data growth

    Only one month after the release of Genshin Impact, the number of active players per month exceeded 10 million. The data volume is also increasing at a high speed with the rapid growth of players and the optimization of the operation system. Therefore, the Genshin Impact team requires a solution to query and analyze large amounts of data with high performance.

-   Centralized data collection

    Genshin Impact becomes popular worldwide and tops the App Store bestseller list in the United States, Germany, Canada, South Korea, and Singapore. However, the globalization of the game also increases the difficulty in centralized data collection and requires higher stability.

-   Traffic surges

    Genshin Impact is an open-world adventure game that offers promotional activities based on different seasons and cultural backgrounds of each region in the game map. Holiday activities and version updates also result in traffic surges. Therefore, the Genshin Impact team requires a logging platform that has the elastic scaling feature to implement rapid scaling during peak hours.


## Solution

To meet the requirements of the Genshin Impact team on the performance, data collection, and elastic scalability of its business monitoring platform, Alibaba Cloud provides Log Service as the solution to help the team fix the issues that occur during data collection, query, and monitoring.

-   High performance: Log Service can process billions of rows of data within seconds. Log Service supports ten s of petabytes of data throughput with an SLA of 99.95%. Log Service provides multiple features that can be used to extract, aggregate, and visualize heterogeneous data. Log Service also provides the alerting and AIOps-based anomaly detection features. The team can use these features to collect and analyze various types of data based on specific business scenarios. The data includes the monitoring logs of business and services, the operation and audit logs of cloud services, and the metrics of game operations.

    ![miHoYo_Solution_1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9643694261/p271790.png)

-   Centralized data collection: Log Service supports more than 50 data sources. Data from clients, web pages, or protocols can be collected by using SDKs or APIs. Log Service provides the resumable upload feature to ensure the reliability of data collection. Log Service also supports multiple transmission methods in different scenarios by combining the internal network and a virtual private cloud \(VPC\), or by using the Internet based on global acceleration. Log Service helps the Genshin Impact team collect service logs from different regions with high efficiency and reliability and manage the logs in a unified manner.

    ![miHoYo_Solution_2](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9643694261/p271792.png)

-   Elastic scalability: Log Service supports linearly elastic scaling for petabytes of data per day based on the actual traffic. Log Service is adaptive to the traffic surges that are caused by promotional activities in Genshin Impact. The Genshin Impact team uses Log Service to overcome the instability caused by frequent traffic surges.


