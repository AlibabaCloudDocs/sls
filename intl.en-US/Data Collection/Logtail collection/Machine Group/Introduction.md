# Introduction

A machine group is a virtual group that contains multiple servers. Log Service uses machine groups to manage servers whose logs are to be collected by using Logtail.

You can create a machine group and add servers to the machine group in the Log Service console. Then you can create Logtail configurations for log collection and apply the configurations to the machine group. This way, you can collect logs from the servers based on the configurations.

To identify a machine group, you can use one of the following methods:

-   IP address: uses the IP addresses of the servers in the machine group as the identifier of the machine group. Each server in the group can be identified by using its unique IP address.
-   Custom ID: uses a custom ID to identify the machine group. Servers in the machine group have the same custom ID.

**Note:** You may want to collect logs from the servers that are provided by another cloud service provider, that reside in your on-premises data center, or belong to another Alibaba Cloud account. In this case, you must configure user identities for the servers before you add the servers to a machine group. For more information, see [Configure an account ID for a server](/intl.en-US/Data Collection/Logtail collection/Machine Group/Configure an account ID for a server.md).

## IP address-based machine groups

You can add the IP addresses of multiple servers to the identifier of a machine group. Then the servers are added to the machine group.

-   If you want to collect logs of Elastic Compute Service \(ECS\) instances, you can add the private IP addresses of the instances to the identifier of the machine group. However, you must make sure that the mapping between hostnames and IP addresses is not configured and the network types of the instances are not changed.
-   In other cases, add the IP addresses of the servers that Logtail automatically obtains after it is installed on the servers. The IP address of a server that Logtail obtains is indicated by the ip field. The field is recorded in the app\_info.json file of the server. Logtail can obtain a server IP address in different scenarios.
    -   If the hostname-to-IP address mapping is configured for the server in the /etc/hosts file, Logtail obtains the mapped IP address.
    -   If the hostname-to-IP address mapping is not configured for the server in the /etc/hosts file of the server, Logtail obtains the IP address of the first network interface card.

**Note:** Log data may not be transferred over the Alibaba Cloud internal network even if the IP addresses that you configure in the identifier of a machine group are internal IP addresses. If you select the **Alibaba Cloud internal network \(classic network or VPC\)** mode when you install Logtail on an ECS instance, logs are collected by using the Alibaba Cloud internal network.

## Custom ID-based machine groups

You can use a custom ID to identify a machine group in the following scenarios:

-   If your servers reside in multiple custom network environments such as virtual private clouds \(VPCs\), some IP addresses of the servers may be the same. In this case, Logtail cannot collect logs as expected. You can use a custom ID to prevent this issue.
-   Automatic scaling of a machine group. In this case, you only need to configure the same custom ID for new servers. Log Service identifies these servers and adds them to the machine group.

In most cases, a system consists of multiple modules. You can scale out each module by adding multiple servers to the module. To collect logs from the modules, you can create a machine group for each module. To identify the machine group of each module, you can create a custom ID for each machine group. For example, a website consists of an HTTP request processing module, a caching module, a logic processing module, and a storage module. The custom IDs of the machine groups that you create for these modules can be http\_module, cache\_module, logic\_module, and store\_module.

