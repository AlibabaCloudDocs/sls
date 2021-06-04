# Multi-set operations

Log Service supports associated monitoring for multiple data sets.

## Set operations

In the alert monitoring system of Log Service, a query result is considered as a set. Associated monitoring of multiple sets is supported.

**Note:**

-   Log Service supports associated monitoring for up to three sets.
-   By default, only the first 1,000 rows of data in a query result are selected for set operations. If you specify three query statements and do not set the **Set Operations** parameter to **No Merge**, only the first 100 rows of data in each query result are selected.
-   If you specify three query statements, the returned three query results are considered as three sets. In this case, the first two sets are used for a set operation, and a result is returned. Then, this result and the third set are used for a set operation. Examples:
    -   Set A LEFT JOIN Set B LEFT JOIN Set C: The LEFT JOIN operation is first performed on Set A and Set B, and a result is returned. Then, the LEFT JOIN operation is performed on the result and Set C.
    -   Set A JOIN Set B INNER JOIN Set C: The JOIN operation is first performed on Set A and Set B, and a result is returned. Then, the INNER JOIN operation is performed on the result and Set C.
    -   Set A LEFT EXCLUDE JOIN Set B No Merge Set C: The LEFT EXCLUDE JOIN operation is performed on Set A and Set B, and Set C is ignored.

![Multi-set operations](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9168902261/p263364.png)

|Set operation|Illustration|Description|
|-------------|------------|-----------|
|No Merge|![No Merge](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9168902261/p263319.png)

|The specified two sets are not associated. Set A is a query result. The fields in Set B are only used as alert template variables that can be quoted in alert notifications. |
|CROSS JOIN|None|Arbitrary data from Set A combines with arbitrary data from Set B. This set operation is used to filter data for further evaluation.|
|JOIN|![JOIN](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9168902261/p263320.png)

|Data in Set B is added to Set A and aligned by field.|
|INNER JOIN|![INNER JOIN](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9168902261/p263321.png)

|Only the data that exists in Set B is retained in Set A. Set B is the whitelist of Set A.|
|LEFT JOIN|![LEFT JOIN](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9168902261/p263325.png)

|Partial data from Set B is complemented to Set A. Set B is the dimension table of Set A.|
|RIGHT JOIN|![RIGHT JOIN](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9168902261/p263327.png)

|Partial data from Set A is complemented to Set B. Set A is the dimension table of Set B.|
|FULL JOIN|![FULL JOIN](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9168902261/p263329.png)

|Set A and Set B complement each other.|
|LEFT EXCLUDE JOIN|![LEFT EXCLUDE JOIN](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9168902261/p263330.png)

|Data that exists in Set B is deleted in Set A. Set B is the blacklist of Set A.|
|RIGHT EXCLUDE JOIN|![RIGHT EXCLUDE JOIN](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9168902261/p263332.png)

|Data that exists in Set A is deleted in Set B. Set A is the blacklist of Set B.|

## Examples

-   No Merge
    -   Requirement

        NGINX access logs are monitored. If the number of 5xx error responses exceeds 500 every 15 minutes, an alert is triggered and an alert notification is sent. The information of the hosts that meet the trigger condition is included in the alert notification.

    -   Configurations

        ![No Merge](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9168902261/p263399.png)

    -   Results
        -   Result of Query Statistics 0

            The number of 5xx error responses within 15 minutes is calculated.

            |cnt|
            |---|
            |1234|

        -   Result of Query Statistics 1

            The top 5 hosts with the most 5xx error responses within 15 minutes and the related number of errors are calculated.

            |host|pv|
            |----|--|
            |host1|60|
            |host2|55|
            |host3|47|
            |host4|45|
            |hosg5|30|

        -   Result of the set operation

            If you set the **Set Operations** parameter to **No Merge**, the result of the set operation is the result of Query Statistics 0.

-   JOIN
    -   Example 1
        -   Requirement

            Two Logstores are used to store NGINX access logs. One Logstore resides in the China \(Beijing\) region, and the other Logstore resides in the China \(Shanghai\) region. The number of hosts that have more than 30 5xx error responses is calculated every 15 minutes. If the number of hosts that meet the specified condition in the two Logstores exceeds 10, an alert is triggered.

        -   Configurations

            ![JOIN](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9168902261/p263407.png)

        -   Results
            -   Result of Query Statistics 0

                The hosts whose number of 5xx error responses within 15 minutes exceeds 30 and the related number of errors are calculated.

                |host|pv|
                |----|--|
                |host1|60|
                |host2|55|
                |host3|47|
                |host4|45|
                |hosg5|31|

            -   Result of Query Statistics 1

                The hosts whose number of 5xx error responses within 15 minutes exceeds 30 and the related number of errors are calculated.

                |host|pv|
                |----|--|
                |hosta|70|
                |hostb|45|
                |hostc|44|
                |hostd|42|

            -   Result of the set operation

                If you set the **Set Operations** parameter to **JOIN**, the result of the set operation is displayed in the following table.

                |host|pv|
                |----|--|
                |host1|60|
                |host2|55|
                |host3|47|
                |host4|45|
                |hosg5|31|
                |hosta|70|
                |hostb|45|
                |hostc|44|
                |hostd|42|

    -   Other examples
        -   If the fields in two query results are not completely matched, the non-matching fields are left blank after a set operation is performed.
            -   Result of Query Statistics 0

                |a|b|
                |--|--|
                |a1|b1|
                |a2|b2|

            -   Result of Query Statistics 1

                |b|c|
                |--|--|
                |b1|c1|
                |b2|c2|

            -   Result of the set operation

                |a|b|c|
                |--|--|--|
                |a1|b1|None|
                |a2|b2|None|
                |None|b1|c1|
                |None|b2|c2|

        -   If you specify three query statements, the results of three Query Statistics are used for set operations. In this case, the results of Query Statistics 0 and Query Statistics 1 are used for a set operation, and a result is returned. Then, the JOIN operation is performed on the preceding result and the result of Query Statistics 2.
            -   Result of Query Statistics 0

                |a|b|
                |--|--|
                |a1|b1|
                |a2|b2|

            -   Result of Query Statistics 1

                |a|b|
                |--|--|
                |a1|b11|
                |a2|b22|
                |a3|b33|

            -   Operation result of the Query Statistics 0 result and Query Statistics 1 result

                If you set the **Set Operations** parameter to **INNER JOIN**,**$0.a == $1.a**, the result of the set operation is displayed in the following table.

                |a|$0.b|$1.b|
                |--|----|----|
                |a1|b1|b11|
                |a2|b2|b22|

            -   Result of Query Statistics 2

                |a|b|
                |--|--|
                |a3|b333|
                |a4|b444|

            -   Result of the set operation

                If you set the **Set Operations** parameter to **JOIN**, the result of the set operation is displayed in the following table.

                **Note:** The b field in the result of Query Statistics 2 is aligned with the $0.b field.

                |a|$0.b|$1.b|
                |--|----|----|
                |a1|b1|b11|
                |a2|b2|b22|
                |a3|b333|None|
                |a4|b444|None|

-   INNER JOIN
    -   Example 1
        -   Requirement

            The number of 5xx error responses in each specified bucket is monitored. If the number of 5xx error responses within 15 minutes exceeds 1,000, an alert is triggered. To meet this requirement, you must add resource data to maintain the bucket whitelist.

        -   Configurations

            ![INNER JOIN](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0268902261/p263417.png)

        -   Results
            -   Result of Query Statistics 0

                The buckets whose number of 5xx error responses exceeds 1,000 within 15 minutes are calculated.

                |bucket|pv|
                |------|--|
                |bucket\_01|1600|
                |bucket\_02|1550|
                |bucket\_03|1470|
                |bucket\_04|1450|

            -   Result of Query Statistics 1

                The resource data of the specified buckets.

                |bucket|desc|
                |------|----|
                |bucket\_03|for dev team|
                |bucket\_04|for test team|
                |bucket\_05|for service team|
                |bucket\_06|for support team|

            -   Result of the set operation

                If you set the **Set Operations** parameter to **INNER JOIN**,**$0.bucket == $1.bucket**, the result of the set operation is displayed in the following table.

                |bucket|pv|desc|
                |------|--|----|
                |bucket\_03|47|for dev team|
                |bucket\_04|45|for test team|

    -   Example 2
        -   Requirement

            Two Logstores are used to store NGINX access logs. One Logstore resides in the China \(Beijing\) region, and the other Logstore resides in the China \(Shanghai\) region. The clients that have more than 30 5xx error responses are calculated every 15 minutes. If both Logstores have 5xx error responses, and the number of errors in the China \(Beijing\) region is greater than that in the China \(Shanghai\) region, an alert is triggered.

        -   Configurations

            ![INNER JOIN](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0268902261/p265931.png)

        -   Results
            -   Result of Query Statistics 0

                The clients whose number of 500 error responses within 15 minutes exceed 30 and the related number of errors are calculated. These errors occur in the China \(Beijing\) region.

                |client\_ip|pv|
                |----------|--|
                |192.0.2.4|60|
                |192.0.2.5|55|
                |192.0.2.6|47|
                |192.0.2.7|45|
                |192.0.2.8|31|

            -   Result of Query Statistics 1

                The clients whose number of 500 error responses within 15 minutes exceed 30 and the related number of errors are calculated. These errors occur in the China \(Shanghai\) region.

                |client\_ip|pv|
                |----------|--|
                |192.0.2.5|70|
                |192.0.2.6|45|
                |192.0.2.7|44|
                |192.0.2.8|42|
                |192.0.2.9|42|

            -   Result of the set operation

                If you set the **Set Operations** parameter to **INNER JOIN**,**$0.client\_ip == $1.client\_ip**,**$0.pv \> $1.pv**, the result of the set operation is displayed in the following table.

                |client\_ip|pv|
                |----------|--|
                |192.0.2.6|47|
                |192.0.2.7|45|

    -   Other examples

        For example, a field in the result of Query Statistics 0 and a field in the result of Query Statistics 1 are not associated but share the same name. In this case, the two fields in the result of the specified set operation are automatically prefixed by $0 and $1.

        -   Result of Query Statistics 0

            |a|b|c|d|
            |--|--|--|--|
            |a1|b1|c1|d1|
            |a2|b2|c2|d2|
            |a3|b3|c3|d3|

        -   Result of Query Statistics 1

            |a|b|c|
            |--|--|--|
            |a1|b11|c11|
            |a2|b22|c22|

        -   Result of the set operation

            If you set the **Set Operations** parameter to **INNER JOIN**,**$0.a == $1.a**, the result of the set operation is displayed in the following table.

            |a|$0.b|$0.c|d|$1.b|$1.c|
            |--|----|----|--|----|----|
            |a1|b1|c1|d1|b11|c11|
            |a2|b2|c2|d2|b22|c22|

-   LEFT EXCLUDE JOIN
    -   Requirement

        The number of 5xx error responses in each bucket that is not specified is monitored. If the number of 5xx errors within 15 minutes exceeds 1,000, an alert is triggered. To meet this requirement, you must add resource data to maintain the bucket blacklist.

    -   Configurations

        ![LEFT EXCLUDE JOIN](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0268902261/p263428.png)

    -   Results
        -   Result of Query Statistics 0

            The buckets whose number of 500 error responses exceeds 1,000 within 15 minutes are calculated.

            |bucket|pv|
            |------|--|
            |bucket\_01|60|
            |bucket\_02|55|
            |bucket\_03|47|
            |bucket\_04|45|

        -   Result of Query Statistics 1

            The resource data of the specified buckets.

            |bucket|desc|
            |------|----|
            |bucket\_03|for dev team|
            |bucket\_04|for test team|

        -   Result of the set operation

            If you set the **Set Operations** parameter to **LEFT EXCLUDE JOIN**,**$0.bucket == $1.bucket**, the result of the set operation is displayed in the following table.

            |bucket|pv|
            |------|--|
            |bucket\_01|60|
            |bucket\_02|55|

-   RIGHT EXCLUDE JOIN
    -   Requirement

        The number of 5xx error responses in each bucket that is not specified is monitored. If the number of 5xx error responses within 15 minutes exceeds 1,000, an alert is triggered. To meet this requirement, you must add resource data to maintain the bucket blacklist.

    -   Configurations

        ![RIGHT EXCLUDE JOIN](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0268902261/p263429.png)

    -   Results
        -   Result of Query Statistics 0

            The resource data of the specified buckets.

            |bucket|desc|
            |------|----|
            |bucket\_03|for dev team|
            |bucket\_04|for test team|

        -   Result of Query Statistics 1

            The buckets whose number of 500 error responses exceeds 1,000 within 15 minutes are calculated.

            |bucket|pv|
            |------|--|
            |bucket\_01|60|
            |bucket\_02|55|
            |bucket\_03|47|
            |bucket\_04|45|

        -   Result of the set operation

            If you set the **Set Operations** parameter to **RIGHT EXCLUDE JOIN**,**$0.bucket == $1.bucket**, the result of the set operation is displayed in the following table.

            |bucket|pv|
            |------|--|
            |bucket\_01|60|
            |bucket\_02|55|

-   CROSS JOIN
    -   Example 1
        -   Requirements

            Object Storage Service \(OSS\) access logs and Server Load Balancer \(SLB\) access logs are monitored at the same time. The number of 4xx error responses in OSS and the number of 5xx error responses in SLB are calculated every 15 minutes. If the total number of errors reaches 1,000, an alert is triggered.

        -   Configurations

            ![CROSS JOIN](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7394972261/p265331.png)

        -   Results
            -   Result of Query Statistics 0

                The number of 4xx error responses within 15 minutes in OSS is calculated.

                |pv|
                |--|
                |890|

            -   Result of Query Statistics 1

                The number of 5xx error responses within 15 minutes in SLB is calculated.

                |pv|
                |--|
                |567|

            -   Result of the set operation

                If you set the **Set Operations** parameter to **CROSS JOIN**, the result of the set operation is displayed in the following table.

                |$0.pv|$1.pv|
                |-----|-----|
                |890|567|

    -   Other examples
        -   Result of Query Statistics 0

            |a|b|
            |--|--|
            |a1|b1|
            |a2|b2|
            |a5|b5|

        -   Result of Query Statistics 1

            |a|c|
            |--|--|
            |a1|c1|
            |a3|c3|

        -   Result of the set operation

            If you set the **Set Operations** parameter to **CROSS JOIN**, the result of the set operation is displayed in the following table.

            |$0.a|b|$1.a|c|
            |----|--|----|--|
            |a1|b1|a1|c1|
            |None|None|a3|c3|
            |a2|b2|a1|c1|
            |None|None|a3|c3|
            |a5|b5|a1|c1|
            |None|None|a3|c3|

-   LEFT JOIN
    -   Result of Query Statistics 0

        |a|b|
        |--|--|
        |a1|b1|
        |a2|b2|
        |a3|b3|

    -   Result of Query Statistics 1

        |a|b|c|
        |--|--|--|
        |a1|b11|c1|
        |a2|b22|c2|

    -   Result of the set operation

        If you set the **Set Operations** parameter to **LEFT JOIN**,**$0.a == $1.a**, the result of the set operation is displayed in the following table.

        |a|$0.b|$1.b|c|
        |--|----|----|--|
        |a1|b1|b11|c1|
        |a2|b2|b22|c2|
        |a3|b3|None|None|

-   RIGHT JOIN
    -   Result of Query Statistics 0

        |a|b|c|
        |--|--|--|
        |a1|b11|c1|
        |a2|b22|c2|

    -   Result of Query Statistics 1

        |a|b|
        |--|--|
        |a1|b1|
        |a2|b2|
        |a3|b3|

    -   Result of the set operation

        If you set the **Set Operations** parameter to **RIGHT JOIN**,**$0.a == $1.a**, the result of the set operation is displayed in the following table.

        |a|$0.b|c|$1.b|
        |--|----|--|----|
        |a1|b11|c1|b1|
        |a2|b22|c2|b2|
        |a3|None|None|b3|

-   FULL JOIN
    -   Result of Query Statistics 0

        |a|b|c|
        |--|--|--|
        |a1|b1|c1|
        |a2|b2|c2|
        |a5|b5|c3|

    -   Result of Query Statistics 1

        |a|b|d|
        |--|--|--|
        |a1|b11|d1|
        |a2|b22|d2|
        |a3|b33|d3|

    -   Result of the set operation

        If you set the **Set Operations** parameter to **FULL JOIN**,**$0.a == $1.a**, the result of the set operation is displayed in the following table.

        |a|$0.b|c|$1.b|d|
        |--|----|--|----|--|
        |a1|b1|c1|b11|d1|
        |a2|b2|c2|b22|d2|
        |a5|b5|c3|None|None|
        |a3|None|None|b33|d3|


