# Request URL classification function

The request URL classification function classifies a request URL and attaches a tag to the URL. The function also provides the regular expression that defines the pattern of the tag. You can search for URLs by using the tag pattern and then pass the search result to extract, transform, and load \(ETL\) processes.

**Note:** The request URL classification function is available in the China \(Beijing\) and China \(Shanghai\) regions.

-   Function syntax

    ```
    select url_classify(url_path varchar);
    select url_classify(url_path varchar, weight long);
    ```

-   Input parameters

    |Parameter|Description|
    |---------|-----------|
    |url\_path|The URL of the request.|
    |weight|The number of times that the request URL is called.|

-   Output parameters

    |Parameter|Description|
    |---------|-----------|
    |url\_path|The URL of the request.|
    |api\_path|The API endpoint that corresponds to the request URL. The API endpoint is returned by a predefined function.|
    |regex\_tpl|The regular expression that is returned by a predefined algorithm.|

-   Response

    ```
         url_path                          |     api_path                             |         regex_tpl
    -------------------------------------+------------------------------+-------------------------------------
     /gl/balance/666398186799140           | /gl/balance/*                             | \/gl\/balance\/[0-9].+
     /gl/glaccount/30579281472076          | /gl/glaccount/*                         | \/gl\/glaccount\/[0-9].+
     /gl/balance/709016207098025           | /gl/balance/*                            | \/gl\/balance\/[0-9]. +
    ```

-   Example
    -   Enter the following query statement:

        ```
        * | select url_classify(uri, num) from (select uri, COUNT(*) as num from log group by uri limit 1000)
        ```

    -   View the query result.

        ![Request URL classification function](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7905813061/p96745.png)


