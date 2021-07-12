# Customize the UI of embedded pages

This topic describes how to set parameters to customize the UI of embedded pages in the Log Service console.

Log Service allows you to embed console pages in a self-managed website without the need to log on to the console. This simplifies log query and analysis. Log Service also provides a series of UI parameters. You can set these parameters to customize the UI of console pages that are embedded in a self-managed website. For more information, see [Embed console pages and share log data](/intl.en-US/Developer Guide/Other visualization methods/Embed console pages and share log data.md).

## URL format

UI parameters are encoded in the URL based on the following format:

```
https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/logsearch/$\{LogstoreName\}?Parameter1&Parameter2
```

**Note:**

-   All parameters except the $\{ProjectName\}, $\{LogstoreName\}, $\{savedsearchID\}, and $\{dashboardID\} must be encoded after the question mark \(?\) at the end of a URL.
-   You can set multiple parameters at a time. The parameters are separated by ampersands \(&\).
-   To use the dark theme, you can add theme=dark&sls\_iframe=true to a URL.

## Common parameters

The following table describes the common parameters that you can use to customize console pages.

|Parameter|Type|Required|Description|Example|
|:--------|:---|--------|:----------|:------|
|hideSidebar|boolean|No|Specifies whether to hide the left-side navigation pane.|hideSidebar=true|
|hideTopbar|boolean|No|Specifies whether to hide the Alibaba Cloud title bar.|hideTopbar=true|
|hiddenBack|boolean|No|Specifies whether to hide the back button on the Search & Analysis page of the Logstore.|hiddenBack=true|
|hiddenChangeProject|boolean|No|Specifies whether to hide the Switch button of the project.|hiddenChangeProject=true|
|hiddenOverview|boolean|No|Specifies whether to hide the Overview tab of the project.|hiddenOverview=true|
|ignoreTabLocalStorage|boolean|No|Specifies whether to close the accessed tabs.|ignoreTabLocalStorage=true|
|queryTimeType|long|No|Specifies the time range of a query. For more information, see [Table 1](#table_67f_tyr_y57). Valid values:-   1 to 26: Each integer in the interval indicates a time range.
-   -2: a custom time range \(relative\). You must specify a start time and end time. Example: start:-10m,end:now.
-   -3: a custom time frame. You must specify a start time and end time. Example: start:-2h,end:absolute.
-   99: a custom time range. If you set queryTimeType to 99, you must set the startTime and endTime parameters to timestamps.

|queryTimeType=1|
|startTime|Timestamp \(date\)|No|Specifies the start time of a query. This parameter is available only when you set queryTimeType to 99.|startTime=1547776643|
|endTime|Timestamp \(date\)|No|Specifies the end time of a query. This parameter is available only when you set queryTimeType to 99.|endTime=1547776731|

|queryTimeType|Description|
|:------------|:----------|
|1|1 minute \(relative\)|
|2|15 minutes \(relative\)|
|3|1 hour \(relative\)|
|4|4 hours \(relative\)|
|5|1 day \(relative\)|
|6|1 week \(relative\)|
|7|30 days \(relative\)|
|8|1 minute \(time frame\)|
|9|15 minutes \(time frame\)|
|10|1 hour \(time frame\)|
|11|4 hours \(time frame\)|
|12|1 day \(time frame\)|
|13|1 week \(time frame\)|
|14|30 days \(time frame\)|
|15|Today \(time frame\)|
|16|Yesterday \(time frame\)|
|17|The day before yesterday \(time frame\)|
|18|This week \(time frame\)|
|19|Previous week \(time frame\)|
|20|This month \(time frame\)|
|21|This quarter \(time frame\)|
|22|Today \(relative\)|
|23|5 minutes \(relative\)|
|24|This year \(time frame\)|
|25|This month \(relative\)|
|26|Previous month \(time frame\)|
|27|This week \(relative\)|
|99|Specifies a custom time range. If you set queryTimeType to 99, you must specify the startTime and endTime parameters.|

The following examples show the URL parameters and results:

-   To hide the back button, Switch button, Overview tab, and the Alibaba Cloud title bar, use the following URL:

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/logsearch/$\{LogstoreName\}?hiddenBack=true&hiddenChangeProject=true&hiddenOverview=true&hideTopbar=true
    ```

    ![Hide settings](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3742503261/p143369.png)

-   To hide the left-side navigation pane, use the following URL:

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/logsearch/$\{LogstoreName\}?hideSidebar=true
    ```

    ![Hide the left-side navigation pane](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3742503261/p120922.png)

-   To hide the back button on the Search & Analysis page of a specified Logstore, use the following URL:

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/logsearch/$\{LogstoreName\}?hiddenBack=true
    ```

    ![Hide the back button in the left-side navigation pane](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3742503261/p120923.png)

-   To set the query and analysis time, use the following URL:

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/logsearch/$\{LogstoreName\}?queryTimeType=3
    ```

    ![queryTimeType](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3742503261/p37611.png)


## Parameters of the Search & Analysis page

The following table describes the parameters that you can set to customize the Search & Analysis page of a Logstore.

|Parameter|Type|Required|Description|Example|
|:--------|:---|--------|:----------|-------|
|ProjectName|string|Yes|The name of the project.|website-01|
|LogstoreName|string|Yes|The name of the Logstore.|logstore01|
|queryString|string|No|The query statement that is encoded in Base64. For example, the `*|select count(*)` statement is `KnxzZWxlY3QgY291bnQoKik=` after it is encoded in Base64.

|KnxzZWxlY3QgY291bnQoKik=|
|readOnly|boolean|No|Specifies whether to hide the buttons on the Search & Analysis page, such as **Share**, **Index Attributes**, **Save Search**, and **Save as Alert**.|readOnly=true|
|encode|string|No|Specifies whether to encode a query string. To prevent special characters in the value of the queryString parameter, you can set the encode parameter to base64. Then, the value of the queryString parameter is a Base64-encoded string.|encode=base64|
|hiddenEtl|boolean|No|Specifies whether to hide the Data Transformation button.|hiddenEtl=true|
|hiddenShare|boolean|No|Specifies whether to hide the Share button.|hiddenShare=true|
|hiddenIndexSetting|boolean|No|Specifies whether to hide the Index Attributes button.|hiddenIndexSetting=true|
|hiddenSavedSearch|boolean|No|Specifies whether to hide the Save Search button.|hiddenSavedSearch=true|
|hiddenAlert|boolean|No|Specifies whether to hide the Save as Alert button.|hiddenAlert=true|
|hiddenQuickAnalysis|boolean|No|Specifies whether to hide the Quick Analysis search bar.|hiddenQuickAnalysis=true|
|hiddenDownload|boolean|No|Specifies whether to hide the download icon.|hiddenDownload=true|
|keyDispalyMode|string|No|Specifies the display mode of log entries. -   Single: Log entries are displayed in a single line.
-   Multi: Log entries are displayed in multiple lines.

|keyDispalyMode=single|

The following examples show the URL parameters and results:

-   To set a string for a query, use the following URL:

    For example, the `*|select count(*)` statement is `KnxzZWxlY3QgY291bnQoKik=` after it is encoded in Base64.

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/logsearch/$\{LogstoreName\}?encode=base64&queryString=KnxzZWxlY3QgY291bnQoKik=
    ```

    ![Query statements](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3424285261/p277551.png)

-   To hide buttons such as **Index Attributes** and **Save as Alert**, use the following URL:

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/logsearch/$\{LogstoreName\}?readOnly=true
    ```

    ![Set the readOnly parameter](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3742503261/p37613.png)


## Parameters on the saved search page of a Logstore

The following table describes the parameters that you can set to customize the saved search page of a Logstore.

|Parameter|Type|Required|Description|Example|
|:--------|:---|--------|:----------|:------|
|ProjectName|string|Yes|The name of the project.|website-01|
|savedSearchName|string|No|The name of the saved search.|quick-search01|
|savedsearchID|string|Yes|The ID of the saved search. **Note:** After you create a saved search, you can obtain the ID of the saved search in the URL. For more information, see [Obtain the ID of a saved search](/intl.en-US/Index and query/Query/Saved search.md).

|savedsearch-1621845672511-314813|

The following example shows the URL parameters and result:

```
https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/savedsearch/$\{savedsearchID\}
```

![Saved search page](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3742503261/p120924.png)

## Embedded parameters of dashboards

The following table describes the embedded parameters that you can set to customize dashboard pages.

|Parameter|Type|Required|Description|Example|
|:--------|:---|--------|:----------|:------|
|ProjectName|string|Yes|The name of the project.|website-01|
|dashboardName|string|No|The name of the dashboard.|Website Logs|
|dashboardID|string|Yes|The ID of the dashboard. **Note:** After you create a dashboard, you can obtain the ID of the dashboard in the URL. For more information, see [Description](/intl.en-US/Developer Guide/API Reference/Dashboard-related API operations/ListDashboard.md).

|dashboard-1609817292009-742588|
|filters|string|No|The filter conditions. The value of this parameter must be transcoded by encodeURIComponent\(\). For example, `filters=key1:value1&filters=key2:value2` is transcoded to `filters%3Dkey1%3Avalue1%26filters%3Dkey2%3Avalue2`.

|filters%3Dkey1%3Avalue1%26filters%3Dkey2%3Avalue2|
|token|JsonString|No|The variables to be replaced. The value of this parameter must be transcoded. `"projectname","value":"1"}, {"key": "region", "value": For example,token=[{"key":"hangzhou"}]` is transcoded to `token%3D%5B%7B%22key%22%3A%20%22projectname%22%2C%22value%22%3A%221%22%7D%2C%20%7B%22key%22%3A%20%22region%22%2C%20%22value%22%3A%20%22hangzhou%22%7D%5D`.

|token%3D%5B%7B%22key%22%3A%20%22projectname%22%2C%22value%22%3A%221%22%7D%2C%20%7B%22key%22%3A%20%22region%22%2C%20%22value%22%3A%20%22hangzhou%22%7D%5D|
|readOnly|boolean|No|Specifies whether to hide the buttons on the dashboard page, such as the **Edit** and **Alerts** buttons.|readOnly=true|
|hiddenFilter|boolean|No|Specifies whether to hide the filter conditions.|hiddenFilter=true|
|hiddenToken|boolean|No|Specifies whether to hide the variables to be replaced.|hiddenToken=true|
|hiddenProject|boolean|No|Specifies whether to hide the project information.|hiddenProject=true|
|hiddenEdit|boolean|No|Specifies whether to hide the Edit button.|hiddenEdit=true|
|hiddenReport|boolean|No|Specifies whether to hide the Subscribe button.|hiddenReport=true|
|hiddenTitleSetting|boolean|No|Specifies whether to hide the title setting button.|hiddenTitleSetting=true|
|hiddenReset|boolean|No|Specifies whether to hide the Reset Time button.|hiddenReset=true|
|autoFresh|string|No|The interval at which the dashboard is refreshed, for example, 30 seconds or 5 minutes. The minimum refresh interval is 15 seconds.|autoFresh=5m|

The following examples show the URL parameters and results:

-   To enable read-only mode for the dashboard page, use the following URL:

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/dashboard/$\{dashboardID\}?readOnly=true 
    ```

    ![Read-only dashboards](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3742503261/p37614.png)

-   To add two filter conditions of key 1=value 1 and key 2=value 2, use the following URL. The filter condition `filters=key1:value1&filters=key2:value2` is transcoded to `filters%3Dkey1%3Avalue1%26filters%3Dkey2%3Avalue2`.

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/dashboard/$\{dashboardID\}?filters%3Dkey1%3Avalue1%26filters%3Dkey2%3Avalue2
    ```

    ![Filter dashboards](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3742503261/p38975.png)

-   To add multiple conditions projectname=1 and region=hangzhou to replace variables, use the following URL. token=\[\{"key": "projectname","value":"1"\}, \{"key": "region", "value":`"projectname","value":"1"}, {"key": "region", "value": "hangzhou"}]` is transcoded to `token%3D%5B%7B%22key%22%3A%20%22projectname%22%2C%22value%22%3A%221%22%7D%2C%20%7B%22key%22%3A%20%22region%22%2C%20%22value%22%3A%20%22hangzhou%22%7D%5D`.

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/dashboard/$\{dashboardID\}?token%3D%5B%7B%22key%22%3A%20%22projectname%22%2C%22value%22%3A%221%22%7D%2C%20%7B%22key%22%3A%20%22region%22%2C%20%22value%22%3A%20%22hangzhou%22%7D%5D
    ```

    ![Specify the token parameter in the URL to replace variables](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3742503261/p38976.png)

-   To refresh a dashboard every 5 minutes, use the following URL:

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/dashboard/$\{dashboardId\}?autoFresh=5m
    ```

    ![Automatic refresh](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4742503261/p47224.png)


## Tree structure parameters

The following table describes the tree structure parameters that you can set to customize the left-side navigation pane of the console.

|Parameter|Type|Required|Description|Example|
|---------|----|--------|-----------|-------|
|treeConfig|JSON|No|Specifies the tree structure of the left-side navigation pane. If you set treeEncode to base64, you need to transcode the value of treeConfig by using Base64 first. For example, `{"logstore":{"expand":true,"resourceList":["delete-log"],"template":["savedsearch","alert"]}}` is transcoded to `eyJsb2dzdG9yZSI6eyJleHBhbmQiOnRydWUsInJlc291cmNlTGlzdCI6WyJkZWxldGUtbG9nIl0sInRlbXBsYXRlIjpbInNhdmVkc2VhcmNoIiwiYWxlcnQiXX19` by using Base64.

|eyJsb2dzdG9yZSI6eyJleHBhbmQiOnRydWUsInJlc291cmNlTGlzdCI6WyJkZWxldGUtbG9nIl0sInRlbXBsYXRlIjpbInNhdmVkc2VhcmNoIiwiYWxlcnQiXX19|
|treeEncode|string|No|The encoding method for treeConfig. Default value: null. This value indicates that treeConfig is not encoded. Only Base64 is supported.|treeEncode=base64|

The following example shows the related parameters in the treeConfig parameter:

```
{
    "logstore": { 
        "search": true, 
        "expand": true, 
        "resourceList": [ 
            "L1",
            "L2"
        ],
        "template": [
            "favor", 
            "logtail",
            "import", 
            "etl", 
            "savedsearch",
            "alert",
            "export", 
            "consumergroup", 
            "dashboard" 
        ]
    },
    "machineGroup": {
        "search": true,
        "resourceList": [
            "m1",
            "m2"
        ]
    },
    "savedSearch": {
        "search": true,
        "resourceList": [
            "s1",
            "s2"
        ]
    },
    "alarm": {
        "search": true,
        "resourceList": [
            "a1",
            "a2"
        ]
    },
    "dashboard": {
        "search": true,
        "resourceList": [
            "d1",
            "d2"
        ]
    },
    "etl": {
        "search": true,
        "resourceList": [
            "e1",
            "e2"
        ]
    }
}
```

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|logstore|object|No|Specifies whether to display the nodes in the left-side navigation pane of the Logstores tab.|
|template|string\[\]|No|Specifies whether to display the icons on the Logstores tab. For more information, see [Table 4](#table_4ta_5g9_u9i).|
|machineGroup|object|No|Specifies whether to display the nodes in the left-side navigation pane of the Machine Groups tab.|
|savedSearch|object|No|Specifies whether to display the nodes in the left-side navigation pane of the Saved Search tab.|
|alert|object|No|Specifies whether to display the nodes in the left-side navigation pane of the Alerts tab.|
|dashboard|object|No|Specifies whether to display the nodes in the left-side navigation pane of the Dashboard tab.|
|etl|object|No|Specifies whether to display the nodes in the left-side navigation pane of the Data Transformation tab.|

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|search|boolean|No|Specifies whether to display the search box. Default value: true. This value indicates that the search box is displayed.|
|resourceList|String\[\]|No|Displays the resource list of the specified resource. If this parameter is set to an empty array, an empty list is displayed. If this parameter is unspecified, all resources are displayed in the list based on an exact match. By default, all resources of the list are displayed.|
|expand|boolean|No|Specifies whether to expand the list. Default value: false. This value indicates that the list is not expanded. This parameter is valid only for the Logstore list.|

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|favor|string|No|The watchlist.|
|logtail|string|No|The Logtail configurations.|
|import|string|No|The data import configurations.|
|etl|string|No|The data transformation tasks.|
|savedsearch|string|No|The saved searches.|
|alert|string|No|The alerts.|
|export|string|No|The data export tasks.|
|consumergroup|string|No|The data consumption.|
|dashboard|string|No|The visual dashboards.|

The following example shows the URL parameters and result:

To customize the left-side navigation pane, use the following URL:

```
https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/logsearch/$\{LogstoreName\}?treeconfig=eyJsb2dzdG9yZSI6eyJleHBhbmQiOnRydWUsInJlc291cmNlTGlzdCI6WyJkZWxldGUtbG9nIl0sInRlbXBsYXRlIjpbInNhdmVkc2VhcmNoIiwiYWxlcnQiXX19&hiddenBack=true&hiddenChangeProject=true&hiddenOverview=true&hideTopbar=true&treeEncode=base64&ignoreTabLocalStorage=true
```

![Tree structure](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4424285261/p143404.png)

## Advanced dashboard parameters

If you embed an inline frame into a dashboard, the height of the inline frame cannot be calculated. The scroll bar of the dashboard and the scroll bar of the inline frame may appear at the same time. In this case, you can set advanced dashboard parameters to adjust the dashboard height.

To fix this issue, you can use the postMessage method of Log Service to obtain the height of the dashboard and specify the height for the inline frame. The following sample code is used:

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>POST message test</title>
</head>
<style>
  * {
    padding: 0;
    margin: 0;
  }

  iframe {
    display: block;
    width: 100%;
  }
</style>
<body>
  <script>
    window.addEventListener('message',function(e){
      console.log(e.data.dashboardHeight)
      document.getElementById('test').style.height = e.data.dashboardHeight + 'px'
    });
  </script>
  <div style="height: 700px;">somethings</div>
  <iframe id="test" src="http://sls4service.console.aliyun.com/lognext/project/${projectName}/dashboard/${dashboardName}?hideTopbar=true&product=${productCode}">
</body>
</html>
```

