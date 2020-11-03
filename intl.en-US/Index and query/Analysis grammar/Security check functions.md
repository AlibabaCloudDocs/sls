# Security check functions

Log Service provides security check functions based on the globally shared asset library of WhiteHat Security. This topic describes security check functions that you can use to identify whether an IP address, domain name, or the URL of a log is secure.

## Scenarios

You can use security check functions in the following scenarios:

-   Enterprises and institutions in industries such as the Internet, gaming, and information that require robust O&M services can use security check functions to identify suspicious requests or attacks. They can also use the functions to implement in-depth analysis and defend against potential attacks.
-   Enterprises and institutions in industries such as banking, financial securities, e-commerce that require strong protection for internal assets can use security check functions to identify risky access to suspicious websites and downloads initiated by trojans. This way, enterprises and institutions can take immediate actions to prevent potential losses.

## Features

Security check functions have the following features:

-   Reliability: Security check functions are based on the globally shared asset library of WhiteHat Security. The functions update immediately after the WhiteHat Security updates.
-   Efficiency: Security check functions can check millions of IP addresses, domain names, and URLs within seconds.
-   Ease of use: You can use the security\_check\_ip, security\_check\_domain, and security\_check\_url functions to seamlessly analyze network logs.
-   Flexibility: You can perform interactive queries, visualize query and analysis results, and configure alerts.

## Functions

|Function|Description|Example|
|:-------|:----------|:------|
|security\_check\_ip|Checks whether an IP address is secure.-   The value 1 indicates that the specified IP address is suspicious.
-   The value 0 indicates that the specified IP address is secure.

|select security\_check\_ip\(real\_client\_ip\)|
|security\_check\_domain|Checks whether a domain name is secure.-   The value 1 indicates that the specified domain name is suspicious.
-   The value 0 indicates that the specified domain name is secure.

|select security\_check\_domain\(site\)|
|security\_check\_url|Checks whether a URL is secure.-   The value 1 indicates that the specified URL is suspicious.
-   The value 0 indicates that the specified URL is secure.

|select security\_check\_domain\(concat\(host, url\)\)|

## Examples

-   Check external suspicious requests.

    For example, an e-commerce enterprise collects logs from its NGINX servers and wants to scan the clients for suspicious IP addresses. To do this, the enterprise can pass the ClientIP field in logs that are collected from the NGINX servers to the security\_check\_ip function and identify IP addresses whose returned value is 1. Then, the enterprise can view the distribution of the countries and ISPs of those IP addresses by displaying the result on a map.

    ```
    * | select ClientIP, ip_to_country(ClientIP) as country, ip_to_provider(ClientIP) as provider, count(1) as PV where security_check_ip(ClientIP) = 1 group by ClientIP order by PV desc
    ```

    ![Display the countries and ISPs on a map.](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6033713061/p8690.png)

-   Check internal suspicious access.

    For example, a securities operator collects logs of its internal devices that access the Internet by using gateways. To check whether a client has accessed suspicious websites, the operator can run the following statement. The operator can also save this statement as a saved search and configure an alert. This way, the alert is triggered when a client frequently accesses suspicious websites.

    ```
    * | select client_ip, count(1) as PV where security_check_ip(remote_addr) = 1 or security_check_site(site) = 1 or security_check_url(concat(site, url)) = 1 group by client_ip order by PV desc
    ```

    ![Configure alerts](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2984934061/p8691.png)


