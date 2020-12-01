# Anomaly comparison function

The anomaly comparison function compares the degree of differences of an observation object in two time ranges.

-   Function syntax 1
    -   Function expressions

        ```
        select anomaly_compare(long stamp, array[ feature_1, feature_2 ], long timePoint, long interval)
        select anomaly_compare(long stamp, array[ feature_1, feature_2 ], array[ feature1_name, feature2_name ], long timePoint, long interval)
        ```

    -   Input parameters

        |Parameter|Description|
        |---------|-----------|
        |stamp|The UNIX timestamp of the data.|
        |array\[features\]|The metrics of the observation object at a specific point in time.|
        |array\[featureNames\]|The description of the metrics.|
        |timePoint|The UNIX timestamp of the time when the observed object changes.|
        |interval|The interval at which data is collected. For example, if data is collected every 10 seconds, the interval is 10.|

-   Function syntax 2
    -   Function expressions

        ```
        select anomaly_compare(long stamp, array[ feature_1, feature_2 ], array[ feature1_name, feature2_name ], long version)
        ```

    -   Input parameters

        |Parameter|Description|
        |---------|-----------|
        |stamp|The UNIX timestamp of the data.|
        |array\[features\]|The metrics of the observation object at a specific point in time.|
        |array\[featureNames\]|The description of the metrics.|
        |version|The version number of the time series.         -   A value of 0 indicates the raw data.
        -   A value of 1 indicates the new data. |

-   Result

    ```
    {
       "results" : [ {
         "attr" : "cpu",
         "anomalyScore" : 0.01106371634297909,
         "details" : {
           "left" : [ {
             "key" : "mean",
             "value" : 0.07002069952622482
           }, {
             "key" : "std",
             "value" : 0.1364542814430179
           }, {
             "key" : "median",
             "value" : 0.04467685956328345
           }, {
             "key" : "variance",
             "value" : 0.018619770924130346
           } ],
           "rightMetrics" : [ {
             "key" : "mean",
             "value" : 0.4472823405432968
           }, {
             "key" : "std",
             "value" : 0.22405908739288383
           }, {
             "key" : "median",
             "value" : 0.42513225830553775
           }, {
             "key" : "variance",
             "value" : 0.05020247464333195
           } ]
         }
       } ]
     }
    ```

-   Result description
    -   The mean, std, median, and variance methods are used for the statistics of a time series.
    -   If you specify the names of metrics, the names are included in the attr field. Otherwise, the prefix column\_ is concatenated with the array subscript of a metric as the name of the metric, for example, column\_0.
    -   The anomalyScore indicates the degree of difference of a feature metric. Value values: 0 to 1. If the value approaches 0, the degree of difference is low. If the value approaches 1, the degree of difference is high.
-   Example

    ![Anomaly comparison function - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3784688951/p96989.png)


