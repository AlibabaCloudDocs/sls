# Customize the UI of embedded pages

This topic describes how to set parameters to customize the UI of embedded pages in the Log Service console.

Log Service allows you to embed console pages in a user-created website without the need to log on to the console. This simplifies log query and analysis. Log Service also provides a series of UI parameters. You can set these parameters to customize the UI of console pages that are embedded in a user-created website. For more information, see [Embed console pages and share log data](/intl.en-US/Developer Guide/Other visualization methods/Embed console pages and share log data.md).

## URL format

UI parameters are encoded in the URL based on the following format:

```
https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/logsearch/$\{LogstoreName\}?Parameter1&Parameter2
```

**Note:**

-   All parameters except the $\{ProjectName\} and $\{LogstoreName\} parameters must be encoded after the question mark \(?\) at the end of a URL.
-   You can set multiple parameters at a time. The parameters are separated by ampersands \(&\).
-   To use the dark theme, you can add theme=dark&sls\_iframe=true to a URL.

## Common parameters

The following table describes the common parameters that you can use to customize console pages.

|Parameter|Type|Required|Description|Example|
|:--------|:---|:-------|:----------|:------|
|hideSidebar|boolean|No|Specifies whether to hide the left-side navigation pane.|hideSidebar=true|
|hideTopbar|boolean|No|Specifies whether to hide the Alibaba Cloud title bar.|hideTopbar=true|
|hiddenBack|boolean|No|Specifies whether to hide the back button on the Search & Analysis page of the Logstore.|hiddenBack=true|
|hiddenChangeProject|boolean|No|Specifies whether to hide the Overview tab of the project.|hiddenOverview=true|
|hiddenOverview|boolean|No|Specifies whether to close the accessed tabs.|ignoreTabLocalStorage=true|
|keyFilter|JSON|No|Filters resources that are listed in the left-side navigation pane. The following resources can be filtered: -   logstore: Logstores
-   savedsearch: saved searches
-   dashboard: dashboards

**Note:**

-   A hyphen \(-\) indicates a fuzzy match.
-   The value of the parameter must be in the JSON format and encoded by using the encodeURI function.

|\{"logstore":\["logstore-xx"\],"savedsearch":\["savedsearch-xx"\],"dashboard":\["dashboard-xx"\]\}|
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

    ![Hide settings](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2389374161/p143369.png)

-   To hide the left-side navigation pane, use the following URL:

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/logsearch/$\{LogstoreName\}/?hideSidebar=true
    ```

    ![Hide the left-side navigation pane](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1651520061/p120922.png)

-   To hide the back button on the Search & Analysis page of a specified Logstore, use the following URL:

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/logsearch/$\{LogstoreName\}/?hiddenBack=true
    ```

    ![Hide the back button in the left-side navigation pane](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1651520061/p120923.png)

-   To filter the resources that are listed in the left-side navigation pane, use the following URL.

    To filter the resources that are listed in the left-side navigation pane, add and set the keyFilter parameter in a URL. The value of the keyFilter parameter is a JSON object. For example, assume that you want to show Logstores whose names contain aegis- and a Logstore whose name is 500osslog. You also want to show saved searches whose names contain oss, and dashboards whose names contain ddos.

    In this case, you can set the value of the keyFilter parameter to a JSON object in the format of \{"logstore":\["aegis-","500osslog"\],"savedserach":\["oss"\],"dashboard":\["ddos"\]\}. The value aegis- indicates that all Logstores whose names contain aegis are queried in a fuzzy match. The value 500osslog indicates that the Logstore whose name is 500osslog is queried in an exact match. You can use a name and a hyphen \(-\) to specify a fuzzy match.

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/logsearch/$\{LogstoreName\}/?keyFilter=%7B"logstore":%5B"aegis-","500osslog"%5D,"savedsearch":%5B"oss"%5D,"dashboard":%5B"ddos"%5D%7D 
    ```

    ![Filter the resources that are listed in the left-side navigation pane](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6387470951/p37610.png)

-   To specify a time range of a query, use the following URL:

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/logsearch/$\{LogstoreName\}/?queryTimeType=2
    ```

    ![queryTimeType](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6387470951/p37611.png)


## Parameters of the Search & Analysis page

The following table describes the parameters that you can set to customize the Search & Analysis page of a Logstore.

|Parameter|Type|Required|Description|Example|
|:--------|:---|--------|:----------|-------|
|ProjectName|string|Yes|The name of the project.|None|
|LogstoreName|string|Yes|The name of the Logstore.|None|
|queryString|string|No|The string that is used for a query.|queryString=$\{A Base64-encoded SQL statement\}|
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

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/logsearch/$\{LogstoreName\}?queryString=${A Base64-encoded SQL statement}
    ```

-   To hide the buttons such as the Edit and Alerts buttons, use the following URL:

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/logsearch/$\{LogstoreName\}?readOnly=true
    ```

    ![Set the readOnly parameter](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6387470951/p37613.png)

-   To hide the Data Transformation button, use the following URL:

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/logsearch/$\{LogstoreName\}?hiddenEtl=true
    ```

    ![Hide the Data Transformation button](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2651520061/p120917.png)


## Parameters on the saved search page of a Logstore

The following table describes the parameters that you can set to customize the saved search page of a Logstore.

|Parameter|Type|Required|Description|Example|
|:--------|:---|--------|:----------|:------|
|ProjectName|string|Yes|The name of the project.|None|
|savedSearchName|string|Yes|The name of the saved search.|\{savedSearchName\}|

The following example shows the URL parameters and result:

```
https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/savedsearch/$\{savedSearchName\}
```

![Saved search page](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2651520061/p120924.png)

## Embedded parameters of dashboards

The following table describes the embedded parameters that you can set to customize dashboard pages.

|Parameter|Type|Required|Description|Example|
|:--------|:---|--------|:----------|:------|
|ProjectName|string|Yes|The name of the project.|None|
|dashboardName|string|Yes|The name of the dashboard.|None|
|filters|string|No|The filter conditions. The value of this parameter must be transcoded. For example, the `encodeURIComponent('filters=key1:value1&filters=key2:value2')` function includes a string of filter conditions. After you invoke the function, the string is changed to `"filters%3Dkey1%3Avalue1%26filters%3Dkey2%3Avalue2"`.

|encodeURIComponent\('filters=key1:value1&filters=key2:value2'\)|
|token|JsonString|No|The variables to be replaced. The value of this parameter must be transcoded. For example, the `encodeURIComponent('token=[{"key": "projectName","value":"1"}, {"key": "xxx", "value": "yy"}]')` function includes a string of tokens. After you invoke the function, the string is changed to `"token%3D%5B%7B%22key%22%3A%20%22projectName%22%2C%22value%22%3A%221%22%7D%2C%20%7B%22key%22%3A%20%22xxx%22%2C%20%22value%22%3A%20%22yy%22%7D%5D"`.

|encodeURIComponent\('token=\[\{"key": "projectName","value":"1"\}, \{"key": "xxx", "value": "yy"\}\]'\)|
|readOnly|boolean|No|Specifies whether to hide the buttons on the dashboard page, such as the **Edit** and **Alerts** buttons.|readOnly=true|
|hiddenFilter|boolean|No|Specifies whether to hide the filter conditions.|hiddenFilter=true|
|hiddenToken|boolean|No|Specifies whether to hide the variables to be replaced.|hiddenToken=true|
|hiddenProject|boolean|No|Specifies whether to hide the project information.|hiddenProject=true|
|hiddenEdit|boolean|No|Specifies whether to hide the Edit button.|hiddenEdit=true|
|hiddenReport|boolean|No|Specifies whether to hide the Subscribe button.|hiddenReport=true|
|hiddenTitleSetting|boolean|No|Specifies whether to hide the title setting button.|hiddenTitleSetting=true|
|hiddenReset|boolean|No|Specifies whether to hide the Reset Time button.|hiddenReset=true|
|autoFresh|string|No|The interval at which the dashboard is refreshed, for example, 30s or 5m. The minimum refresh interval is 15 seconds.|autoFresh=5m|

The following examples show the URL parameters and results:

-   To hide edit buttons on a dashboard page, use the following URL. In this case, the dashboard page is not editable.

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/dashboard/$\{dashboardId\}/?readOnly=true 
    ```

    ![Read-only dashboards](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7072979951/p37614.png)

-   To add two filter conditions of key 1=value 1 and key 2=value 2, use the following URL.

    After you specify the two filter conditions in the URL, the filter conditions are applied to all charts of the dashboard. The query statements of all charts are executed after log data is filtered based on the filter conditions.

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/dashboard/$\{dashboardId\}?encodeURIComponent('filters=key1:value1&filters=key2:value2') 
    ```

    ![Filter dashboards](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7072979951/p38975.png)

-   To add multiple conditions to replace variables, use the following URL:

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/dashboard/$\{dashboardId\}?encodeURIComponent('token=[{"key": "projectName","value":"1"}, {"key": "xxx", "value": "yy"}]') 
    ```

    ![Specify the token parameter in the URL to replace variables](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7072979951/p38976.png)

-   To refresh a dashboard every 5 minutes, use the following URL:

    ```
    https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/dashboard/$\{dashboard\_name\}?autoFresh=5m
    ```

    ![Automatic refresh](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7072979951/p47224.png)


## Tree structure parameters

The following table describes the tree structure parameters that you can set to customize the left-side navigation pane of the console.

|Parameter|Type|Required|Description|Example|
|---------|----|--------|-----------|-------|
|treeConfig|JSON|No|Specifies the tree structure of the left-side navigation pane. The value of this parameter must be transcoded. For example, the tree structure is `{"logstore":{"expand":true,"resourceList":["delete-log"],"template":["savedsearch","alert"]}}`. After you invoke the transcoding function, the tree structure is changed to `eyJsb2dzdG9yZSI6eyJleHBhbmQiOnRydWUsInJlc291cmNlTGlzdCI6WyJkZWxldGUtbG9nIl0sInRlbXBsYXRlIjpbInNhdmVkc2VhcmNoIiwiYWxlcnQiXX19`.

|treeConfig=eyJsb2dzdG9yZSI6eyJleHBhbmQiOnRydWUsInJlc291cmNlTGlzdCI6WyJkZWxldGUtbG9nIl0sInRlbXBsYXRlIjpbInNhdmVkc2VhcmNoIiwiYWxlcnQiXX19|
|treeEncode|string|No|The encoding method of treeConfig. By default, the value of this parameter is empty. This value indicates that no encoding is performed. Only Base64 is supported.|treeEncode=base64|

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
|logstore|Object|No|Manages Logstores.|
|template|string\[\]|No|Manages the Logstore list. For more information, see [Table 4](#table_4ta_5g9_u9i).|
|machineGroup|Object|No|Manages machine groups.|
|savedSearch|Object|No|Manages saved searches.|
|alert|Object|No|Manages alerts.|
|dashboard|Object|No|Manages dashboards.|
|etl|Object|No|Manages data transformation tasks.|

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

To customize the left-side navigation pane, use the following URL:

```
https://sls4service.console.aliyun.com/lognext/project/$\{ProjectName\}/logsearch/$\{LogstoreName\}?treeconfig=eyJsb2dzdG9yZSI6eyJleHBhbmQiOnRydWUsInJlc291cmNlTGlzdCI6WyJkZWxldGUtbG9nIl0sInRlbXBsYXRlIjpbInNhdmVkc2VhcmNoIiwiYWxlcnQiXX19&hiddenBack=true&hiddenChangeProject=true&hiddenOverview=true&hideTopbar=true&treeEncode=base64&ignoreTabLocalStorage=true
```

![Tree structure](../images/p143404.png)

## Advanced dashboard parameters

If you embed an inline frame into a dashboard, the height of the inline frame cannot be calculated. The scroll bar of the dashboard and the scroll bar of the inline frame may appear at the same time. In this case, you can set advanced dashboard parameters to adjust the dashboard height.

![Advanced dashboard parameters](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2651520061/p120907.png)

To fix this issue, you can use the postMessage method of Log Service to obtain the height of the dashboard and specify the height for the inline frame. The following sample code is used:

```
<! DOCTYPE html>
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

